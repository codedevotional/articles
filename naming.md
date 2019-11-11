# Names Pin the Jell-O to the Wall

Your code and its artifacts are strewn with names. You name repositories, files,
packages, classes, functions, variables, and on and on. The names you choose
determine the quality of the conversations you have with your programmer peers.

On the flip side of that bargain, your understanding of libraries and
frameworks, i.e. other people's code, starts with names. Names form the
substrate of APIs, delineating their boundaries, clarifying their inputs and
outputs, and defining how they're used.

If you can trust that a library works, you need look no further than its API
docs to understand it. Ideally, API docs are the only necessary point of contact
you have with a library. And they are, essentially, a bunch of names.

Just as a writer grinds through word choices, a programmer must struggle with
names. As the joke goes, the two hardest things in computer science are naming,
cache invalidation, and off-by-one errors. No matter the programming language,
no matter whether you're operating from a functional or object-oriented
perspective, no matter if the language is statically or dynamically typed, names
are a ubiquitous and critical part of your code. Cache invalidation may not
matter in your application, but names sure do. The quality of your work depends
on the quality of your names.

When coding, you spend a lot of time burrowed in the minutiae of specifics. You
ply your craft at the mercy of unbending syntax rules that will bring a
million-line codebase to its knees over a single missing character. You write
small functions that, given an exact input, produce an exact output. Every time.

But naming demands that you wrest your mind from granular, concrete details and
think more generally, more categorically.

## The Abstraction Ladder

The linguist Sam Hayakawa, in his book [Language in Thought and
Action](https://www.amazon.com/Language-Thought-Action-S-I-Hayakawa/dp/0156482401/),
describes a model of language he calls the Abstraction Ladder. On this
conceptual ladder, the bottommost rung represents the most concrete idea. Ideas
get more abstract up the ladder, with the topmost rung occupied by a decidedly
abstract, intangible concept. The ladder is straightforward to understand, but
studying it leads to broad and profound insights.

**This is a temporary, placeholder image**

<img src="images/abstraction-ladder-temp.jpg" width="100%" alt="The Abstraction Ladder" />

**TODO: Replace above with custom illustration/diagram**

[Might take the concreteness caveat out of the diagram and only mention it in th
text.]
Consider the diagram above. [Assuming it's a dog at the bottom] Before the
ladder even starts, it acknowledges that Molly is far from static, she's
actually a being in-process, constantly changing, and composed of numerous
subsystems. But despite the fact that one must abstract a living system of
processes to conceive of Molly, she seems, from a language perspective and from
a mental perspective, like a concrete entity. Indeed, associating a name
with a living being is often the first abstract utterance of a toddler learning
to speak - "Mama."

The next rung up the ladder, "Dog" seems like a small step. But, consider the
enormous mental processing it takes for a toddler to learn to ignore the
differences between the family dog Molly, a [small breed - Corgi?], and the
great dane that lives down the street, then to communicate this understanding by
categorizing them with the word "dog." Coming by this ability to abstract takes
enormous mental development, but once it's learned, it's automatic, you come by
it without perceived thought.

The linguist is concerned with the relationship between words and abstractions
and meaning. So too is the programmer. Your words take the form of names. Your
names communicate meaning. Yes, you write comments as well, but often comments
stand in to explain names that aren't quite right. A well-chosen name can
obviate the need for an explanatory comment.

## Naming functions

Concrete examples explain abstract ideas. In order to explain the color green,
you might use examples like leaves in summertime, the bottom light of a traffic
signal, or the color of an emerald.

Communication is most effective when it spans various levels of abstractions,
when it travels up and down the ladder. Just stating that you should consider
abstract ideas when naming things in your code is not enough. Providing abstract
naming advice also requires concrete examples in order to be convincing.

Consider a requirement in your application to display a person's first and last
name. Simple enough. But the name you retrieve from an API you do not own
sometimes contains extra whitespace, leading, trailing, and in between. For
example, the name might arrive in your application as ` Jane Doe `. The
unpredictable, extra spaces cause issues in your UI and you would need to strip
the whitespace out for presentation.

Since you do not own the data source, you cannot fix it there. You check the
language documentation's standard string library and the name `trim` catches
your eye. You try that function but it's not quite right. Given the example
string above, it trims leading and trailing whitespace as you'd like, but the
extra space between the first and last names persists: `Jane Doe`. What you need
is a way to get just the words in the name and join them with a space. Further
docs investigation reveals both `String.split` and `Enum.join`. Used together,
they do exactly what you need.

```elixir
# In syntax that should be familiar to most programmers
name = "   Jane  Doe "
list = String.split(name)
Enum.join(list, " ")
```

Sidenote: these simple examples could be written similarly using the standard
library of almost any modern language. Since they are being written in Elixir,
henceforth, they will use idiomatic Elixir syntax. Elixir's pipe operator `|>`
passes the result of one operation as the first argument to the next. It affords
graceful and easy-to-read function composition:

```elixir
# Returns "Jane Doe"
"   Jane  Doe "
|> String.split()
|> Enum.join(" ")
```

You have figured out code that should work. In your application, there's a
`PersonPresenter` module, in which it should go. But, now you must create a
function and name it.

```elixir
defmodule PersonPresenter do
  # literal function name
  def split_and_join(name)
    name
    |> String.split()
    |> Enum.join(" ")
  end
end
```

At this point, many programmers reach for a literal explanation in choosing a
function name, like `split_and_join` above. Indeed, the code is splitting and
joining a name. The function name explains what the code is doing. The name was
quick and easy to conceive of; it's convenient.

But, recall that the data comes from a source that you don't own. It's likely to
change in unpredictable ways. Even code and data that you do own are susceptible
to ever-changing business requirements. The code you write today might seem
concrete, but like Molly the dog, even seemingly concrete entities are not
static. Code is a being in-process, constantly churning.

Naming a function after its current implementation leaves you no wiggle room for
change. It explains what the function does, but not what it means. In terms of
the Abstraction Ladder, `split_and_join` would be on a lower rung, if not the
lowest.

[Kinda roughed in. Abrupt. Might be a new subsection.]

A new requirement comes in. The product team would like names to be displayed in
last, first order. The implementation will be simple enough to rework. But the
name `split_and_join` has painted you into a corner. Do you rename it, perhaps
to `split_and_join_and_reorder`? Down that path lies madness. The naming process
that seemed convenient has proven to be costly.

Spreadsheet

Advice
