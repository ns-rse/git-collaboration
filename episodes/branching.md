---
title: "Collaborative Git : Branching"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions

- How do we use branches in git effectively?
- How can I check out other peoples branches whilst working on my own?
- How do I keep my development branch up-to-date with `main`?
-

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- How branches can be used to fix bugs or develop features in isolation.
- Switching branches, stashing and restoring.
- How to keep a development branch up-to-date.
- Differences between and when to use merge and rebase.
- Git worktrees instead of branches.
- Tracking multiple origins

::::::::::::::::::::::::::::::::::::::::::::::::

## Branches

Branches are key to working with version control as they allow the development of new features or fixing of bugs without
touching the current working version of code. New features and bug fixes are then merged into the `main` branch to
update the code base.

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

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

## Stashing and Restoring

::::::::::::::::::::::::::::::::::::: callout

Callout sections can highlight information.

They are sometimes used to emphasise particularly important points
but are also used in some lessons to present "asides":
content that is not central to the narrative of the lesson,
e.g. by providing the answer to a commonly-asked question.

::::::::::::::::::::::::::::::::::::::::::::::::

## Keeping Development branches up-to-date

Describe how to keep a development branch up-to-date, two strategies

- `git merge` the `main` branch into development.
- `git rebase` development branch on to `main`.

Useful resources...

## Git Worktrees

## Tracking multiple Origins

Describe how to track multiple origins

## Worktrees

Describe the concept of worktrees and why they might be preferable over branches.

::::::::::::::::::::::::::::::::::::: keypoints

- First
- Second
- Third
- Fourth

::::::::::::::::::::::::::::::::::::::::::::::::
