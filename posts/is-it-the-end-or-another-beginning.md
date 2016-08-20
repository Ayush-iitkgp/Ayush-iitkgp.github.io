<!-- 
.. title: Is it the end or another beginning ?
.. slug: is-it-the-end-or-another-beginning
.. date: 2016-08-20 02:13:21 UTC+05:30
.. tags: GSoC'16
.. category: 
.. link: 
.. description: 
.. type: text
-->

##[Proposal](http://nbviewer.jupyter.org/github/Ayush-iitkgp/GSoc-Proposal/blob/master/GSoC%202016%20Application%20Ayush%20Pandey-%20Support%20for%20complex%20numbers%20within%20Convex.jl.ipynb) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Blog](https://ayush-iitkgp.github.io/categories/gsoc16/) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Talk](https://ayush-iitkgp.github.io/stories/juliacon-2016-talk/) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Presentation](https://ayush-iitkgp.github.io/stories/juliacon.slides.html/) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Github](https://github.com/Ayush-iitkgp/Convex.jl/tree/gsoc2)

It hasn't been long when I was hugging my friends, calling my parents to inform them of my selection to Google Summer of Code, 2016 program and now here I am writing a wrap-up post for the program. I remember wanting to spend my summers working on the project that involved lots of mathematics, some of the computer science and travel as part of the project. I am so thankful to Google Summer of Code, 2016 program and The Julia Language to have made my each and every wish come true. The thanksgiving note would be incomplete without the mention of my mentors Madeleine and Dj who always helped me whenever I felt like giving up.

Now, coming to the technical aspects of my project, it was divide in 3 phases based on the branches of convex programming namely:

###1. Support for complex-domain linear programs

During this phase of the project, I extended the present implementation in Convex.jl to provide support for linear programs involving complex variables and the complex coefficients. The technical details of the implementation are described in the blog post [here](https://ayush-iitkgp.github.io/posts/announcing-support-for-complex-domain-linear-programs-in-convexjl/).

###2. Support for second order conic programs

In this phase of the project, I in consulation with my mentors, rewrote many the second order cone atoms such as abs, norm to accept complex arguments. We also had an intense discussion on whether redefining the above atoms to accept complex arguments would violate DCP compliance and we came to a conclusion that defining inverse or the square atoms on complex variables neither makes sense nor does it preserve the DCP compliance.

###3. Support for Complex Semidefinite programs

The above 2 phases were relatively difficult for us as we had no literature references and the decision we made were solely based on our understanding and intuition. During this phase, we used the mapping in the introductory section of the research paper [here](http://arxiv.org/pdf/1007.2905v2.pdf) to transform a complex semidefinite program to the corresponding real semidefinite program. Presently, I am writing test cases to check the correctness of our implementation.

The exhaustive list of commits can be found [here](https://github.com/Ayush-iitkgp/Convex.jl/commits/gsoc2).

I am glad to have been successfully implemented what I proposed. Presently, I am also writing the documentation and examples to demonstrate the usability of my implementation. The project will culminate with a single pull request to the Convex.jl repository as well as the release of a new version of Convex.jl which we plan to do in next few days. 

## References

[1].[Approximation algorithms for MAX-3-CUT and other problems via complex semidefinite programming](http://www.sciencedirect.com/science/article/pii/S0022000003001454)

[2].[Invariant semidefinite programs](http://arxiv.org/pdf/1007.2905v2.pdf)

[3].[Convex Optimization in Julia](http://arxiv.org/pdf/1410.4821.pdf)

[4].[Support for complex variables](https://github.com/JuliaOpt/Convex.jl/issues/103)

[5].[Add complex variables](https://github.com/cvxgrp/cvxpy/issues/191)

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