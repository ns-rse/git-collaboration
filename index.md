---
site: sandpaper::sandpaper_site
---

Welcome to the _Git Collaboration_ course material.

[Git][git] is, in 2024, the most widely used version control system by far. It was developed by Linus Torvalds to manage
[Linux kernel][linux] development and since then has exploded. Its a wonderful tool, but because of the complexities of
version controlling software in distributed, collaborative environments the tool itself has become quite complex.

## Objectives and Learning Outcomes

Its relatively easy to get the _basics_ of working with [Git][git] on your own or with small groups to work
collaboratively on code development. If you aren't already familiar with these basics then this courser isn't for you,
yet, and you would benefit from an introductory course such as [Git, GitHub through GitKraken : From Zero to
Hero!][zeroHero] or the [Software Carpentry : Version Control with Git][swCarpentryGit]. This course aims to show you
some of the more involved ways to use Git in a collaborative environment.

### Branches

How branches can be used to fix bugs or develop features in isolation.

+ Comfortable creating and switching between multiple branches.
+ Keep development branches up-to-date with `main` (i.e. `git merge` and `git rebase`).
+ Moving commits made to the wrong branch (`git cherrypick` and `git reset`).
+ Git Worktrees
+ Tracking multiple Origins

### Git Hygeine

How to maintain a clean commit history.

+ Understand the concept of atomic commits
+ `git ammend`
+ `git add patch`
+ Using `git rebase` squash commits.
+ The use of consistent nomenclature such as [Conventional Commits][conventionalCommits].

### Hooks and Continuous Integration

Understand what hooks are available and how to leverage Continuous Integration on [GitHub][github] and [GitLab][gitlab].

+ How and why to use [pre-commit](https://pre-commit.com)/[pre-commit.ci](https://pre-commit.ci).
+ Benefits of Continuous Integration, particularly the importance of tests (but not covering tests in detail).
+ [GitHub][github] Actions and Workflows.
+ [GitLab][gitlab] Continuous Integration.

### Effective Reviewing

How to make useful and constructive Pull/Merge request reviews

+ Not overwhelming people with lots of comments.
+ Making suggestions when reviewing



[conventionalCommits]: https://www.conventionalcommits.org/en/v1.0.0/
[git]: https://git-scm.com
[github]: https://github.com
[gitlab]: https://gitlab.com
[linux]: https://www.kernel.org
[swCarpentryGit]: https://swcarpentry.github.io/git-novice/
[workbench]: https://carpentries.github.io/sandpaper-docs
[zeroHero]: https://srse-git-github-zero2hero.netlify.app
