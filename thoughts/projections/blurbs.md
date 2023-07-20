Projections, not Estimates
--------------------------

---

As a basic anecdote, take something as simple as travel. Erin used to facepaint (~~covid killed that~~), she'd know how long it takes to get to the place, how long the event was, how long it takes to clean and pack up, travel time back and add a bit of a buffer, and she'd still very regularly be off by at least 30+min (for a ~2-6h day). She did that for years, and even in those cases where so much was known, her estimates never really improved. Instead, she got to the point where she'd just say "I paint until X and google says it takes Y for one way."

Then, consider software. You work in a team of multiple people on something with many components and the ambiguity is substantially higher. The goal is to solve problems that generally have not been solved before. How do you produce a meaningful estimate in such conditions?

---

The goal of software in general is to produce things that have value, things that our customers say are great. With that in mind, let's ask a few questions:

* How can we improve the chances of deliverables being great?
    * How does _knowing_ (not guessing!) when it will be done factor in to the result being great?
* How much work does it take before we have enough understanding?
    * Is the effort spent on producing a _date for a date_ more worthwhile than working on the project itself?
* How can you prove the estimates helped you make a good decision?
    * and free of confirmation bias -- we value information that agrees with our beliefs over opinions and data that disagree
* Can "better estimates" be the way forward? May there be alternatives?
    * Is the _problem_ the bad estimates, or are they a symptom?
* If the things that matter cannot be measured (project value), should we instead allow things that can be measured to become what matters?
    * Is _on time and on budget_ a good measure of the results of our decisions? Of our project?
        * (feature cuts, working late/crunch)

---

> The object isn't to make art, it's to be in that wonderful state which makes art inevitable.
>
> -Robert Henri; Painter

---

5 whys?  
Why estimate?
* To have an idea of when the work will be done.

Why do we need to know when the work is done?
* To inform our customers. To manage their expectations.

Why do they need to know that? What will they do with that information? What happens when you deliver early? What happens when you deliver late?
* ???

---

* estimates are used in order to make decisions
    * should we do A or B? should we do this project at all? cut a feature? what should we do first? do we need to hire? do we need to _fire_?
* we believe our knowledge about the time/cost inform these estimates, which are used for decision making
    * the decision making process _requires_ this information
    * would this be required if we had a different way of making decisions?
    * make decisions based on guesses -- fine

---

> People love to say, _We've always done it this way._ I try to fight that.
>
> -Rear Admiral Grace Hopper;

---

My answer to can I get a date would depend on the data available, and I tend to view the vast majority of past "similar" work to have no impact or relation to current work. Current work is always novel, otherwise it would have already been done and we would not be doing it. So, if you ask for a date before any real work has started, I will say I don't know yet. If you ask after some time, after some tasks have been completed and we start to get a better picture of what it is we are doing, no problem, do some maths and project the result. Using which, decisions can be made on things like scope, concurrency, etc. You can even project the impact of such changes. Reducing scope lowers the number of tasks and slope proportionally, but completion rate stays the same. Increasing concurrency increases the completion rate by Amdahl's law.

I could go on about cognitive biases and invalid environments at a later date too, if you want. Point is, I don't give estimates because I acknowledge bias and refuse to perpetuate it, and would rather encourage decision making based on data.

---
