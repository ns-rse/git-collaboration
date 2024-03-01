---
title: "Git Hygiene"
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

## Git Configuration

Git configuration comes in two forms, "global" and "local" and is courtesy of some simple text files. The global
configuration file is `~/.gitconfig` (on Windows it is `C:\Users\<username>\.gitconfig`) and will have been setup when
you first attempted to use Git and were prompted for your name and email address.

::::::::::::::::::::::::::::::::::::: callout

You can always lookup the location of configuration files using

``` bash
git config --list --show-origin --show-scope
```

::::::::::::::::::::::::::::::::::::::::::::::::

### `.gitignore`

### `difftastic`

**TODO** Short aside on using [difftastic](https://difftastic.wilfred.me.uk/) as an alternative `git diff` and how to
configure it globally. Include examples of diff's before and after.

## Atomic Commits

The idea of atomic commits is that they are small self-contained commits focused on one issue

### `git commit --ammend`

You'll often find after having made a commit that there are some additional changes you wish to make

## `git add patch`

## `git absorb`

## Squashing commits

Squashing commits takes advantage of a topic we've already touched on `git rebase` as it uses the same framework but
rather than moving the `HEAD` forward along each commit on the branch you are rebasing onto it also allows you to
"squash" commits on the same branch.

We will now make a few commits to our branch and then squash them via an interactive rebase. This helps keep commits
that you will merge into `main` atomic since even if you've been using `git commit --ammend` to sequentially update a
commits you may still have several commits on a branch but be at the stage where you can combine all information into a
single informative commit that is ready for merging into the `main` branch.

## Orphaned commits

## `git maintenance`

::::::::::::::::::::::::::::::::::::: keypoints

- Use `.md` files for episodes when you want static content
- Use `.Rmd` files for episodes when you need to generate output
- Run `sandpaper::check_lesson()` to identify any issues with your lesson
- Run `sandpaper::build_lesson()` to preview your lesson locally

::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

TEMPLATE

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 1

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

## Challenge 2

:::::::::::::::::::::::: solution

You can add a line with at least three colons and a `solution` tag.

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: callout

TEMPLATE

::::::::::::::::::::::::::::::::::::::::::::::::
