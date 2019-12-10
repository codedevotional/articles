# Naming: Climbing Towards Abstraction

Your code and its artifacts are strewn with names. You name repositories, files, packages, classes, functions, variables, and on and on. The names you choose determine the quality of the conversations you have with your programmer peers.

On the flip side of that bargain, your understanding of libraries and frameworks, i.e. other people's code, starts with names. Names form the substrate of APIs, delineating their boundaries, clarifying their inputs and outputs, and defining how they're used.

If you can trust that a library works, you need look no further than its API docs to understand it. Ideally, API docs are the only point of contact you have with a third-party library. And they are, essentially, a bunch of names.

Just as a writer grinds through word choices, a programmer must struggle with names. As the joke goes, the two hardest things in computer science are naming, cache invalidation, and off-by-one errors. No matter the programming language, no matter whether you're operating from a functional or object-oriented perspective, no matter if the language is statically or dynamically typed, names are a ubiquitous and critical part of your code. Cache invalidation may not matter in your application, but names sure do. The quality of your work depends on the quality of your names.

When writing code, you spend a lot of time burrowed in the minutiae of specifics. You ply your craft at the mercy of unbending syntax rules &mdash; rules that will bring a million-line codebase to its knees over a single missing character. You write small functions that, given an exact input, produce an exact output. Every time.

But naming demands that you wrest your mind from granular, concrete details and think more generally, more categorically.

The linguist Sam Hayakawa, in his book [Language in Thought and Action](https://www.amazon.com/Language-Thought-Action-S-I-Hayakawa/dp/0156482401/), describes a model of language he calls the Abstraction Ladder. On this conceptual ladder, the bottommost rung holds the most concrete idea. The topmost rung is occupied by a decidedly abstract, intangible concept. In between, ideas get more abstract the higher up they are. The Abstraction Ladder is straightforward to understand, but studying it leads to broad and profound insights.

<img src="images/abstraction-ladder.svg" width="100%" alt="The Abstraction Ladder" />

Consider the diagram above. Molly is on the second rung. She is far from a static entity &mdash; she's a being in process, constantly changing, composed of numerous subsystems that are represented on the bottom rung. But from a cognitive, linguistic perspective, Molly seems like a single entity. Indeed, associating a name with a living being is often the first utterance of a toddler learning to speak - "Mama."

The next rung up the ladder, "Dogs" seems like a small step. But, consider the enormous mental processing it takes for a toddler to learn to ignore the differences between the family dog Molly, a small bull terrier, and the gigantic Great Dane that lives down the street. Learning to abstract from "Molly" to "Dogs" takes significant mental development, but once it's learned, it's automatic, processed by the brain without the perception of thought.

The linguist is concerned with the relationship between words, abstractions, and meaning. So too is the programmer. Names are your words. Your names communicate meaning and understanding. Yes, you write comments as well, but comments often stand in to explain names that aren't quite right. A well-chosen name can obviate the need for an explanatory comment.

## Naming functions

Communication is most effective when it spans various levels of abstraction, when it travels up and down the ladder. It's not enough to recommend that you consider abstract ideas when naming things in your code. Providing abstract naming advice also requires concrete examples in order to be convincing. Concrete examples explain abstract ideas. In order to explain the color green, you might use examples like leaves in summertime, the bottom light of a traffic signal, or the color of an emerald.

Consider a requirement in your application to display a person's first and last name. Simple enough. But the name you retrieve, and all other information associated with a person, comes from an external data source that you do not own. Sometimes the data contains extra whitespace &mdash; leading, trailing, and in between. For example, the name might arrive in your application as "   Jane  Doe ". The unpredictable, extra spaces cause issues in your UI and you need to strip the whitespace out for presentation.

Since you do not own the data source, you cannot fix the problem at its root. The example string above contains extra whitespace before, after, and between individual parts of the name. Simply trimming leading and trailing whitespace, as most `trim` functions do, will not be enough &mdash; it will leave extra whitespace in the middle of the string. You need to isolate the words in the name and join them with a space. `String.split` and `Enum.join`, used together, do exactly what you want.

```elixir
# In syntax familiar to most programmers
name = "   Jane  Doe "
list = String.split(name) # => ["Jane", "Doe"]
Enum.join(list, " ") # => "Jane Doe"
```

Since these examples are being shown in Elixir, henceforth, they will use idiomatic Elixir syntax.[^1] Elixir's pipe operator `|>` passes the result of one operation as the first argument to the next. It affords graceful and easy-to-read function composition:

```elixir
# Returns "Jane Doe"
"   Jane  Doe "
|> String.split()
|> Enum.join(" ")
```

### Bottom rung: dirty mirrors

You have an implementation that works. You have, of course, written tests (not shown). In your application, there's a `PersonPresenter` module, in which this functionality belongs. But, now you must create a function and name it.

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

At this point, it might be tempting to reach for an overtly literal function name, like `split_and_join` above. Indeed, the code splits and joins a name. It's quick and easy to conceive of; it's convenient.

The name `split_and_join` mirrors the function's internals. Refer back to the Abstraction Ladder diagram above. Linguistically, Molly seems like concrete entity, but internally she's a collection of subprocesses. It would be technically correct, but awfully strange, to have named Molly after her internal subprocesses, perhaps something like `organs_and_bones_and_waggy_tail`.

Names should establish an identity that's relatively unique. It's conceivable that other dogs in the world will be named Molly, but it's unlikely that many other dogs in the park will share her name. In `PersonPresenter`, you will need to present other data related to a person, like their address or phone number. It's also conceivable that you might split and join other bits of this externally-supplied data. When that day comes, you will be forced into renaming decisions. One ill-advised option would be to use `split_and_join` as a prefix for different kinds of data:

```elixir
defmodule PersonPresenter do
  def split_and_join_name(name)
    name
    |> String.split()
    |> Enum.join(" ")
  end

  def split_and_join_address(address)
    address
    |> String.split()
    |> Enum.join(" ")
  end

  def split_and_join_phone_number(phone_number)
    phone_number
    |> String.split()
    |> Enum.join(" ")
  end
end
```

Repeated name prefixes (and suffixes) are an indicator for various code smells. Here, the repetition simply boils down to a poorly chosen name. The code above might seem ridiculous on its face, but it's not hard to find function names that mirror implementation details in any sizable codebase. You should reject these kinds of names out of hand. Unless you're writing something similar to a string library, they're not a good idea.

**GUIDELINE: Resist mirroring implementation details in a function name**

### A rung up: eventually false explanations

`split_and_join` describes how the function works. Describing what the function *does* moves the name one level of abstraction higher. You might consider a name like `trim_whitespace`.

```elixir
defmodule PersonPresenter do
  def trim_whitespace(name)
    name
    |> String.split()
    |> Enum.join(" ")
  end
end
```

This is better. But recall that the data comes from a source you do not own. It's controlled by someone else and likely to change in unpredictable ways. Even code and data that you *do* own are susceptible to ever-evolving business requirements. The code you write today might seem concrete, but like Molly the dog, even seemingly concrete entities are not static. Code is a being in-process, constantly churning. Your source control repository's diff log is a more accurate mental model of your application than current files on disk.

It's quite possible that people's names will someday arrive in a different form and that removing whitespace will no longer be the only operation required to prepare it for presentation. Naming a function after what it currently does leaves you no wiggle room for change. `trim_whitespace` explains the function's current implementation, but not what it means from the caller's perspective, nor what it means from an application domain perspective.

Consider a new requirement. As it turns out, the data has not changed, but the product team would like names to be displayed in last name, first name order. The implementation will be simple enough to rework. But the name `trim_whitespace` has painted you into a corner.

Should you rename the function, perhaps to `trim_whitespace_and_reorder`? Down that path lies madness. Naming a function after its implementation leads to expensive, hand-wringing decisions.

And this simple example obscures an even more costly consideration. Your codebase depends on many messaging interfaces, as it should. In order to get anything done, your code must send messages somewhere. In other words, it must know the names of functions it can call. When your code calls a function, it becomes dependent upon the name of that function. Again, this is fine. It's better than fine; message interfaces provide the loosest form of coupling. Well-designed code depends on the messages it sends to direct collaborators and little else. Over time, many parts of your codebase may become dependent on a single message name.[^2]

In your hypothetical app, a person's name is presented in several views. `trim_whitespace` is called from many places. Your codebase has become dependent upon that name. Alarm bells should be going off in your head when you see names representing implementation details spread throughout your codebase. Changing this function name at its source means changing it everywhere it's called. There's a code smell called Shotgun Surgery that occurs when making a single modification requires that you make changes in many other places. In the categories of code smells, Shotgun Surgery is considered a Change Preventer.

Indeed, the bar for changing `trim_whitespace` increases dramatically over time as more and more code comes to depend on the function name. Public APIs are versioned for this very reason: dealing with API changes is an expensive process, fraught with peril. Handling the requirements change has become difficult and expensive due to a concrete name.

**GUIDELINE: Resist describing current implementation details in a function name**

### A rung to rest on: discovering essence

Faced with the conundrum of renaming a function with many dependents, you have a couple of options.

You can change the name where it's defined and everywhere it's used throughout your codebase. This works if your tests are robust and there are no message sends that aren't revealed by tests, grepping, or tooling.[^3] and [^4]

You might decide to live with the lie, implementing new functionality without changing the function name. Your name then professes that the function behaves in a way that it actually doesn't. This decision is the quickest to implement. It might appease your product team, but in terms of software engineering, it's kicking the can down the road. You're accruing technical debt and confusing your peers.

The cost of code is in the reading. Your code will be expensive to read as long as it is poorly named. Living with the lie and conceding technical debt, in this case, is the optimist's view. Changing a function's behavior to betray its name almost inevitably leads to downstream bugs. It's not hard to imagine a future programmer, maybe even you, deciding to trust a name and being burned with bugs when the result of the function defies the promise of its name. Once this trust is broken in your codebase, programmers in your organization will feel compelled to dig into the implementation guts of functions all the time to determine what they actually do. Working on untrustworthy code is unpleasant and time-consuming. Time is money. Poorly named functions are expensive.

You might determine that the cost of implementing the new requirement is too high in the present and poses too much risk in the near future. You'd prefer to prevent the change altogether. When you push back on the product team, explaining that it's too complex and expensive to make now, expect looks of befuddlement in response. Those looks are justified. A simple request should be simple to implement. The more you can harmonize the seemingly simple and the actually simple, the more you engender organizational trust in software engineering.

This might seem like hyperbole. How can a single poorly named function make your programming life unpleasant and engender organizational mistrust in software engineering? A single poorly named function probably won't. But, the *habit* of naming functions after their current implementation will infect your codebase, making all aspects of development slower, more expensive, and less fun.

Function names should tell the right story. Good names are resilient &mdash; they stand the test of time through changes in implementation. Naming is difficult, but names matter. It's incumbent upon you to develop this core programming skill. So, how should you name functions? 

`trim_whitespace` is clearly named after its current implementation. The requirement to reorder the name to `last, first` order has forced you into a decision: to rename or not to rename. However, that decision could have been avoided altogether, by wresting your mind from granular implementation details to think about names more categorically. Your function name should move one rung higher.

On the Abstraction Ladder, `trim_whitespace` occupies a lower rung. It's literal and concrete. In order to move the function name one rung higher, you must consider the function from the perspective of its callers. What service is the function providing?

One way to derive names is to consider other things in the same category as the current implementation and then to name that category. What other things might you do to a name to present it? A simple matrix helps:

| Person name                  | ???               |
|------------------------------|-------------------|
| Remove extraneous whitespace | `trim_whitespace` |
| Present last name first      | `last_first`      |
| Capitalize words             | `capitalize`      |
| Prepend salutation           | `add_salutation`  |

The verb `present` is a viable candidate for the missing column heading in the matrix above. Remember that the name of the module is `PersonPresenter` so naming the function present would read like an echo chamber: `PersonPresenter.present_name`.

There's another issue with the name `present`. From the internal perspective of the `PersonPresenter` module, everything is about presentation. But naming a public function demands that you consider its purpose from an outside perspective. Function callers want the presenter to do something specific to a person's name. They want it to be formatted. Context matters. `format_name` is a better choice. 

```elixir
defmodule PersonPresenter do
  def format_name(name)
    name
    |> String.split()
    |> Enum.reverse
    |> Enum.join(", ")
  end
end
```

Context and perspective also dictate that the function be named not just `format`, but `format_name` Why include `_name` when the function will accept `name` as a parameter? Despite the fact that it, too, creates a bit of an echo chamber, it contains the right amount of information from the caller's perspective. Right away you know what this does: `PersonPresenter.format_name("  Jane  Doe ")`. You are not forced to open up a file and understand implementation details. `format_name` also leaves open the possibility of something like `format_address`.

If the module were named `PersonFormatter`, it might make more sense to name the function something else, perhaps simply `name`. Naming the module is outside the purview of this article, but will be addressed in a followup article.

Diagram 2: Function names abstraction ladder

<img src="images/function-names-ladder.svg" width="100%" alt="Function Names Abstraction Ladder" />

Should you need to change how the name is formatted in response to upstream data changes or new application requirements, the impact of the implementation will be constrained to the internals of the function. The name will continue to be appropriate and no dependents will be forced to change.

In terms of the Open Closed Principle, the code is more open to the change. For example, a new requirement to add a salutation would only necessitate changes to implementation internals, no API-breaking name change would be necessary. By increasing the level of abstraction, you have created an approprate and resilient function name.

**GUIDELINE: Name functions one level of abstraction higher than their implementation**

### Climb higher: ask a friend for stability

When you seriously consider function names, it's not uncommon to come up with several candidates that seem equally viable. In those cases, break the tie by choosing words that your customers and product team use. In other words, when you're not sure, ask others for help. Just as you might ask a friend to stabilize the ladder while you climb that one rung higher to reach the work you need to do, asking for help in naming has a stabilizing effect on code.

Application lexicons arise from conversations. It reduces mental friction when the vocabulary of your team matches the words you see on the screen in your text editor. Without having to translate person-speak to code-speak, you remove just one more bit of mental processing you'd do best to avoid. Agreeing on names reduces the overall organizational cost of writing software. In this spirit, it can be helpful, especially for newcomers on your team, to maintain a glossary of terms for your application.

**GUIDELINE: Choose names from your application lexicon**

## Climb up and down the ladder

Does software design really demand this level of attention to detail, this careful weighing of names for something as unremarkable and ordinary as a single function? If you want to do your best work, then yes, it does. Names matter. Attention to details matters. Writing software is about more than just getting something to work. At an abstract level, it's an expression of thought and communication, like writing, like speaking, like relationship-building. Your thoughtfulness around names will pay off.

A literal function name indicates that you know what a function does but may not have considered what it means or how it's perceived by others who use it. When your internal and external dialog spans multiple levels of the Abstraction Ladder, you become open to the full breadth of understanding that categorical thinking bestows. That deeper understanding leads to better code, in every way.

You have guidelines for improving names and your code will be better for following them. Thinking more broadly, how do you help others improve? How do you spread value throughout your organization? Part 2 of this article will explore those questions and offer advice about naming classes and modules.

The ideas and techniques in this article rely and expand upon the content in the [99 Bottles of OOP book](https://99bottlesofoop.com) and [Sandi Metz's Practical Object-Oriented Design Course](https://www.sandimetz.com/courses). The second edition of the 99 Bottles book is in the works. It will contain several new chapters with about 50% more content than the first edition. Now, when you buy the first edition, you get a free upgrade to the second edition.

## Guidelines summary

- Resist mirroring implementation details in a function name
- Resist describing current implementation details in a function name
- Name functions one level of abstraction higher than their implementation
- Choose names from your application lexicon

## Image attributions

- Ladder: Ladder by Jamie Dickinson from the Noun Project
- Bull terrier: Dog by Jens Tärning from the Noun Project
- Large dog: Dog by Amanda Wray from the Noun Project
- Puppy: Dog by bmijnlieff from the Noun Project
- Cat: Cat by Nabilauzwa from the Noun Project
- Hamster: Hamster by Nagy Máté from the Noun Project
- Family with dog: Family by b farias from the Noun Project
- Heart: Love by Aulia from the Noun Project
- Lungs: Lungs by Olena Panasovska from the Noun Project
- Ladder with three rungs: Ladder by businessicons13 from the Noun Project

## Footnotes

[^1]: These simple examples could be written similarly using the standard library of almost any modern language. 

[^2]: In object-oriented parlance, you send messages. In functional parlance, you call functions. This article is concerned with naming, which applies generally, across approaches to programming. As such, it treats these ideas equally and refers to them interchangeably.

[^3]: Metaprogramming gets a bad rap because it complicates this kind of change. Failures attributed to metaprogramming are sometimes failures of naming, at their root.

[^4]: Compiled languages may save your bacon for this kind of change. But the original sin still stands. At some point, overly concrete thinking and hasty decision making will defeat the security blanket of your tools.

