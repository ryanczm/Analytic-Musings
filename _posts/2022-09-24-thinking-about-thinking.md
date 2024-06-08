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

There are two fundamental things: <i>concepts (abstraction)</i> and <i>physical objects (physical variable)</i>. On the underlying side, they are just numbers, quantities that can vary, basically. But what they represent are two different things. Their textual representation is a `symbol` and innate representation is a `visual`. 

<p>They are linked via connections. These can be direct or latent. Direct connections are obvious. Latent ones are sudden inspirations between seemingly unrelated things. The idea is to build up a dense, well-connected network of things in your head.</p>

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

<br> 

## Semideterministic Framework

For semi-deterministic fields, which is computing, I believe it is about breaking down a system into its parts from a top-down fashion. This is the key trick. Look at the system from the highest level, then go down, recursively. So we would have one input > system > output, then you would break it down recursively from there. I'll call this operation _decomposition_.


A _system_ can be broken down into its _components_, recursively (that every _component_ can be viewed as _system_ in itself). _Components_ could be _data_ or _computation_. The nodes are data. A _computation_ arrow is defined by the type(s) of data it receives and type(s) of data it produces. We could even argue an interaction is defined by the type of input it allows and the type of output it generates.

Anyway, the key is to break a system down or have a _graphical view_ of the system, similar to those systems architecture diagrams we see. To do so, you either write it down on paper/screen or have it in your head.

To make connections within the system, we have to be wary of the _types_ of _components_ and _data_, since only certain types of data can be read or processed by certain types of components. 

Another view is _memory view_. This would be visualising on a lower level of abstraction what is going on. In terms of input-system-output, we can abstract a _computation_ as a _copy transformation of memory_: it takes some memory, copies it (or modifies in place), then modifies the structure of memory (or copy).

So to summarize:

1. Think in terms of systems and components. 
2. Graphical thinking: draw out a graphical view of the system. 
3. Memory view: at lower levels, envision what the memory is doing.

Not very elegant, but I think this works for now. I'll be practicing graphical view understanding the codebase at my current role.


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

<br>
<h2>Humanistic Framework</h2>

These fields encompass finance, economics, history, etc. In here, the key node is a composite of a player. A player has a view/context and behaviors stem from it. We draw connections to build a _situation_ to other nodes. A cheap heuristic would be to ask _who_, _what_ and _why_. Their representation is a `player`.

A _situation_ is a complex interplay of different players. The trick is to hold multiple actors in your head and connect their interactions together by asking who and why, _from their perspective_.

Then, we can ask historically how a situation has evolved and how it will play out in the future.

So, the first step in any similar situation is to ask 'who'. Who are the players? What are their actions? Why did they did it? And there 


<blockquote>
<p>My framework is built on two propositions. The first is that in situations that have thinking <i>participants</i>, the participants’ <i>views</i> of the world never perfectly correspond to the actual state of affairs.
The second proposition is that these imperfect views can influence the situation to which they relate through the <i>actions</i> of the participants.</p>
</blockquote>
<figcaption>—George Soros, <cite><a href="https://www.georgesoros.com/2014/01/13/fallibility-reflexivity-and-the-human-uncertainty-principle-2/">Fallibility, Reflexivity, and the Human Uncertainty Principle</a></cite></figcaption>



<h2>Visualisation: Dual form</h2>

We have covered the primal form or words. Now for the dual form of visualising. How do we paint a picture/simulation in our head corresponding to the linked things?

For deterministic fields, we have to precisely visualise things. This means lengths, angles, distances matter.

For semideterministic fields, we visualise computations together. This means we only care about seeing how the inputs and outputs get produced or made or changed.

For humanistic fields, we visualise the network of actions of participants in a chain-reaction sort of manner.

<h2>Conclusion </h2>

Clearly, complexity or understanding it is about linking things together and having a good mental picture of them. But different discplines have different ways to approach this. By observing interviews of experts and their thought process, we can put the right words to them and try to adapt them so we can learn faster.