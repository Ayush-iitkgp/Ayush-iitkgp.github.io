<!-- 
.. title: Applying Machine Learning in Ad Tech
.. slug: applying-machine-learning-in-adtech
.. date: 2019-04-08 02:13:21 UTC+05:30
.. tags: Machine Learning, Data Science, AdTech
.. category: 
.. link: 
.. description: 
.. type: text
-->

# Introduction

In recent years, programmatic advertising is been taking over the online advertisement industry. Many companies referred to as DSP (Demand Side Platform) compete for the same ad-slot on the internet. The success of the DSPs to deliver the values to the advertizers is evaluated on the below two criteria:

1. High Click Through Rate = Number of clicks/ Number of ads shown
2. High Conversion Rate = Number of conversions (such as purchase)/ Number of ads shown

To achieve high CTR and Conversion Rates, DSPs havily rely on using Artificial Intelligence techniques and develop their in-house Machine Learning algorithms. The problem of applying Machine Learning in Adtech is different from the standard problem in many sense. In the reamining blog post, I will discuss the differences and how we tackle it in the production:

* **Large size of the training vector:** Every feature in the ML moldel is a categorical feature and encoding them into numerical feature explodes the size of the training vector to the order of billions. For example, one of the most important features in the ML model is the *publisher* website where the ad would be displayed which is a categorical feature and there are millions of publishers so using one-hot encoding would result in a training vector of million entries. 

* **Skewness of the training data:** Typically, CTRs are much lower than 50% (generally, CTR is around 1-2%)which means that positive examples (clicks) are relatively rare, hence is the problem of skewness in the training data is introduced. 

* **Rapid changes in the online ad landscape:**

* **Per-coordinate learning rate:**

* **Using Progressive Validation rather than cross-validation:**

* **Segmented Visualization:**

DSPs develop their own machine learning algorithms using techniques such as [hashing trick](https://en.wikipedia.org/wiki/Feature_hashing), feature combinations, stochastic gradient descent etc.


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
