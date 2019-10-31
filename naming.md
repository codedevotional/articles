# Names Pin the Jell-O to the Wall

Your code and its artifacts are strewn with names. You name repositories, files, packages, classes, functions, variables, and on and on. The names you choose shape the conversations you have, in absentia, with your programmer peers.

On the flip side of that bargain, your understanding of libraries and frameworks, i.e. other people's code, starts with names. Names form the substrate of APIs, delineating their boundaries, clarifying their inputs and outputs, and defining how they're used. If you can trust that a library works, you need look no further than its API docs to understand it. Ideally, API docs are the only necessary point of contact you have with a library. And they are, essentially, a bunch of names.

Just as a writer grinds through word choices, a programmer must struggle with names. As the joke goes, the two hardest things in computer science are naming, cache invalidation, and off-by-one errors. No matter the programming language, no matter whether you're operating from a functional or object-oriented perspective, no matter if the language is statically or dynamically typed, names are a ubiquitous and critical part of your code. Cache invalidation may not matter in your application, but names sure do. The quality of your work depends on the quality of your names.

When coding, you spend a lot of time burrowed in the minutiae of specifics. You ply your craft at the mercy of unbending syntax rules that will bring a million-line codebase to its knees over a single missing character. You write functions that, given an exact input, produce an exact output. Every time.

But naming demands that you wrest your mind from granular, concrete details and think more generally, more categorically.

## The Abstraction Ladder

The linguist Sam Hayakawa, in his book [Language in Thought and Action](https://www.amazon.com/Language-Thought-Action-S-I-Hayakawa/dp/0156482401/), describes a model of language he calls the Abstraction Ladder. On this conceptual ladder, the bottommost rung represents the most concrete idea and things get more abstract up the ladder, with the topmost rung occupied by a decidedly abstract and intangible idea. The ladder is straightforward to understand, but its simplicity engenders broad and profound insights.

**This is a temporary, placeholder image**

<img src="images/abstraction-ladder-temp.jpg" width="100%" alt="The Abstraction Ladder" />

TODO: Insert custom illustration/diagram.

Consider the diagram above. [Assuming its a dog at the bottom] Before the ladder even starts, it acknowledges that Molly is far from static, she's actually a being in-process, constantly changing, and composed of numerous subsystems. But despite the fact that one must abstract a living system of processes to conceive of Molly, she seems, from a language and from a mental perspective, like a pretty concrete entity. Indeed, associating a name with a living being is often the first abstract utterance of a toddler learning to speak - "Mama."

The next rung up the ladder seems like a small step. But, consider the enormous mental processing it takes for a toddler to learn to ignore the differences between the family dog Molly, a [small breed - Corgi?], and the great dane that lives down the street, then to communicate this understanding by categorizing them with the word "dog." Coming by this ability to abstract takes enormous mental development, but once it's learned, it's automatic.

The linguist is concerned with the relationship between words and abstractions and meaning. So too is the programmer. Your words take the form of names. Your names communicate meaning. Yes, you write comments as well, but often comments stand in to explain names that aren't quite right. A well-chosen name can obviate the need for an explanatory comment.

## Naming functions
