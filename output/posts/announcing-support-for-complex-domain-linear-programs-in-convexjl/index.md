<!-- 
.. title: Announcing support for complex-domain linear Programs in Convex.jl
.. slug: announcing-support-for-complex-domain-linear-programs-in-convexjl
.. date: 2016-07-27 02:13:21 UTC+05:30
.. tags: GSoC'16
.. category: 
.. link: 
.. description: 
.. type: text
-->

# Introduction

In recent years, programmatic advertising is been taking over the online advertisement industry. Many companies referred to as DSP (Demand Side Platform) compete for the same ad-slot on the internet. The success of the DSPs to deliver the values to the advertisers is evaluated on the below two criteria:

1. High Click Through Rate = Number of clicks/ Number of ads shown
2. High Conversion Rate = Number of conversions (such as a purchase)/ Number of ads shown

To achieve high CTR and Conversion Rates, DSPs heavily rely on using Artificial Intelligence techniques and develop their in-house Machine Learning algorithms. The problem of applying Machine Learning in Adtech is different from the standard problem in many sense. Google's paper [*Ad Click Prediction: a view from the Trenches*](https://ai.google/research/pubs/pub41159) and Facebook's paper [*Practical Lessons from Predicting Clicks on Ads at Facebbok*](https://research.fb.com/publications/practical-lessons-from-predicting-clicks-on-ads-at-facebook/) have discussed in details the lessons learned while building an AI for the adtech industry. In the remaining blog post, I will try to summarize the intricacies of applying ML in adtech and how it is tackled in general:

* **Large size of the training vector:** Every feature in the ML model is a categorical feature and encoding them into numerical feature explodes the size of the training vector to the order of billions. For example, one of the most important features in the ML model is the *publisher* website where the ad would be displayed which is a categorical feature and there are millions of publishers so using one-hot encoding would result in a training vector of million entries.

* **Skewness of the training data:** Typically, CTRs are much lower than 50% (generally, CTR is around 1-2%)which means that positive examples (clicks) are relatively rare, hence is the problem of skewness in the training data is introduced.

* **Rapid changes in the online ad landscape:** The adtech domain is a very dynamic environment where the data distribution changes over time. Facebook conducted an experiment where they trained the model on one day of data and evaluated on the six consecutive days. The results showed that the performance of the model decreases as the delay between the training and test set increases. Thus, it is very important to update the model every few hours to keep it very real-time

* **Per-coordinate learning rate:** In most of the standard ML problems, the learning rate is a constant. In adtech, there is a huge imbalance of the number of training instances on each feature. For example, a famous publisher such as cnn.com will be having more users thus more ad-slots compared to a very unknown one thus, our training data will have a huge number of training instances for cnn.com. Therefore, we want to decrease the learning rate for the coordinate as its frequency in the training data increases.

* **Using Progressive Validation rather than cross-validation:** Validating the model on the data set which lags the train set by hours or days is not a good idea as we discussed above that the nature of the dataset changes with time. The *online log loss* is instead a good proxy for the model performance because it measures the performance only on the most recent data before we train on it, this is exactly analogous to what happens when the model is in production. This also ensures that we can use 100% of our data for both training and testing.

* **Relative changes in the metric compared to Absolute metric:** Click rates vary from country to country and from ad-slot to ad-slot and therefore the metrics change over the course of a single day. Google experiments indicate that the relative changes(compared to a baseline model) are much more stable over time, therefore, a relative change in logloss is a better metric than the average log loss. We also take care only to compare metrics computed from exactly the same data.

* **Segmented performance metrics instead of aggregated metrics:** One of the things that we have to take care while analyzing the performance of the models in adtech is that the aggregated performance metrics may hide effects that are specific to certain sub-populations of the data. For example, a high CTR may, in fact, be caused by a mix of low and high CTR from different ad exchanges. This makes it critical to visualize the performance metrics not only on the aggregate data but also on the various slicing of the data such as per ad exchange, per adgroup, per device type, per publisher.

This blog is the aggregation of all the lessons learnt while working as Data Scientist in the AdTech company. Do let me know if you have any additional comments. 

Do you have any questions?

Ask your questions in the comments below and I will do my best to answer.

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
