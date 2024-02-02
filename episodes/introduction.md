---
title: "Collaborative Git : Introduction"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions

- Who else is doing this course?
- What can you expect from this course?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Find out something interesting about other participants.
- Understand the way in which you are expected to behave and interact with other participants.
- Have an overview of the content and material that will be covered.
- Pair up with another participant to collaborate with during this workshop.

::::::::::::::::::::::::::::::::::::::::::::::::

## Code of Conduct

To make clear what is expected, everyone participating in The Carpentries activities is required to abide by our
[Code of Conduct][coc]. Any form of behaviour to exclude, intimidate, or cause discomfort is a violation of the Code of
Conduct. In order to foster a positive and professional learning environment we encourage you to:

- Use welcoming and inclusive language
- Be respectful of different viewpoints and experiences
- Gracefully accept constructive criticism
- Focus on what is best for the community
- Show courtesy and respect towards other community members

If you believe someone is violating the Code of Conduct,
we ask that you report it to The Carpentries Code of Conduct Committee
by completing [this form](https://goo.gl/forms/KoUfO53Za3apOuOK2).

## Icebreaker

::: challenge

### Getting to Know Each Other

In order to break the ice and find out something about the other participants on this course , please think about a
situation BVC (**B**efore **V**ersion **C**ontrol) where you might have had a problem that Version Control would have
prevented. This might be deleting files by mistake, making changes to code that broke your programme. If you are
participating online please provide an answer in the collaborative notepad, if the course is being run in person please
describe the situation to the person next to you.

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Before the start of the course you should setup a new [collaborative pad][carpentryPad] where participants can answer
questions and collaborate.

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

:::

## Collaboration

Since this course is all about collaboration we would like you now to pair up with another participant in order to
undertake the exercises contained in this course. This could be the person sitting next to you if this is an in-person
course or if the course is online one of the instructors will pair you up at random.

Once paired up please add details to the Etherpad along with your GitHub usernames.

Its relatively easy to get the _basics_ of working with [Git][git] on your own or with small groups to work
collaboratively on code development. If you aren't already familiar with these basics then this course isn't for you,
yet, and you would benefit from an introductory course such as [Git, GitHub through GitKraken : From Zero to
Hero!][zeroHero] or the [Software Carpentry : Version Control with Git][swCarpentryGit]. This course aims to show you
some of the more involved ways to use Git in a collaborative environment.

### Branches

How branches can be used to fix bugs or develop features in isolation.

- Comfortable creating and switching between multiple branches.
- Keep development branches up-to-date with `main` (i.e. `git merge` and `git rebase`).
- Understand and use `git stash` to save work in progress.
- Moving commits made to the wrong branch (`git cherrypick` and `git reset`).
- Git Worktrees
- Tracking multiple Origins

## Git Hygiene

How to maintain a clean commit history.

- Understand the concept of atomic commits
- `git commit --amend`
- `git add patch`
- Using `git rebase` squash commits.
- The use of consistent nomenclature such as [Conventional Commits][conventionalCommits].

## Hooks and Continuous Integration

Understand what hooks are available and how to leverage Continuous Integration on [GitHub][github] and [GitLab][gitlab].

- How and why to use [pre-commit](https://pre-commit.com)/[pre-commit.ci](https://pre-commit.ci).
- Benefits of Continuous Integration, particularly the importance of tests (but not covering tests in detail).
- [GitHub][github] Actions and Workflows.
- [GitLab][gitlab] Continuous Integration.

## Effective Reviewing

How to make useful and constructive Pull/Merge request reviews

- Not overwhelming people with lots of comments.
- Making suggestions when reviewing.

[carpentryPad]: https://pad.carpentries.org/
[coc]: https://docs.carpentries.org/topic_folders/policies/code-of-conduct.html
[git]: https://git-scm.com
[github]: https://github.com
[gitlab]: https://gitlab.com
[swCarpentryGit]: https://swcarpentry.github.io/git-novice/
[zeroHero]: https://srse-git-github-zero2hero.netlify.app
