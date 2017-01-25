<!-- 
.. title: One-Class Classification Algorithms
.. slug: one-class-classification-algorithms
.. date: 2017-01-22 12:13:21 UTC+05:30
.. tags: Machine Learning, Application of Novelty Detection Algorithms to predict Electricity Theft
.. category: 
.. link: 
.. description: 
.. type: text
-->

In my [previous blog entry](/posts/when-does-traditional-classification-algorithm-fail/), I have put forth a case when the traditional classification algorithms do not perform well i.e. when the training data has reasonable level of balance between the various classes. The [problem statement](https://drive.google.com/file/d/0B2oOdWdSJWa1NF8ySXhvR1ZvODA/view?usp=sharing) that I was trying to solve had a similar problem of the skewed distribution of the training data. **The power company had only created the database of fraudulent customers**. 

As I mentioned in my previous blog post, there are 2 methods to tackle the case when we have data from only one class:

1. The first method is an obvious approach to generate an artificial second class and proceed with traditional classification algorithms.

2. The second one is to modify the existing classification algoriths to learn on the data from only one class. These algorithms are called "one-class classfication algorithms" and include one-class SVM, One-Class K-Means, One-Class K-Nearest Neighbor, and One-Class Gaussian.

In this blog entry, I will elaborate on the second method and explain the mathematics behind the one-class classification algorithms and how it improves over the traditional classification algorithms. 

## Fundamental difference between Binary and One Class Classification Algorithms

Binary classification algorithms are *discriminatory in nature*, since they learn to discriminate between classes using all data classes to create a hyperplane(as seen in the fig. below) and use the hyperplane to label a new sample. In case of imbalance between the classes, the discriminatory methods can not be used to their full potential, since by their very nature, they rely on data from all classes to build the hyperplane that separate the various classes.

<center><img src="/images/binaryClassification.png" alt="Binary Classification Hyperplane" height="200px" width="375px" border="1px" style="margin: 0px 20px">Binary Classification Hyperplane</center><br/>

Where as the one-class algorithms are based on *recognition* since their aim is to recognize data from a particular class, and reject data from all other classes. This is accomplished by creating a boundary that encompasses all the data belonging to the target class within itself, so when a new sample arrives the algorithm only has to check whether it liew within the boundary or outside and accordingly classify the sample as belonging to the outlier class or the outlier.

<center><img src="/images/oneClassClassification.png" alt="One-Class Classification Boundary" height="200px" width="375px" border="1px" style="margin: 0px 20px">One-Class Classification Boundary</center><br/>


Thank you very much for making it this far.











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