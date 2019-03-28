<!-- 
.. title: Click Through Rate Analysis using Spark
.. slug: ctr-analysis-using-spark
.. date: 2019-02-21 02:13:21 UTC+05:30
.. tags: Machine Learning, Data Science
.. category: 
.. link: 
.. description: 
.. type: text
-->

# Introduction

In recent years, programmatic advertising is been taking over the online advertisement industry. To enable automatic selling and purchasing ad impressions between advertisers and publishers through real-time auctions, Real-Time Bidding (RTB) is quickly becoming the leading method.

In contrast to the traditional online ad market, where a certain amount of impressions is sold at a fixed rate, RTB allows advertisers to bid each impression individually in real time at a cost based on impression-level features. Real-time Bidding (RTB) is a way of transacting media that allows an individual ad impression to be put up for bid in real-time. This is done through a programmatic **on-the-spot auction**, which is similar to how financial markets operate. RTB allows for Addressable Advertising; the ability to serve ads to consumers directly based on their demographic, psychographic, or behavioral attributes.

Many DSPs (Demand Side Platforms) act as agents for the advertisers and take part in the real-time auction on behalf of them. In order to enable real-time bidding and provide the advertisers with the clicks at the lowest price possible, DSPs develop their own machine learning algorithms using techniques such as [hashing trick](https://en.wikipedia.org/wiki/Feature_hashing), feature combinations, stochastic gradient descent etc.

# Motivation

Like the standard practice in most of the data science use cases, whenever a new algorithm is developed, they are put into A/B test against the already existing algorithm in the production (at least for few days) in order to do determine which algorithm suits the business metrics better. 

Due to the huge volume of bid requests (around a million bid requests per second), the amount of data collected during AB is in the order of 100 GBs. Python's Pandas library has basically all the functionality needed to do the offline analysis of the data collected in terms of CPCs, spend, clicks, CTR, AUC etc. 

But, Pandas has a huge problem, it has to load all the dataset in memory in order to run some computations on it. From my experience, **Pandas needs the RAM size to be 3 times the size of the dataset** and it can not be run into a distributed environment as cluster a of machines. This is where [Apache Spark](https://spark.apache.org/) is useful as it can process the datasets whose size is more than the size of the RAM. This blog will not cover the internals of Apache Spark and how it works rather I will jump to how the Pandas CTR Analysis code can be easily converted into spark analysis with few syntax changes.


# Migrating to Spark from Pandas

In new versions, Spark started to support Dataframes which is conceptually equivalent to a dataframe in R/Python. Dataframe support in Spark has made it comparatively easy for users to switch to Spark from Pandas using a very similar syntax. In this section, I would jump to coding and show how the CTR analysis that is done in Pandas can be migrated to Spark. 

Before I jump into the coding, I would like to introduce some of the keywords used in the code:

*Effective CPC*: Total money spent / Total number of clicks

*Label*: It is either 0 or 1 (1 signifies that the click happened and 0 is for no click)

*Win Price*: The price paid to win the on-spot auction

*Bid CPC*: The price the advertiser is willing to pay for the impression

*CTR*: Click Through Rate = Total Number of Clicks / Total Number of Impressions

**How is Win Price different from Bid Price?**

If an exchange is using [First Price Auction](https://en.wikipedia.org/wiki/First-price_sealed-bid_auction), the win pice and the bid price is same but if the exchange is using [Second Price Auction](https://en.wikipedia.org/wiki/Generalized_second-price_auction), the advertizer with the highest bid price wins but it pays the price equivalent to the second highest bid price hence the win price is less than the bid price.

## **Setting up notebook and importing libraries**

#### **Pandas**

	import pandas as pd

#### **Spark**

	import os
	import sys
	os.environ['SPARK_HOME'] = "/home/spark-2.3.2-bin-hadoop2.7"
	os.environ['JAVA_HOME'] = "/home/jdk1.8.0_181"
	spark_home = os.environ.get("SPARK_HOME")
	sys.path.insert(0, spark_home + "/python")
	sys.path.insert(0, os.path.join(spark_home, "python/lib/py4j-0.10.7-src.zip"))

	from  pyspark import SparkContext
	from pyspark.conf import SparkConf
	CLUSTER_URL = "spark://address:7077"
	conf = SparkConf()
	conf.setMaster(CLUSTER_URL).setAppName("CTR Analysis").set("spark.executor.memory", "120g")
	sc = SparkContext(conf=conf)
	print(sc.version)

## **Reading CSV File**

#### **Pandas**

	df = pd.read_csv(data_file_path, names=cols, error_bad_lines=False, warn_bad_lines=True, sep=',')

#### **Spark**

	from pyspark.sql.types import StructType, StructField
	from pyspark.sql.types import DoubleType, IntegerType, StringType
	file_location = "hdfs://address:port/hadoop/dataNode/pyspark/data.csv"
	df = spark.read.csv(file_location, header=False, schema=schema, sep=",")
	# df = spark.read.csv(file_location, header=False, inferSchema=True, sep=",")
	# df.cache()

## **Cleaning data**

#### **Pandas**

	numeric_cols = ['label','win_price','ctr','bid_cpc']
	df[numeric_cols] = df[numeric_cols].convert_objects(convert_numeric=True)
	df.dropna(inplace=True, subset=numeric_cols)


#### **Spark**

	numeric_cols = ['label','win_price','ctr','bid_cpc']
	df = df.dropna(subset=numeric_cols)


## **Calculating Spend, CTR, CPC Per Algo**

#### **Pandas**

	data = df.groupby(['algo']).agg({'win_price': np.sum, c_label:{'clicks':np.sum, 'wins':'count'}}).reset_index()
    data[('win_price','sum')] = data[('win_price','sum')] / 1000.
    data['ecpc'] = data[('win_price','sum')] / data[('label,'clicks')]
    data = pd.DataFrame(data.to_records())
    data.columns = ['',algo, 'spend', 'number of impressions', 'number of clicks', 'effective cpc']


#### **Spark**

	from pyspark.sql.functions import udf
	import pyspark.sql.functions as f

	def divide_by_1000(x):
    	return x/1000.0

	udfdivide_by_1000 = udf(divide_by_1000, DoubleType())

	data_wins = df.groupby(['algo']).agg( {'win_price': 'sum', 'label: 'count'})
    data_clicks = df.groupby(['algo']).agg({'label: 'sum'})
    # print data_wins.columns
    # print data_clicks.columns
    # Rename the columns
    data_wins = data_wins.withColumnRenamed("sum(win_price)", "win_price").withColumnRenamed("count(label)", "wins")
	#     print data_wins.schema
	#     data_wins['win_price'] = data_wins.win_price/1000.
    data_wins = (data_wins.withColumn("win_price",udfdivide_by_1000("win_price")))
    data_clicks = data_clicks.withColumnRenamed("sum(label)", "clicks")
	#     print data_wins.columns
    # print data_clicks.columns
    data = data_wins.join(data_clicks, on = 'algo', how='inner')
    data = (data.withColumn("effective cpc", f.col("win_price")/100))




<div id="disqus_thread"></div>
<script>
/**
* RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
* LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables
*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL; // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
*/
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');

s.src = '//avoyage.disqus.com/embed.js';

s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript" rel="nofollow">comments powered by Disqus.</a></noscript>
