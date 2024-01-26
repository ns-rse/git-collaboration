---
title: "Collaborative Git : Git Hygiene"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions

- How do we keep our repository and history clean?
- What are atomic commits?
- How do I squash commits?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Use `.gitignore` to avoid adding unnecessary files.
- Understand the concept of Atomic commits.
- Ammending commits.
- `git add patch`
- `git absorb` the magic sponge!
- Squashing commits

::::::::::::::::::::::::::::::::::::::::::::::::

## `.gitignore`

## Atomic Commits

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

## Ammending Commits

::::::::::::::::::::::::::::::::::::: callout

Callout sections can highlight information.

They are sometimes used to emphasise particularly important points
but are also used in some lessons to present "asides":
content that is not central to the narrative of the lesson,
e.g. by providing the answer to a commonly-asked question.

::::::::::::::::::::::::::::::::::::::::::::::::

## `git add patch`

## `git absorb`

## Squashing commits

Squashing commits takes advantage of a topic we've already touched on `git rebase` as it uses the same framework but
rather than moving the `HEAD` forward along each commit on the branch you are rebasing onto it also allows you to
"squash" commits on the same branch.

We will now make a few commits to our branch and then squash them. This helps keep commits that you will merge into
`main` atomic since even if you've been using `git ammend` to sequentially update a commits you may still have several
commits on a branch but be at the stage where you can combine all information into a single informative commit that is
ready for merging into the `main` branch.

::::::::::::::::::::::::::::::::::::: keypoints

- Use `.md` files for episodes when you want static content
- Use `.Rmd` files for episodes when you need to generate output
- Run `sandpaper::check_lesson()` to identify any issues with your lesson
- Run `sandpaper::build_lesson()` to preview your lesson locally

::::::::::::::::::::::::::::::::::::::::::::::::
