<!-- 
.. title: A month of Excitement!
.. slug: a-month-of-excitement
.. date: 2016-07-22 21:44:47 UTC+05:30
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

**Every day is a new experience and it opening up new opportunities. Life is awesome**. 

That is how I would describe my first month as GSoCer. From meeting new like minded people from all across the world to having an intense discussion about our projects in different groups to getting excited about receiving google goodies and now finally getting invited to JuliaCon'16 to be held at MIT, Cambridge, Massachusetts. Every experience has been worth the work put behind getting selected into GSoC'16.

I got into coding my project and community bonding as soon as my exams got over on 29th April. Before I get into the details of my project, I would like to extend this opportunity to explain how my project is of importance to the optimization community in particular. The reason is as follows: 

Many problems in applied sciences are posed as optimization problems over the complex field such as:

* Phase retrieval from sparse signals
* Designing an FIR filter given desired frequency response
* Optimization problems in AC power systems
* Frequency domain analysis in signal processing and control theory.

The present approach is to manually convert the complex-domain problems to real-domain problems [(example)](http://nbviewer.jupyter.org/github/cvxgrp/cvxpy/blob/master/examples/notebooks/WWW/fir_chebychev_design.ipynb) and pass to solvers. This process can be time-consuming and non-intuitive sometimes. The correct approach to such problem would be to make existing packages deal with complex-domain optimization hence making it easier for the optimization community to deal with complex-domain problems.

In the first hangout call with my mentors, we had decided to implement the functionality to support the Linear complex-domain optimization problem in Convex.jl. In order to support the above functionality, I was required to make the following changes:

1. Add support for complex variables, Hermitian SemiDefinite matrix and complex constants. This was done by introducing a new sign `ComplexSign` which was introduced as the subtype of the user-defined type `Sign` in dcp.jl file. I also went on to define the new rules for basic rules on interactions of mathematical expressions with the mathematical expression with `ComplexSign`

        abstract Sign
        type Positive <: Sign                   end
        type Negative <: Sign                   end
        type NoSign <: Sign                     end
        type ComplexSign <: Sign                end

        -(s::ComplexSign) = ComplexSign()
        +(s::ComplexSign, t::ComplexSign) = s
        +(s::Sign, t::ComplexSign) = t
        +(t::ComplexSign, s::Sign) = s+t
        *(t::ComplexSign, s::ComplexSign) = t
        *(t::ComplexSign, s::Sign) = t
        *(s::Sign, t::ComplexSign) = t
        *(s::ComplexSign, m::Monotonicity) = NoMonotonicity()
        *(m::Monotonicity, s::Sign) = s * m
        *(s::ComplexSign, v::ConstVexity) = v
        *(s::ComplexSign, v::AffineVexity) = v
        *(s::ComplexSign, v::ConvexVexity) = NotDcp()
        *(s::ComplexSign, v::ConcaveVexity) = NotDcp()
        *(s::ComplexSign, v::NotDcp) = v


 
2. As a result of changes made in point 1, I was able to modify the constant.jl and variable.jl file accordingly.

        ComplexVariable(m::Int, n::Int, sets::Symbol...) = Variable((m,n), ComplexSign(), sets...)
        ComplexVariable(sets::Symbol...) = Variable((1, 1), ComplexSign(), sets...)
        ComplexVariable(size::Tuple{Int, Int}, sets::Symbol...) = Variable(size, ComplexSign(), sets...)
        ComplexVariable(size::Int, sets::Symbol...) = Variable((size, 1), ComplexSign(), sets...)
        HermitianSemidefinite(m::Integer) = ComplexVariable((m,m), :Semidefinite)
        function HermitianSemidefinite(m::Integer, n::Integer)
          if m==n
            return ComplexVariable((m,m), :Semidefinite)
          else
            error("HermitianSemidefinite matrices must be square")
          end
        end

 Now, the user can create the complex variables, Hermitian-semidefinite matrix in Convex.jl as follows:

        y = ComplexVariable()
        Variable of
        size: (1, 1)
        sign: Convex.ComplexSign()
        vexity: Convex.AffineVexity()
        z = HermitianSemidefinite(4)
        Variable of
        size: (4, 4)
        sign: Convex.ComplexSign()
        vexity: Convex.AffineVexity()

3. The third step was to redefine the [affine](https://github.com/Ayush-iitkgp/Convex.jl/tree/gsoc1/src/atoms/affine) atoms in Convex.jl to accommodate the complex variable case as well. Interestingly, by virtue of the rules defined in step 1 in dcp.jl, we didn't even need to define any affine atom to accommodate the complex case, Convex.jl is smart enough to pick the sign of the expression using rules in dcp.jl. 

So, this was the summary of what I have been doing on the coding side of GSoC. On the other side, I moved to Bangalore (India's Silicon Valley) where I would be spending next two months and I am very excited to meet the other JuliaSoCers at the Julia Computing Bangalore office in the days to come.
 
Also, the coming week is crucial for us as we have decided to fully implement the complex-domain Linear Programming Problem in Convex.jl. Last night, we had our 3rd hangout meeting where we discussed the nitty-gritty of Convex.jl especially the conic form functionality and how to proceed so as to meet the deadline that we have set up for ourselves. I look forward to another week of coding and making new friends and getting involved in the Open Source Community.

**The Bragging Rights- Convex.jl would be the second optimization package after Matlab's cvx to support complex-domain optimization problems, so yes this excites me as well to be a part the team that is developing such a useful package.**

Also, I am very very much excited to attend JuliaCon'16 at the Massachusetts Institute of Technology and give a talk on what I have been doing this summer. I hope I get the visa on time, wish me luck everyone :)

See you next time till then keep laughing, spreading happiness and the most importantly living this journey called life!!!! 


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
