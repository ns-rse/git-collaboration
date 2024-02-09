---
title: "Introduction"
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

Its relatively easy to get the _basics_ of working with [Git][git] on your own or with small groups to work
collaboratively on code development. If you aren't already familiar with these basics then this course isn't for you,
yet, and you would benefit from an introductory course such as [Git, GitHub through GitKraken : From Zero to
Hero!][zeroHero] or the [Software Carpentry : Version Control with Git][swCarpentryGit]. This course aims to show you
some of the more involved ways to use Git in a collaborative environment.

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

Between you please choose one person who will be the repository owner and one who will be a collaborator. The owner
should

::::::::::::::::::::::::::::::::::::: challenge

## Choose Roles, Copy Template Repository and Clone

Introduce yourself to the person you have paired up with. You now need to decide who is to take on each of the two
roles. There isn't much between them in terms of what you will be doing but one person needs to be the **repository
owner** and one person needs to be a **collaborator**.

## Repository Owner

The Repository Owner should visit the [Python Maths][pythonMaths] repository on GitHub and click on the "_Use This
Template_" button to make a copy to their own repository.

Once you have made a copy you need to invite your collaborator to work on the repository with you. Navigate to
_Settings > People_ and add you collaborator to the project.

:::::::::::::::::::::::::::::::::

:::::::::::::::::::::::: solution

You should receive an email from GitHub inviting you to collaborate on the repository or had a notification on GitHub
that you have been invited to collaborate on a repository. Accept this invitation.

:::::::::::::::::::::::::::::::::

:::::::::::::::::::::::: solution

## Clone the repository

Both the repository owner and collaborator should now clone the repository _from the repository owners copy **not** the
original template_.

Click on the _Code_ button and then the _SSH_ tab. Copy the URL. If you want to clone the work to `~/work/git/` then in
a terminal

``` bash
cd ~/work/git
git clone git@github.com:<owners_id>/python-maths
cd python-maths
```

:::::::::::::::::::::::::::::::::

After completing these steps you should both have a copy of the `python-maths` repository on your local computer.

## Install the Package

If you have not already done so activate the `git-collaboration` environment you created as described in the

``` bash
conda activate git-collaboration
```

You can now install the package and its test dependencies in an editable format so that as you work on the package the
changes you make will instantly be available. Make sure you are in the `python-maths` directory (use `pwd` to show where
you are and `cd` to change directory).

``` bash
pip install -e .[tests]
```

:::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

[carpentryPad]: https://pad.carpentries.org/
[coc]: https://docs.carpentries.org/topic_folders/policies/code-of-conduct.html
[git]: https://git-scm.com
[pythonMaths]: https://github.com/ns-rse/python-maths
[swCarpentryGit]: https://swcarpentry.github.io/git-novice/
[zeroHero]: https://srse-git-github-zero2hero.netlify.app
