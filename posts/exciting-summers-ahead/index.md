<!-- 
.. title: Exciting Summers Ahead!
.. slug: exciting-summers-ahead
.. date: 2016-07-22 21:38:08 UTC+05:30
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

**Fall seven times, stand up eight !!!!**

Google Summer of Code, the thing I wanted to be a part of as soon as I heard of it in my 1st year of college. It took me 4 years when I am finally selected for the prestigious open source program. Harsh Gupta no word can express my gratefulness for you. Thank you very much for always being there. Coincidently, the results were announced in the middle of my end-semester examination and I must accept my selection was one more reason for not studying for the exams. I am glad that my exams are over and as always I am looking forward to the summers where I apply the theories I have learnt during the semester in a practical world. I think I should make it apparent that applying to Julia and getting selected was a well-thought process, there were series of events which helped me getting selected in GSoC under The Julia Lang. It all started when I was finishing the web-development internship during the winter vacations when I realized that I need to pursue applied mathematics more seriously as that is somewhere I can excel at. **Operations Research and Optimization** was an obvious choice as I enjoyed the introductory course in Linear Programming way more than other cs and mathematics courses that I took during my stay at IIT Kharagpur. I decided to take an advanced course in Optimization(Non-Linear Programming) but sadly the course was open for final year students but yes *where there is will, there is a way*, I was able to enroll for the course after consulting with my professor, interestingly I was the only pre-final year student in that class yet I tried to attend each and every class in spite of not having familiar faces to sit with. During the semester, 4th InterIIT Meet was being held at IIT Mandi. Initially, I was not the part of IIT Kharagpur's contingent as I didn't find myself competent enough to apply for the selections. But as soon as the problem statement for the event Portfolio Optimization was out, my friend Raja Ambrish contacted me as the problem was related to Convex Optimization because he thought I could help him. My eyes were lit bright as soon as I saw the problem statement as it was the application of what I have been reading throughout the semester. I worked with the other team members and was finally selected to present our solution to the jury at IIT Mandi. Yay, IIT Kharagpur were the overall champions and also our team managed to win gold medal Portfolio Optimization as well. 
![A click during InterIIT Presentation](/content/images/2016/05/InterIITTech.jpg)


The InterIIT experience was awesome and it made me realize that indeed I have a special interest in the field of Optimization. I wanted to explore more of this field and Google Summer of Code was an exciting opportunity. I told my friend Harsh about the plans to participate in GSoC in an organization which has Optimization related projects and his first suggestion was [Julia](http://julialang.org/). I came back room and browsed projects and wow Julia has this one project namely `"Support for Complex SemiDefinite Programming within Convex.jl"` on Convex Optimization that I fell in love with. The concepts I learnt during the classroom hours in Non-Linear Programming were of immense help to me in getting started with Convex.jl special thanks to Prof. Geetanjali Panda for teaching this course in a very intuitive way.

Finally, after having an extensive discussion with my mentors [Madeleine Udell](https://people.orie.cornell.edu/mru8/) and [Dvijotham Krishnamurthy](http://www.its.caltech.edu/~dvij/), I submitted my [proposal](http://nbviewer.jupyter.org/github/Ayush-iitkgp/GSoc-Proposal/blob/master/GSoC%202016%20Application%20Ayush%20Pandey-%20Support%20for%20complex%20numbers%20within%20Convex.jl.ipynb) under The Julia Lang and NumFocus and was selected under former. I want to express my gratefulness to each and everyone who has helped me before and throughout the GSoC Application Period.

I am officially in the first week of the Community Bonding Period. My week started with having a video call with my mentors where we discussed my proposal in more detail and went through some of the existing codebase in order to understand what specific changes need to be made to accomplish our task. We have started by first tackling the complex-domain Linear Programming Problems. We have decided that we would need few utility functions like:

* A function to recurse on the expression tree to check if an expression is real or complex
* A function conj(A)
* A function to check the LHS and RHS of each inequality constraint is real.

I would also need to think of the rules that would be helpful in determining whether the combination of two or more expressions is real or complex on the lines of how we determine the DCP compliance of a problem. The next step would be to rewrite all the atoms to accept the complex parameters.

The next assignment was to set-up a blog to write and publish my experience as GSoCer or rather JuliaSoCer which I did just complete :D During this week, I was also introduced to the Julia community and added to their slack channel. I believe I would have some awesome time with the community and hope to see them during JuliaCon 2016.

Interestingly, I have my birthday on 4th May which I plan to celebrate with my family so I would be traveling to my home from my university. Don't forget to wish me ;) 

This is all for now. Thank you very much for making it this far :)


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
