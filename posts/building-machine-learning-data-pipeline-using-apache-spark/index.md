<!-- 
.. title: Building Machine Learning Data Pipeline using Apache Spark
.. slug: building-machine-learning-data-pipeline-using-apache-spark
.. date: 2019-03-20 02:13:21 UTC+05:30
.. tags: Machine Learning, Data Science
.. category: 
.. link: 
.. description: 
.. type: text
-->

# Introduction

[Apache Spark]() is increasingly becoming popular in the field of Data Sciences because of its ability to deal with the huge datasets and the capability to  run computations in memory which is particularly useful in iterative tasks such as the training step of the Machine Learning algorithm. As part of the Data Engine team at [Sprinklr](https://www.sprinklr.com/), I had some experience building the data processing pipeline in Spark. In this blog post, I will try to summarize my learning in simpler, easy to understand terms along with the python code.  

**Q. Why is Apache Spark a suitable tool for building the ML data pipeline?**

**Ans.** Few years ago, [scikit-learn](https://scikit-learn.org/stable/) came up with the idea of data pipeline but with the advent of big data, it became very problematic to scale. Spark's data pipeline concept is mostly inspired by the scikit-learn project. It provides the APIs for machine learning algorithms which make it easier to combine multiple algorithms into a single pipeline, or workflow.

Now, I will introduce the key concepts used in the Pipeline API:

**DataFrame:** It is basically a data structure for storing the data in-memory in a highly efficient way. Dataframe in Spark is conceptually equivalent to a dataframe in R/Python. It can store  different data types such a string, vectors, true labels, and predictions. Dataframes can be created from the csv, json and many different file formats stored on the local filesystem, Hadoop HDFS or cloud environment such as AWS S3.

**Transformer:** It is a method or an algorithm which can transform one DataFrame into another DataFrame. It includes SQL statements, feature transformers and learned ML models. While defining a transformer, you have to specify the column it would operate on and the output column it would append to the input DataFrame. Technically, a Transformer implements a method **transform()**. E.g., a SQL *select* statement which would return a new dataframe with only required columns. Another example is a *trained ML model* which turns a dataframe with feature vectors into a dataframe with predictions.

**Estimator:** It is an algorithm which can be fit on a DataFrame to produce a Transformer.  Technically, an Estimator implements a method **fit()** which accepts a DataFrame and produces a **Model** which is a Transformer. For example, a machine learning algorithm is an Estimator which trains on a DataFrame and produces a trained model which is a transformer as it can transform a feature vector into predictions.

**Parameter:** These are the hyperparameters used during cross-validation phase of the ML pipeline.

**Pipeline:** A Pipeline is a sequence of PipelineStage (Transformers and Estimators)together to be running in a particular order to specify a Machine Learning workflow. **A Pipeline’s stages are specified as an ordered array**. For example predicting the price of a house given it's breadth, length, location and age involve several stages:

* Remove the data points which have all columns as null value - Transformer
* Create a new feature **area** - Transformer
* Learn a prediction model using the feature vectors and the actual price - Estimator
* Use the learned model to predict the prices - Transformer

A Pipeline is an *Estimator* in itself. Thus, after a Pipeline’s fit() method is called, it produces a *PipelineModel*, which is a Transformer. 

PipelineModel has the same number of stages as the original Pipeline, **but all Estimators in the original Pipeline have become Transformers**. This PipelineModel is used at test time. 

Let's dive into the Python code using an example mentioned in the Spark's doc [here](https://spark.apache.org/docs/latest/ml-pipeline.html#example-pipeline) where we are trying to classify a line of text into 0 or 1:

**Step 1: Create a DataFrame**

    # Some import statements
    from pyspark.ml import Pipeline
    from pyspark.ml.classification import LogisticRegression
    from pyspark.ml.feature import HashingTF, Tokenizer

    # DataFrame could be created from a csv file or any other sources 
    training = spark.createDataFrame([
    (0, "a b c d e spark", 1.0),
    (1, "b d", 0.0),
    (2, "spark f g h", 1.0),
    (3, "hadoop mapreduce", 0.0)
    ], ["id", "text", "label"])

    # Let's also create a test dataset which we will use later
    test = spark.createDataFrame([
    (4, "spark i j k"),
    (5, "l m n"),
    (6, "spark hadoop spark"),
    (7, "apache hadoop")
    ], ["id", "text"])


**Step 2: Specify the transformers and the estimators of the pipeline**
    
    # Tokenizer is a transformer which would convert the text column into words using space as a delimeter
    tokenizer = Tokenizer(inputCol="text", outputCol="words")

    # HashingTF is again a transformer which takes the column "words as an input and creates a new column of a vector" 
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")

    # Logistic Regression is an Estimator which would take "features" and "label" as an input and create a trained model
    lr = LogisticRegression(maxIter=10, regParam=0.001)


**Step 3: Create the pipeline using the transformers and the estimators defined in step 2**

    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])


**Step 4: Call the fit() method on the pipeline to create a PipelineModel**

    model = pipeline.fit(training)


**Step 5: Use the PipelineModel to do the predictions of the test dataset**

    prediction = model.transform(test)
    selected = prediction.select("id", "text", "probability", "prediction")
    selected.show()


One of the big benefits of the Machine Learning Data Pipeline in Spark is hyperparameter optimization which I would try to explain in the next blog post. I hope this blog would help you in getting started with Spark for building ML data pipelines. 



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
