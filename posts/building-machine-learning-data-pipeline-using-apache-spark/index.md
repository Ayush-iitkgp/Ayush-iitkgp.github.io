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

[Apache Spark]() is increasingly becoming popular in the field of Data Sciences because of its ability to deal with the huge datasets and the capability to  run computations in memory which is particularly useful in iterative tasks such as th training step of th Machine Learning algorithm. As part of the Data Engine team at [Sprinklr]() I had some experince working with building data processing pipeline in Spark. In this blogpost, I will try to summarize my learning in simpler, easy to understand terms along with the python code.  

**Q. Why is Apache Spark a suitable tool for building the data pipeline?**

**Ans.** Few years ago, [scikit-learn]() came up with the idea of data pipeline but with the advent of big data, it became very problematic to scale. Spark's data pipeline concept is mostly inspired by the scikit-learn project. It provides the APIs for machine learning algorithms which make it easier to combine multiple algorithms into a single pipeline, or workflow.

Now, I will introduce the key concepts used in the Pipelines API:

**DataFrame:** This is conceptually equivalent to a dataframe in R/Python. It can different data types such a string, vectors, strue labels, and predictions.

**Transformer:** A Transformer is an algorithm which can transform one DataFrame into another DataFrame. E.g., an ML model is a Transformer which transforms a DataFrame with features into a DataFrame with predictions.

**Estimator:** An Estimator is an algorithm which can be fit on a DataFrame to produce a Transformer. E.g., a learning algorithm is an Estimator which trains on a DataFrame and produces a model.

**Pipeline:** A Pipeline chains multiple Transformers and Estimators together to specify an ML workflow.

**Parameter:** All Transformers and Estimators now share a common API for specifying parameters.


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
