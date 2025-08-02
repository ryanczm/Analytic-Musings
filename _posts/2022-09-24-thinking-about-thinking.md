---
layout: post
title: Thinking About Thinking (Metacognition)
category: productivity
---

In this post, I explore the notion that we can observe experts in different fields and distill their underlying patterns of thought via the right syntax in order to adapt the "optimal style of thought" for that field (e.g natural sciences, computation, humanities).
<!--more-->

A _field_ is just an area of study. My idea is that understanding complexity is about linking things together, but each field has different _things_, and each thing has it's own type of _link_. As we link things together, we can build a clearer visualisation in our head.

## The Right Words
I posit how to think about these _things_ and _links_ can be captured by invoking the right _word_ or _syntax_.

> We have just seen, through an example, how important words are in Mathematics. One can hardly believe how a well-chosen word can provide economy of thought.

<center>
<img src="{{ site.imageurl }}/Metacognition/henri-poincare.jpg" style="width:50%;"/>
<figcaption>Henri Poincaré 1854-1912</figcaption>
</center>

Thus, by _observing_ experts in respective fields, like a scientist, mathematician, programmer, investor being interviewed,  and examining their _choice of words_, we can capture how they think. Sounds simple enough! 
## Fields

<p>I categorize fields (subjects of study) into three types: deterministic, semi-deterministic, and humanistic.</p>
<img src="{{ site.imageurl }}Metacognition/Page1B.png" height=350 class="center"> 

<p><b>Deterministic</b> fields are the sciences and their derived fields. Physics, mathematics, etc. These have fixed laws (of nature). </p>

<p><b>Semi-deterministic</b> fields just refers to computing: it has fixed laws that are modifiable by humans (e.g we make the laws via telling the computer what to do via programming).</p>

<p><b>Humanistic</b> fields refers to fields studying human behavior: economics, finance, history, politics, etc. The <i>laws</i> arise due to human behavior.</p>

I argue each field can be thought of in two ways: a <i>primal form</i>, involving linking things together as a <i>graph</i>, and a <i>dual</i> form, which is basically <i>visualising</i> something in your head. 

So, let's try to find the right words to express the _things_ and their _links_.

A `textual representation` is how the concept appears on text, or on paper. Aka how we learn. An `innate representation` is what we need to connect the textual representation to.

<h2>Deterministic Framework</h2>

<p>Example Fields: Mathematics, natural sciences, engineering.</p>

There are two fundamental things: <i>concepts (abstraction)</i> and <i>physical objects (physical variable/parameter)</i>. On the underlying side, they are just numbers, quantities that can vary, basically. But what they represent are two different things. Their textual representation is a `symbol` and innate representation is a `visual`. 

A concept (more towards the field of mathematics) and a variable/parameter (more towards the field of physics) are the two main players. The difference is in their treatment: both can be visualised, but the former exists in abstract space whereas the latter exists in physical space (e.g our physical reality). 

And so we can connect concepts together or variables together. But for the latter, we have to visualise them precisely or build a visual simulation. 

For physical variables, we can use these measurements to visualise from a perspective. E.g put yourself in the perspective of a proton in an nucleus (it sounds silly but maybe this works). Physics formalises this: aka how to simulate or think about this 'correctly' and 'accurately'. This idea of visual perspective has an interesting cross-reference to the humanistic framework (down below).

Another idea is that of _dependency_, whereby to understand or make connections of certain concepts, there must be a backlog graph of existing concepts that you have to build in sequential fashion, aka prerequisites.

<p>They are linked via connections. These can be direct or latent. Direct connections are obvious. Latent ones are sudden inspirations between seemingly unrelated things. The idea is to build up a dense, well-connected network of things in your head.</p>

So:

1. Connect concepts/ideas/objects
2. Connect or visualise variables
3. Connect or visualise numbers

<blockquote>
<p>There are moments where you <i>put something together</i> and realize this is how the story has to go.</p>
</blockquote>
<figcaption>—Jacob Lurie, <cite><a href="https://www.youtube.com/watch?v=r_gCOs6vLzE&ab_channel=InstitutdesHautes%C3%89tudesScientifiques%28IH%C3%89S%29&t=135">Breakthrough Prize Acceptance (2:15)</a></cite></figcaption>
<blockquote>
<p> There is a beauty in the way things <i>fit together in an unexpected way</i>.</p>
</blockquote>
<figcaption>—Richard Taylor, <cite><a href="https://www.youtube.com/watch?v=r_gCOs6vLzE&ab_channel=InstitutdesHautes%C3%89tudesScientifiques%28IH%C3%89S%29&t=120">Breakthrough Prize Acceptance (2:00)</a></cite></figcaption>
<p>Notice their choice of words: they imply connecting things together. What about the other type of <i>thing</i> aka measurements? </p>
<blockquote>
<p>Atoms in the coffee jiggle, which makes the cup jiggle. Heat is just jiggling spreading, which is easy to understand.</p>
</blockquote>
<figcaption>—Richard Feynman, <cite><a href="https://www.youtube.com/watch?v=P1ww1IXRfTA&ab_channel=ChristopherSykes&t=80">Fun to Imagine (1:20)</a></cite></figcaption>
<blockquote>
<p> It's a mixture of partial solving of equations ... and having some sort of <i>picture</i> of what's happening that the equations saying.</p>
</blockquote>
<figcaption>—Richard Feynman, <cite><a href="https://www.youtube.com/watch?v=P1ww1IXRfTA&ab_channel=ChristopherSykes&t=3378">Fun to Imagine (56:18)</a></cite></figcaption>
<p>Feynman is imagining a measurement (the jiggling) and showing how that interacts with another measurement (another bunch of atoms jiggling). </p>

Key prompts - "Connections", "Visualisation"

<br> 

## Semideterministic Framework

For semi-deterministic fields, which is essentially computing, I believe it is about breaking down a system into its parts from a top-down fashion or build complex systems from simple parts in a recursive fashion. 

The idea is input and output. So data gets put as input and transformed as output. The transformation or connection is the operation. You start from input data and ask what your desired output data is then piece together the intermediate steps.

And as Knuth says below, we need to see something at lots of levels of abstraction and go between them smoothly. So identify these abstraction layers of tools as granular or rich as you possibly can such.

<blockquote>
Input → system (computation) → output. This is my core paradigm for understanding anything.
</blockquote>
<figcaption>—George Hotz, <cite><a href="https://www.youtube.com/watch?v=N2bXEUSAiTI&ab_channel=georgehotzarchive&t=3240">What is Programming? (Noob Lessons!) (54:00)</a></cite></figcaption>
<blockquote>
There's a relatively good understanding of <i>abstraction layers</i>. Atoms, silicon, transistors, logic gates, functional units, processing elements, instruction sets, languages - abstraction layers from the atom to the datacenter. 
</blockquote>
<figcaption>—Jim Keller, <cite><a href="https://www.youtube.com/watch?v=Nb2tebYAaOA&ab_channel=LexFridman&t=250">Jim Keller | Lex Fridman Podcast #70 (4:10)</a></cite></figcaption>
<blockquote>
Being able to see something at lots of levels and <i>go between them smoothly</i> seems to be more pronodunced in people that resonate with computing
</blockquote>
<figcaption>—Donald Knuth, <cite><a href="https://www.youtube.com/watch?v=2BdBfsXbST8&ab_channel=LexFridman&t=553">Donald Knuth | Lex Fridman Podcast #62 (9:13)</a></cite></figcaption>

Key prompts - "How this works", "Under the hood"

<br>
<h2>Humanistic Framework</h2>

These fields encompass markets, finance, economics, history, etc, any kind of field where the aim is to model decisions made by actor's. A node is an actor's decision. 

First, we start by asking 'who'. Note you can visualise the actor or people involved here, a handy trick. An actor can be a person, or a collection or persons.

We can connect actor's decisions together via edges. One actor can have multiple nodes, since an actor can have multiplier behaviours across different contexts! And you can connect one actors behavior (node) to another actors behavior (another node)!

The connection is done via asking 'why'?. Why does an actor behave this way? There is always a reason, and that is the connection.

So, the key skill is to understand a graph of multiple actors decision making, all at once, and see how it relates to each other. From there, we can understand how organisations emerge, laws and policies, situations, etc. etc.

<blockquote>
<p>My framework is built on two propositions. The first is that in situations that have thinking <i>participants</i>, the participants’ <i>views</i> of the world never perfectly correspond to the actual state of affairs.
The second proposition is that these imperfect views can influence the situation to which they relate through the <i>actions</i> of the participants.</p>
</blockquote>
<figcaption>—George Soros, <cite><a href="https://www.georgesoros.com/2014/01/13/fallibility-reflexivity-and-the-human-uncertainty-principle-2/">Fallibility, Reflexivity, and the Human Uncertainty Principle</a></cite></figcaption>

Key prompts - "From the perspective of xx"

<h2>Visualisation: Dual form</h2>

We have covered the primal form or words. Now for the dual form of visualising. How do we paint a picture/simulation in our head corresponding to the linked things?

For deterministic fields, we have to precisely visualise things. This means lengths, angles, distances matter.

For semideterministic fields, we visualise computations together. This means we only care about seeing how the inputs and outputs get produced or made or changed.

For humanistic fields, we visualise the network of actions of participants in a chain-reaction sort of manner.

<h2>Conclusion </h2>

Clearly, complexity or understanding it is about linking things together and having a good mental picture of them. But different discpilines have different ways to approach this. By observing interviews of experts and their thought process, we can put the right words to them and try to adapt them so we can learn faster.