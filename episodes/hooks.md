<<<<<<< HEAD
---
title: "Hooks"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions

- What the hell are hooks?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Understand the concept of a hook.
- Know what the different types of hooks are and where they are stored.
- Understand how pre-commit fits in to the hooks framework.

::::::::::::::::::::::::::::::::::::::::::::::::

## Branches

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Inline instructor notes can help inform instructors of timing challenges
associated with the lessons. They appear in the "Instructor View"

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 1: Can you do it?

What is the output of this command?

```r
paste("This", "new", "lesson", "looks", "good")
```

:::::::::::::::::::::::: solution

## Output

```output
[1] "This new lesson looks good"
```

:::::::::::::::::::::::::::::::::

## Challenge 2: how do you nest solutions within challenge blocks?

:::::::::::::::::::::::: solution

You can add a line with at least three colons and a `solution` tag.

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Merging Branches

::::::::::::::::::::::::::::::::::::: callout

Callout sections can highlight information.

They are sometimes used to emphasise particularly important points
but are also used in some lessons to present "asides":
content that is not central to the narrative of the lesson,
e.g. by providing the answer to a commonly-asked question.

::::::::::::::::::::::::::::::::::::::::::::::::

## Keeping Development branches up-to-date

One of our episodes contains $\LaTeX$ equations when describing how to create
dynamic reports with {knitr}, so we now use mathjax to describe this:

`$\alpha = \dfrac{1}{(1 - \beta)^2}$` becomes: $\alpha = \dfrac{1}{(1 - \beta)^2}$

Cool, right?

::::::::::::::::::::::::::::::::::::: keypoints

- Use `.md` files for episodes when you want static content
- Use `.Rmd` files for episodes when you need to generate output
- Run `sandpaper::check_lesson()` to identify any issues with your lesson
- Run `sandpaper::build_lesson()` to preview your lesson locally

::::::::::::::::::::::::::::::::::::::::::::::::
||||||| parent of 556214d (Starting to customise and add sections; updates pre-commit hooks)
=======
---
title: "Collaborative Git : Hooks"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions

- What the hell are hooks?
- How can hooks improve my development workflow?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Understand the concept of a hook.
- Know what the different types of hooks are and where they are stored.
- Understand how pre-commit fits in to the hooks framework.

::::::::::::::::::::::::::::::::::::::::::::::::

## What are hooks?

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Inline instructor notes can help inform instructors of timing challenges associated with the lessons. They appear in the
"Instructor View"

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 1: Can you do it?

What is the output of this command?

```r
paste("This", "new", "lesson", "looks", "good")
```

:::::::::::::::::::::::: solution

## Output

```output
[1] "This new lesson looks good"
```

:::::::::::::::::::::::::::::::::

## Challenge 2: how do you nest solutions within challenge blocks?

:::::::::::::::::::::::: solution

You can add a line with at least three colons and a `solution` tag.

::::::::::::::::::::::::::::::::: ::::::::::::::::::::::::::::::::::::::::::::::::

## Types of Hooks

::::::::::::::::::::::::::::::::::::: callout

Callout sections can highlight information.

They are sometimes used to emphasise particularly important points but are also used in some lessons to present
"asides": content that is not central to the narrative of the lesson, e.g. by providing the answer to a commonly-asked
question.

::::::::::::::::::::::::::::::::::::::::::::::::

## Pre-Commit


Pre-commit hooks are _really_ useful to the extent that they require special discussion because there is a framework for
using such hooks, [pre-commit][pre-commit] that makes it incredibly easy to add (and configure) some really useful
pre-commit hooks to your workflow.


### Why are Pre-Commit hooks so important?

You may be wondering why running hooks prior to commits is so important. The short answer is that it reduces the
feedback loop and speeds up the pace of development. The long answer is that it only really becomes apparent after doing
so we're going to have a go at installing and enabling some `pre-commit` hooks on our code base, making some changes and
committing them.


#### Installation

[pre-commit][pre-commit] is written in [Python][python] and hooks are available that lint, check and test many languages other than Python

### Adding Hooks

Which hooks you use will depend largely on the language you are using.

### Pre-commit.ci



::::::::::::::::::::::::::::::::::::: keypoints

- Use hooks liberally, they save you time.
- Enable hooks in CI so contributions are checked.

::::::::::::::::::::::::::::::::::::::::::::::::

[pre-commit]: https://pre-commit.com
[python]: https://python.org
>>>>>>> 556214d (Starting to customise and add sections; updates pre-commit hooks)
