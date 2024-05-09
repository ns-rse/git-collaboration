---
title: "Git Hygiene"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions

- How do I configure Git globally and locally?
- How do we keep our repository and history clean?
- What are atomic commits?
- How do I avoid `Fixing typo` commits?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Command line configuration of Git.
- Manually editing Git configuration files.
- Use `.gitignore` to avoid adding unnecessary files.
- Understand the concept of Atomic commits.
- Ammending commits.
- `git absorb` the magic sponge!
- Squashing commits.
- Automated maintenance.

::::::::::::::::::::::::::::::::::::::::::::::::

## Git Configuration

Git configuration comes in two forms, "global" and "local" and is courtesy of some simple text files. The global
configuration file lives in your home director and on GNU/Linux and OSX systems is `~/.gitconfig` (on Windows it is
`C:\Users\<username>\.gitconfig`) and will have been setup when you first attempted to use Git and were prompted for
your name and email address.

Each repository that is under Git version control has a `.git/` directory where all of the configuration, hooks and
history live. Within this directory you will find a `.git/config` file which is the "local" configuration for that
repository. Configuration defined locally over-rides global configurations.

There are two ways of modifying either the global or local configuration, using the Command Line `git config <options>`
or by editing either the global (`~/.gitconfig`) or local (`git/config`) files.

### `git config`

The `git config` command has a host of options that you can view with the `--help` flag. The first required option says
what file should be modified and is typically either `global` or `local`. You can view the configuration with `git
config --list` and you can optional restrict it to either the `--global` or `--local` configuration.

``` bash
git config --list
git config --list --local
git config --list --global
```

Adding values requires a bit of understanding about the structure of the configuration file, a very simple example is
shown below.

``` bash
[user]
 email = a.n.other@sheffield.ac.uk
 name = A N Other
[core]
 editor = nano
 sshCommand = ssh -i ~/.ssh/id_ed25519 -F /dev/null
 attributesFile = $HOME/.gitattributes
 autocrlf = input
 excludesfile = ~/dotfiles/git/.gitignore
```

Sections are in square brackets with names, e.g. `[user]` or `[core]`. Fields then have key and value pairs e.g. the
`name` value is set to `A N Other` the `email` address is `a.n.other@sheffield.ac.uk` and the `editor` is set to `nano`
and so forth.

To modify values you need to know the section and the key you want to change, these are combined to give the third
argument `user.email` and you then provide the value you want it to be as the fourth argument. For example to change the
email address in the global configuration you would.

``` bash
git config --global user.email a.other@sheffield.ac.uk
```

::::::::::::::::::::::::::::::::::::: callout

You can always lookup the location of configuration options using the following command which shows the file in which each
configuration is set as the first column of output.

``` bash
git config --list --show-origin --show-scope

```

::::::::::::::::::::::::::::::::::::::::::::::::

### Editing config files

You can also edit both the local (`.git/config`) and global (`~/.gitconfig`) files directly to set configuration options
and this can at times be much quicker.

For example if we wanted to configure Git so that the order in which branches are listed is by the most recent commit we
could add the following to our `~/.gitconfig` using `nano`, which will result in branches being listed in reverse
chronological order when you `git branch --list`.

``` bash
[branch]
    sort = -commiterdate
```

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 1

Add the fields `user` and `email` to the `github` section of your global configuration setting them to your GitHub
username and your registered email address.

:::::::::::::::::::::::: solution

## Solution 1 - Command Line

``` bash
git config --global github.user ns-rse
git config --global github.email n.shephard@sheffield.ac.uk
```

:::::::::::::::::::::::::::::::::
:::::::::::::::::::::::: solution

## Solution 2 - Editing `~/.gitconfig`

You could alternatively edit the `~/.gitconfig` file directly and add the following lines

``` bash
[github]
    user = ns-rse
    email = n.shephard@sheffield.ac.uk
```

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

<!-- ### Multiple Configurations -->

<!-- It may be the case that you have multiple Git accounts and want different configurations for each. This is possible to -->
<!-- do in your "global" configuration courtesy of the `includeIf` directive and creating secondary `.gitconfig_<account>` -->
<!-- files. -->

<!-- Say you have an account on GitHub and one on GitLab and you wish to use different configuration parameters for each. -->

### `.gitignore`

The [`.gitignore`][gitignore] file does exactly what you might expect it to, it contains lists of directories and files
that should be ignored. To save having to write out the path to each and every file the format accepts [regular
expressions][regex]. This file, like many others uses `#` as a comment, to
use a `#` in a file name you therefore need to escape it with the `\` slash. As with most regular expression engines the
`*` is a wild card.

A common set of files you may want to ignore is the `.DS_Store` directory that Mac OSX automatically generates in
most directories. Just as you can exclude files you can list directories so add that to the `.gitignore` in the
`python-maths` repository now. Navigate to the directory and open the file using [nano][nano] and add the following
line.

``` bash
.DS_Store
```

It is often sensible to ensure data files are not included in your repository. What these files might be depends on how
you are working, common formats are `.csv` for text files `.RData` for files from [R][r] and `.pkl` are the Python
pickles.

GitHub has a useful feature when you create a repository to include template `.gitignore` files for specific languages,
but if you missed out this step you can always use the [.gitignore generator][ignoregenerator] to generate files to be
ignored and copy and paste these in.

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Remember to switch to GitHub and go through the process of creating a new repository to show where the option to select
a template can be found.

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

The `.gitignore` file is part of the repository and is itself version controlled, this means that its rules are applied
consistently across anyone who works on the project or a fork of it (since forks may end up making contributions
up-stream). You therefore have to remember to stage and commit changes to the file just as you would other files in the
repository.

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 2

In your pairs exclude files with the extension `.csv` and `.pkl` from being added to the `python-maths` project by
adding the appropriate pattern to the `.gitignore` file to a new branch and merge it into the `main` branch via a
pull-request, assigning it to the other person for review.

:::::::::::::::::::::::: solution

## Update the `.gitignore`

The following lines to `.gitignore` will ignore all files with the extensions `.csv` and `.pkl`. The wildcard symbol `*`
is required to ensure _any_ file, no matter what comes before the extension is ignored.

```output
*.csv
*.pkl
```

Staging and committing, then pushing to GitHub

``` bash
git add .gitignore
git commit -m "chore: Ignoring .csv and .pkl files"
git push
```

Pull requests are created on GitHub and as you are both working on the same repository you will have both made Pull
Requests. Make sure to review the changes are as expected and merge one of the pull requests. The second one will have a
conflict, but you can safely close the request, perhaps adding a comment to say the changes have already been merged and
referencing the Pull Request number preceeded with a `#` so it is linked.

:::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

### `difftastic`

When undertaking Pull Requests on GitHub there is the ability to toggle between two [[different views]][githubdiff] of the
differences. The standard view shows the changes line-by-line and looks like the following where the old lines are
started with `-` signs and may well be in red and the newer lines are started with `+` and may well be in green.

``` bash
@@ -1861,12 +1862,18 @@ tree -afhD -L 2 main/

 Each branch can have a worktree added for it and then when you want to switch between them its is simply a case of
-`cd`ing into the worktree (/branch) you wish to work on. You use Git commands within the directory to apply them to that
-branch and Git keeps track of everything in the usual manner.
+`cd`ing into the worktree (/branch) you wish to work on. You use Git commands within the worktree directory to apply
+them to that branch and Git keeps track of everything in the usual manner.

-Lets create two worktree's, the `contributing` and `citation` we created above when working with branches.
+###
+Lets create two worktree's, the `contributing` and `citation` we created above when working with branches. If you didn't
+already follow along the above steps do so now.
```

Its a matter of personal preference but it can sometimes be easier to look at differences in the split view that
`difftastic` provides, the same changes above using the split view are shown below.

``` bash
1862                                                                            1863
1863 Each branch can have a worktree added for it and then when you want to swi 1864 Each branch can have a worktree added for it and then when you want to swi
.... tch between them its is simply a case of                                   .... tch between them its is simply a case of
1864 `cd`ing into the worktree (/branch) you wish to work on. You use Git comma 1865 `cd`ing into the worktree (/branch) you wish to work on. You use Git comma
.... nds within the directory to apply them to that                             .... nds within the worktree directory to apply
1865 branch and Git keeps track of everything in the usual manner.              1866 them to that branch and Git keeps track of everything in the usual manner.
1866                                                                            1867
....                                                                            1868 ###
1867 Lets create two worktree's, the `contributing` and `citation` we created a 1869 Lets create two worktree's, the `contributing` and `citation` we created a
.... bove when working with branches.                                           .... bove when working with branches. If you didn't
....                                                                            1870 already follow along the above
steps do so now.
```

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Show how to toggle the view on GitHub pull requests. Make sure to have an example that is already open in a tab of your
browser.

If you have `difftastic` already configured for Git make sure to disable if you are going to show the difference in the
terminal live.

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 3

Install [difftastic][difftastic] on your computer and configure Git globally to use it.

**Hint** There are instructions on the [website][difftastic].

:::::::::::::::::::::::: solution

## Update the `~/.gitconfig`

The [instructions](https://difftastic.wilfred.me.uk/git.html) show the configuration options you can add to
`~/.gitconfig` to setup an alias for `git dft` which uses `difftastic`. The following in your `.gitconfig` will set that
up.

```config
[diff]
        tool = difftastic

[difftool]
        prompt = false

[difftool "difftastic"]
        cmd = difft "$LOCAL" "$REMOTE"

[pager]
        difftool = true
# `git dft` is less to type than `git difftool`.
[alias]
        dft = difftool
```

:::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

## Atomic Commits

The idea of atomic commits is that they are small self-contained commits focused on one issue, all the changes are
typically in a small subset of files, e.g. only the a particular module and its associated test file. But you may have
learnt to make lots of small commits frequently and so you're history may look like.

``` bash
git log --oneline
  0d2f520 Correct spelling
  325d038 Document function xyz
  86d7633 Add docstring to function xyz
  a58d6e7 Fix function xyz to pass tests
  9429ab4 Add test for function xyz
  bb560b0 Add function xyz
```

Here six commits have been made for adding the `xyz` function, writing tests that pass, adding docstrings to the
function and correcting some spelling mistakes. But all of these pertain to one issue that will have been written up on
the projects issues and as the work is self-contained and we've not added to any other files they could be a single
commit.

Git has a few functions to help here and we'll go through those now, the first is amending commits.

### `git commit --ammend`

We'll use [pytest-examples](https://github.com/ns-rse/pytest-examples) as an example for this work but you can do
this in any repository you have going. We'll clone the repository and make a new branch called `amend-fixup-tutorial`.

``` bash
git clone git@github.com:ns-rse/pytest-examples.git
cd pytest-examples
git switch -c amend-fixup-tutorial
  Switched to a new branch 'amend-fixup-tutorial'
```

Lets add a simple `CONTRIBUTING.md` file to the repository.

``` bash
echo "# Contributing\n\nContributions via pull requests are welcome." > CONTRIBUTING.md
git add CONTRIBUTING.md
git commit -m "Adding CONTRIBUTING.md"
```

``` bash
git log --oneline
  01191a2 (HEAD -> amend-fixup-tutorial) Adding CONTRIBUTING.md
```

This should be familiar to users of [Git][git] whether you use the command line interface (CLI), [Emacs'][emacs] amazing
Git porcelain [magit][magit] or any other tool such as [GitKraken][gitkraken] or the support in you IDE such as
[RStudio][rstudio] or VSCode.

### Making Amends

Sometimes you will have made a commit and you realise that you want to add more to it or perhaps you forgot to run your
test suite and find that on running it your tests fail so you need to make a correction. In this example we want to be
more explicit about how to make contributions and let people know they should fork the branch.

``` bash
echo "\nPlease make a fork of this repository, make your changes and open a Pull Request." >> CONTRIBUTING.md
```

Now you could make a second commit...

``` bash
git add -u && git commit -m "Ask for PRs via fork in CONTRIBUTING.md"
```

``` bash
git log --oneline
9f0655b (HEAD -> amend-fixup-tutorial) Ask for PRs via fork in CONTRIBUTING.md
01191a2 Adding CONTRIBUTING.md
```

...and there is nothing wrong with that. However, Git history can get long and complicated when there are lots of small
commits, because these two changes to `CONTRIBUTING.md` are essentially the same piece of work. If we'd been thinking
clearly we would have written about making forks in the first place and made a single commit.

Fortunately Git can help here as there is the `git commit --amend` option which adds the staged changes to the last
commit and allows you to edit the last commit message (if nothing is currently staged then you will be prompted to edit
the last commit message). We can undo the last commit using `git reset HEAD~1` and instead amend the first commit that
added the `CONTRIBUTING.md`

``` bash
git reset HEAD~1
git add -u
git commit --amend
```

``` bash
git log --oneline
  4fda15f (HEAD -> amend-fixup-tutorial) Adding CONTRIBUTING.md
cat CONTRIBUTING.md
# Contributing

Contributions via pull requests are welcome.

Please make a fork of this repository, make your changes and open a Pull Request.
```

We now have one commit which contains the new `CONTRIBUTING.md` file that contains all the changes we wished to have
in the file in the first place and our Git history is slightly more compact.

### `git commit --fixup`

Amending commits is great providing the commit you want to change is the last commit you made (i.e. `HEAD`). But
sometimes you might wish to correct a commit further back in your history and `git commit --amend` is of no use
here. Git has a solution though in the form of `git commit --fixup` command which allows you to mark a commit as being a
"fix up" of an older commit. These can then be autosquashed via an interactive Git rebase.

Let's add a few empty commits to our `amend-fixup-tutorial` branch to so we can do this.

``` bash
git commit --allow-empty -m "Empty commit for demonstration purposes"
git commit --allow-empty -m "Another empty commit for demonstration purposes"
```

```bash
git log --oneline
  8061221 (HEAD -> amend-fixup-tutorial) Another empty commit for demonstration purposes
  65587ce Empty commit for demonstration purposes
  4fda15f Adding CONTRIBUTING.md
```

And let's expand our `CONTRIBUTING.md` file further.

``` bash
echo "\nPlease note this repository uses [pre-commit](https://pre-commit.com) to lint the Python code and Markdown files." >> CONTRIBUTING.md
```

We want to merge this commit with the first one we made in this tutorial using `git commit --fixup`. To do this we need
to know the hash (`4fda15f` see output from above `git log`) or the relative reference of the commit we want which in
this case is `HEAD~2` as it is three commits back from the current `HEAD` (which is commit `0`, most indexing in
computing starts at `0` rather than `1`). Use _one_ for the following `git commit --fixup` commands (adjusting the hash
to yours if you are using that option, you can find this using `git log --oneline`).

``` bash
git add -u
git commit --fixup 4fda15f
git commit --fixup HEAD~2
```

We see the commit we have just made starts with `fixup!` and is then followed by the commit message that it is fixing.

```bash
git log --oneline
  97711a4 (HEAD -> amend-fixup-tutorial) fixup! Adding CONTRIBUTING.md
  8061221 Another empty commit for demonstration purposes
  65587ce Empty commit for demonstration purposes
  4fda15f Adding CONTRIBUTING.md
```

The final step is to perform the automatic squashing via an interactive rebase, again you can either use the hash or the
relative reference.

``` bash
git rebase -i --autosquash 4fda15f
git rebase -i --autosquash HEAD~2
```

This will open the default editor and because the `--autosquash` option has been used it will already have marked the
commits that need combining with `fixup`. All you have to do is save the file and exit and we can check the history and
look at the contents of the file.

**NB** If you find that the necessary commit _isn't_ already marked navigate to that line and delete `pick`. The lines
below the file you have open give instructions on how you can mark commits for different actions, in this case you can
replace `pick` with either `f` or `fixup`. Save and exit and the commits are squashed.

```bash
git log --oneline
  0fda21e (HEAD -> amend-fixup-tutorial) Another empty commit for demonstration purposes
  65587ce Empty commit for demonstration purposes
  4fda15f Adding CONTRIBUTING.md
cat CONTRIBUTING.md
  # Contributing

  Contributions via pull requests are welcome.

  Please make a fork of this repository, make your changes and open a Pull Request.

  Please note this repository uses [pre-commit](https://pre-commit.com) to lint the Python code and Markdown files.
```

And you're all done! If you were doing this for real on a repository you could now `git push` or continue your work. As
this was just an example we can switch branches back to `main` and force deletion of the branch we created.

``` bash
git switch main
git branch -D amend-fixup-tutorial
```

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 4

In your pairs there are two issue templates in the `python-math` repository that you are using.

- _03 Zero Division Amend and Fixup_
- _04 Square Root Amend and Fixup_

Create and assign one of these each and work through the stages. The tasks build on material already covered
e.g. creating and switching branches and conventions for naming branches and rebasing. Solutions to each step are
provided but try not to use them instead you can use your `history` to check what commands you have used.

:::::::::::::::::::::::: solution

## Main branch with improved docstrings

On the `main` branch of your `python-maths` repository the `divide` function in `pythonmaths/arithmetic.py` should look
like the following with four examples.

```python
def divide(x: int | float, y: int | float) -> float:
    """
    Divide x by y.

    Parameters
    ----------
    x : int | float
        Numerator for division.
    y : int | float
        Denominator for division.

    Returns
    -------
    float
        The result of dividing `x` by `y`.

    Examples
    --------
    >>> from python_math import arithmetic
    >>> arithmetic.divide(10, 2)
        5.0
    >>> arithmetic.divide(5, 2)
        2.5
    >>> arithmetic.divide(3, 0)
        You can not divide by 0, please choose another value for 'y'.
    >>> arithmetic.divide(1, 0.1)
        10
    """
    return x / y

```

The `square_root` function should look like the following.

``` python
def square_root(x):
    """Return the square root of a number.

    Parameters
    ==========
    x : int | float
        The number for which you wish to find the square root.

    Returns
    =======
    float
        The square root of x.

    Examples
    ========
    >>> from python_math import arithmetic
    >>> arithmetic.square_root(4)
        2.0
    >>> arithmetic.square_root(169)
        13.0
    """
    try:
        return x ** (1 / 2)
    except:

```

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

### `git absorb`

Rather than having to look up what commit hashes or how many commits back you need to go to pass as an argument to
`--fixup` you can instead use the [git-absorb][gitabsorb] extension that works out what commits changes to each file
being fixed up need rebasing and with the `--and-rebase` flag it will automatically perform the squashing rebase.

The steps involved then become much shorter with.

```bash
git add -u
git absorb --and-rebase
```

By default `git absorb` will search the last 10 commits but this can be configured at runtime using the `--base` flag to
specify the last commit to check or by adapting the configuration file.

### Squashing commits

If you don't want to use [git-absorb][gitabsorb] and you forgot to use `git commit --fixup` you can still combine
commits using an interactive rebase `git rebase -i`.  We've already touched on `git rebase` in the context of keeping
branches up-to-date but its a very flexible and powerful component of Git and it also allows you to "squash" commits on
the same branch.

We will now make a few commits to our branch and then squash them via an interactive rebase. This helps keep commits
that you will merge into `main` atomic since even if you've been using `git commit --ammend` to sequentially update a
commits you may still have several commits on a branch but be at the stage where you can combine all information into a
single informative commit that is ready for merging into the `main` branch.

Returning to the `pytest-examples` repository we cloned earlier we will make a series of empty commits to the repository
and then undertake an interactive rebase to squash them.

``` bash
cd ~/work/git/hub/ns-rse/pytest-examples
git commit --allow-empty -m "Commit 1"
git commit --allow-empty -m "Commit 2"
git commit --allow-empty -m "Commit 3"
git commit --allow-empty -m "Commit 4"
git commit --allow-empty -m "Commit 5"
gl     # If you set the alias earlier if not git log --oneline
```

To squash these commits we need to know the hash or relative reference to the first commit we wish to interact with
which the `git log` command does (if you set the `gl` alias earlier you can use that)

``` bash
git log --one-line
c33ab51 (HEAD -> main) Commit 5
f7bb1c9 Commit 4
d47d914 Commit 3
e859738 Commit 2
c437414 Commit 1
2f7c382 (origin/main) Merge pull request #6 from ns-rse/ns-rse/tidy-print
a1101c7 [pre-commit.ci] Fixing issues with pre-commit
```

The hash we want is the one _before_ `Commit 1` (`c437414` or `HEAD~4`). We start a rebase with `git rebase -i c437414^`
which will open our default editor, hopefully `nano` and show the following.

::::::::::::::::::::::::::::::::::::: callout

The caret (`^`) is important here as you need to rebase back to the commit _before_ the one you wish to include in the
squash. You could have specified `git rebase -i 2f7c382` and omitted the caret with the same effect.

::::::::::::::::::::::::::::::::::::::::::::::::

``` bash
pick c437414 Commit 1 # empty
pick e859738 Commit 2 # empty
pick d47d914 Commit 3 # empty
pick f7bb1c9 Commit 4 # empty
pick c33ab51 Commit 5 # empty

# Rebase c437414..c33ab51 onto c437414 (4 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup [-C | -c] <commit> = like "squash" but keep only the previous
#                    commit's log message, unless -C is used, in which case
#                    keep only this commit's message; -c is same as -C but
#                    opens the editor
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
#         create a merge commit using the original merge commit's
#         message (or the oneline, if no original merge commit was
#         specified); use -c <commit> to reword the commit message
# u, update-ref <ref> = track a placeholder for the <ref> to be updated
#                       to this position in the new commits. The <ref> is
#                       updated at the end of the rebase
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
```

The instructions here are really useful and tell us how to edit the rebase. The first line tells us that we are rebasing
the range of commits `onto c437414`. Subsequently there is a list of commands, by default `pick` is in place for each of
the commits, but we are shown the available options and simply need to replace each of the `pick` with `s` or `squash`
and we want to apply it to commits two through to 5.

You can do this manually by editing the file or you can use your editors find and replace functionality which in `nano`
is `Ctrl + \` and you will be prompted for the string you want to find (`pick`) and what you want to replace it with
`squash` and then asked if you want to change the first instance or all. We can safely change all as it doesn't matter
if the instances in the comments section are replaced. The first four rows of the file should now read like the
following.

``` bash
pick   c437414 Commit 1 # empty
squash e859738 Commit 2 # empty
squash d47d914 Commit 3 # empty
squash f7bb1c9 Commit 4 # empty
squash c33ab51 Commit 5 # empty
```

Save this file and exit (in `nano` use `Ctrl + o` then `Ctrl + x`), the editor will exit return you to the prompt and
then in the blink of an eye open the editor again with a different message. This is now your opportunity to edit the
commit message for the single commit that will remain in the tree, as the notes show. Any lines starting with a `#` are
comments and will be ignored but this is very useful as it saves you having to re-write all the text across the commits
and you can instead edit them.

``` bash
# This is a combination of 5 commits.
# This is the 1st commit message:

Commit 1

# This is the commit message #2:

Commit 2

# This is the commit message #3:

Commit 3

# This is the commit message #4:

Commit 4

# This is the commit message #5:

Commit 5

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Fri Mar 8 14:39:47 2024 +0000
#
# interactive rebase in progress; onto 2f7c382
# Last commands done (5 commands done):
#    squash f7bb1c9 Commit 4 # empty
#    squash c33ab51 Commit 5 # empty
# No commands remaining.
# You are currently rebasing branch 'main' on '2f7c382'.
#
# No changes
```

Edit the file to read how you want it to, here I've gone with the following to make it clearly

``` bash
Squash of empty commits 1-5

This is an example of how to squash commits and combines the original commits...

+ Commit 1
+ Commit 2
+ Commit 3
+ Commit 4
+ Commit 5
```

When done save and exit (in `nano` use `Ctrl + O` then `Ctrl + X`). You should be informed the rebase was successful and
if you look at the plain `git log` your commit message will be there at the top in all its glory.

``` bash
git rebase -i 2f7c382
[detached HEAD 2a0c155] Squash of empty commits 1-5
 Date: Fri Mar 8 14:39:47 2024 +0000
Successfully rebased and updated refs/heads/main.

git log

commit 2a0c1551039f8fd43af74656a6150e71254c6669 (HEAD -> main)
Author: Neil Shephard <n.shephard@sheffield.ac.uk>
Date:   2024-03-08 14:39:47 +0000

    Squash of empty commits 1-5

    This is an example of how to squash commits and combines the original commits...

    + Commit 1
    + Commit 2
    + Commit 3
    + Commit 4
    + Commit 5

commit 2f7c3826b310269b06dd86cca930bdd767ad9fbf (origin/main)
Merge: feee987 a1101c7
Author: Neil Shephard <n.shephard@sheffield.ac.uk>
Date:   2024-03-07 16:07:06 +0000

    Merge pull request #6 from ns-rse/ns-rse/tidy-print
```

::::::::::::::::::::::::::::::::::::: callout

When squashing commits they do not have to be contiguous, you can pick and choose any combination. Commits that are
prefixed with `pick` will remain in the Git history.

::::::::::::::::::::::::::::::::::::::::::::::::

## Keep things tidy

Overtime the information about branches and commits can become bloated. We've seen how to delete branches already but
there are a few other simple steps we can take to help keep the repository clean.

### Taking out the Garbage

Many repositories on GitHub and GitLab are configured to delete branches once they have been merged to `main`, if you're
not diligent you can end up with a lot of old, stale local branches or orphaned commits. Git has a useful tool to help
with this called `git gc`, the `gc` stands for Garbage Collection.

Understanding how `git gc` works and what it is doing is an [advanced topic][advanced] and not covered in this tutorial,
however it is worth being aware of as it calls a number of other commands such as `git prune` and `git repack`.
Fortunately Git has been designed to run `git gc` automatically at various stages (e.g. after `git pull`, `git merge`,
`git rebase` and `git commit`) so you shouldn't need to run it manually very often but can do so if required.

::::::::::::::::::::::::::::::::::::: callout

Orphaned commits are those that are not referenced by any other commits and don't form part of a branch. This can happen
if you use `git reset` to move the `HEAD` of a branch to an earlier commit before you started making some changes and
then made a new set of commits.

::::::::::::::::::::::::::::::::::::::::::::::::

### Maintenance

[`git maintenance`][gitmaintenance] is a _really_ useful command that will "_Run tasks to optimize Git repository data,
speeding up other Git commands and reducing storage requirements for the repository._". The details of what this does
are beyond the scope of this tutorial (refer to the [help page][gitmaintenance] if interested). Providing you have setup
your GitHub account with SSH keys and they are available via something such as keychain locally then you can bring a
repository under `git maintenance` and forget about it.

``` bash
git mainetenance register
```

This adds entries to your global configuration (`~/.gitconfig`) to ensure the repository will have these tasks run at
the scheduled point (default is hourly).

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Be prepared to explain how SSH keys can be unlocked on login so that the passwords don't need entering every time you
try to use the SSH key.

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

## Conventional Commits

You may have noticed in many of the commit messages used so far a keyword is used to start the commit followed by a
colon. This is an example of [Conventional Commits][concommit] which are a standardised way of writing commit messages
that, as with the branch naming convention suggested earlier, include metadata about what the commit relates to.

There are keywords to start your commit message with that are self-explanatory

- `fix:`
- `feat:`ure
- `build:`
- `chore:`
- `ci:`
- `docs:`
- `style:`
- `refactor:`
- `perf:`ormance
- `test:`

If changes relate to a specific component or "scope" of a repository that can be included in parentheses afterwards. For
example the Zero Division issue in `python-maths` relates to the `artihmatic` module so might be started with
`fix(arithmatic)`.

::::::::::::::::::::::::::::::::::::: keypoints

- Global configuration is via `.gitconfig`
- Local configuration is via `.git/config` and takes precedence over Global.
- Configuration can be done at the command line or by editing files.
- Ignore files using `.gitignore`.
- Make commits atomic, i.e. small and focused using `git commit --amend` and `git commit --fixup`, better still make
  life easier using `git absorb`.
- `git rebase --interactive` can be used to squash commits.
- Keeping the commit history atomic and clean makes it easier to understand what work has been undertaken.
- Git periodically tidies things up for you with `git gc`.
- You can and should enable further automated cleaning by enabling `git mainenance` on a repository.

::::::::::::::::::::::::::::::::::::::::::::::::

## Links

- [Atomic commits will help you git legit. – Pauline Vos](https://www.pauline-vos.nl/atomic-commits/) the [video of her
  talk](https://www.youtube.com/watch?v=_e5oq4JT4_8) is well worth watching.
- [So You Think You Know Git](https://www.youtube.com/watch?v=aolI_Rz0ZqY) an excellent talk by Scot Chacon, one of the
founders of GitHub and co-author of [Pro Git][progit] book on useful tips for using Git.
- [Atlassian | Advanced Git Tutorials][advanced]

[advanced]: https://www.atlassian.com/git/tutorials/advanced-overview
[concommit]: https://www.conventionalcommits.org/en/v1.0.0/
[difftastic]: https://difftastic.wilfred.me.uk/
[emacs]: https://www.gnu.org/savannah-checkouts/gnu/emacs/emacs.html
[git]: https://git-scm.com
[gitabsorb]: https://github.com/tummychow/git-absorb
[githubdiff]: https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-comparing-branches-in-pull-requests#diff-view-options
[gitignore]: https://git-scm.com/docs/gitignore
[gitkraken]: https://www.gitkraken.com/
[gitmaintenance]: https://git-scm.com/docs/git-maintenance
[ignoregenerator]: https://www.toptal.com/developers/gitignore
[magit]: https://magit.vc/
[nano]: https://www.nano-editor.org/dist/latest/cheatsheet.html
[progit]: https://git-scm.com/book/en/v2
[r]: https://www.r-project.org
[regex]: https://regexr.com/
[rstudio]: https://posit.co/products/open-source/rstudio/
