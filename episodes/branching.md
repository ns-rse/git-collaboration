---
title: "Branching"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions

- What are branches?
- How do we use branches in git effectively?
- How can I check out other peoples branches whilst working on my own?
- How do I keep my development branch up-to-date with `main`?

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
update the code base, but what is a branch?

The word suggests an analogy with trees where branches are parts of a tree the extend from the "main" trunk or recursively
from parent "branches". An intuitive model of this is shown in the figure below.

<!-- Source for Mermaid diagram :
     https://gist.github.com/ns-rse/08fb86b003a26f7855281eeea88566d0#file-git_graph_branching_branches-js
-->
<!--
%%{init: {'theme': 'base', 'gitGraph': {'rotateCommitLabel': true}
         }
}%%
    gitGraph
       commit id: "0-472f101"
       commit id: "1-51816d6"
       commit id: "2-6769ff2 (base)"
       branch "branch"
       commit id: "3-8c52dce"
       checkout main
       commit id: "4-8ec389a"
       checkout "branch"
       commit id: "5-2315fa0 (HEAD)"
       checkout main
       commit id: "6-93e787c (HEAD)"
-->

![Basic GitHub Branches](https://mermaid.ink/img/pako:eNqVkTtrwzAUhf-KuWDcgh30sPVa29KlW7fiRZHkWKS2gitDU-P_XjshJUNKqab7-M65gjOBCdaBgjSdfO-jSqYstq5zmUqyrf5wWZ5kOx-fB31os3U7hKijewhd5-OL3rr3ZRqH0c11n1zeUs9peh5cxD9rc5Im3qqkBlSUnDQY4RpuA7iosMDMst8AUjDOZNOQ5G797_0Vtx10b9qFORe_OdBCmIpY466B1pl9GGPSad_flpWFcIYKqW_J_rpZFYTiqtHofzdZIanjgpsaIIfODQtql_SmFa_hlFwNK2n1sF-954UbD3aJ7Mn6GAZQa1g56DGG12NvLv2ZefR6N-juMjzo_i2E6xbUBJ-gWLXBkmAqCUKIlLLM4QiKSLmhAqFKcsIk54LOOXydDPCmZJRhxEqJkRCC8fkbTTe3dw?type=png){alt="Basic GitHub Branches with the `main` branch showing five commits and a `branch` forking off at the third commit with two commits of its own"}

The `branch` has two commits on it and stems from the parent `main` at a point referred to as `base`. A branch is _not_
just the two commits that appear to exist on it (i.e. `3-8c52dce` and `5-2315fa0`) rather it is the full commit history
of that lineage including the commits in the "parent". That means the `branch` consists of the commits `0-472f101`,
`1-98f9a30` and `2-6769ff2` as well as `3-8c52dce` and `5-2315fa0`.

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Take the time to make sure everyone understands what the graphic represents, explaining that each tag is a commit and
that the `branch` forks at a given point but doesn't have a commit associated with it.

The history of _both_ the `main` and the `branch` contain all points from the origin but

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

In a repository that is version controlled you will typically be checked out on the `HEAD` of a named branch. The `HEAD`
means the most recent commit in the history of that branch which on the `branch` is commit `5-2315fa0` whilst on `main`
the `HEAD` is `6-93e787c`.

You can change branches by using `git switch <branchname>`.

::::::::::::::::::::::::::::::::::::: callout

`git switch` was introduced in [Git
v2.23.0](https://github.blog/2019-08-16-highlights-from-git-2-23/#experimental-alternatives-for-git-checkout) along with
`git restore` to provide two separate commands for the functionality that was originally available in `git checkout`.
The main reason was to separate the functionality of `git checkout` which could "switch" branches, including creating
branches using the `--branch`/`-b` flag, and change ("restore") individual files with `git checkout [treeish] --
<filename>` (more on this later).

Splitting this functionality means that `git switch` is solely for `switch`ing branches whilst `git restore` is solely
concerned with `restore`ing files but is destructive and we will cover later the `git revert` command as an alternative.

`git checkout` has not been deprecated and is still available and many people still use it as old habits die hard.

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 1: What is the first and last commit on branch `divide`?

Using the `python-maths` repository you have cloned look up the first and last commit of the `divide` branch.

What are the commit hashes, commit messages, date/time and committers names?

:::::::::::::::::::::::: solution

## Solution

``` bash
git switch divide
git log --pretty="%h %ad (%cr) %x09 %an : %s"
* 6353fb4 - (HEAD -> divide, origin/divide) bug: Fix tpyo in divide function (2024-03-26 10:28:36 +0000) <Neil Shephard>
* 7485e56 - chore: Fix merge conflict (2024-03-26 10:28:11 +0000) <Neil Shephard>
* adfef4d - feat: Divide branch (2024-03-25 15:55:15 +0000) <Neil Shephard>
* 400896a - Divide branch (2024-03-25 15:55:15 +0000) <Neil Shephard>
* c1f64b0 - Setting up the repository for git-collaboration (2024-02-02 15:48:50 +0000) <Neil Shephard>
*   fa76751 - (origin/main, main) Merge pull request #6 from RSE-Sheffield/ns-rse/5-setup-clean-up (2023-10-19 22:46:14 +0100) <Neil Shephard>
|\
| * c8f0697 - 5 | Removing comment from setup.cfg (2022-10-04 11:12:23 +0100) <Neil Shephard>
* |   aff8153 - Merge pull request #7 from RSE-Sheffield/subtract-mistake (2023-01-20 10:07:58 +0000) <bobturneruk>
|\ \
| |/
|/|
| * a45a8dd - introduce mistake in subtract issue (2023-01-20 09:50:03 +0000) <Robert (Bob) Turner>
| * 604a397 - introduce delibarate mistake (2022-12-21 10:29:34 +0000) <Robert (Bob) Turner>
|/
*   f06c0ab - Merge pull request #4 from RSE-Sheffield/simplify_deliberate_errors (2022-06-07 14:58:27 +0100) <David Wilby>
|\
| * f55c0d2 - remove missing colon and no newline deliberate errors (2022-05-06 11:50:24 +0100) <David Wilby>
|/
* 5c9ae75 - correct python testing instruction (2021-05-18 16:15:23 +0300) <Anna Krystalli>
* 86d7633 - add correct details to each issue (2021-05-18 16:01:50 +0300) <Anna Krystalli>
* a58d6e7 - add all github issue templates (2021-05-17 13:43:57 +0300) <Anna Krystalli>
* 9429ab4 - complete subtract issue template (2021-05-14 15:53:25 +0300) <Anna Krystalli>
* bb560b0 - simplify function (2021-05-14 15:53:01 +0300) <Anna Krystalli>
*   325d038 - Merge pull request #1 from RSE-Sheffield/tests_changes (2021-05-14 14:40:36 +0300) <Anna Krystalli>
|\
| * 608ad59 - Restructure so tests pass (2021-05-14 12:24:23 +0100) <Will Furnass>
|/
* 8584b0f - correct pull request branch spec (2021-05-14 12:45:21 +0300) <Anna Krystalli>
* cdc9ea3 - correct push branch specification (2021-05-14 12:40:01 +0300) <Anna Krystalli>
* c01ff62 - add instructions to README (2021-05-14 12:38:29 +0300) <Anna Krystalli>
* 585287a - add test and CI (2021-05-14 12:38:09 +0300) <Anna Krystalli>
* 3f4d54b - rename python_package folder (2021-05-14 12:37:48 +0300) <Anna Krystalli>
* 4b1707b - use requirements.txt instead of env.yml (2021-05-14 10:04:02 +0100) <davidwilby>
* 2556966 - remove build specs from conda env (2021-05-14 10:01:28 +0100) <davidwilby>
* b50e658 - move env.yml to right place.. (2021-05-14 09:54:59 +0100) <davidwilby>
*   0d2f520 - Merge branch 'main' of github.com:RSE-Sheffield/python-calculator into main (2021-05-14 09:53:44 +0100) <davidwilby>
|\
| * b1179a7 - add package name folder (2021-05-14 11:33:06 +0300) <Anna Krystalli>
* | c883789 - add conda environment yaml (2021-05-14 09:53:06 +0100) <davidwilby>
|/
* fdb8716 - draft commit (2021-05-14 11:23:42 +0300) <Anna Krystalli>
* 328e61b - Add subtraction issue template (2021-05-13 12:23:42 +0300) <Anna Krystalli>
* 31a4a93 - Initial commit (2021-05-13 12:14:08 +0300) <Anna Krystalli>
```

From the `git log` graph we see the first and last commits were.

| Commit | Hash    | Message                          | Date/time           | Committer      |
|:-------|:--------|:---------------------------------|:--------------------|:---------------|
| First  | 31a4a93 | Initial commit                   | 2021-05-13 12:14:08 | Anna Krystalli |
| Last   | 6353fb4 | bug: Fix tpyo in divide function | 2024-03-26 10:28:36 | Neil Shephard  |

:::::::::::::::::::::::::::::::::

## Challenge 2: What commit did the `multiply` branch diverge from `master`?

Again using the `python-maths` repository switch to the multiply. Use `git log` what is the commit that `multiply`
diverged from `master`. How many commits have been made on the `multiply` branch?

:::::::::::::::::::::::: solution

## Solution

``` bash
git switch multiply
git log --graph --pretty="%h %ad (%cr) %x09 %an : %s"
* b702501 - (HEAD -> multiply, origin/multiply) bug: multiply instead of add arguments (2024-03-26 10:33:37 +0000) <Neil Shephard>
* 11e36a3 - feat: Adding multiply function and tests (2024-03-26 10:32:42 +0000) <Neil Shephard>
* c1f64b0 - Setting up the repository for git-collaboration (2024-02-02 15:48:50 +0000) <Neil Shephard>
*   fa76751 - (origin/main, main) Merge pull request #6 from RSE-Sheffield/ns-rse/5-setup-clean-up (2023-10-19 22:46:14 +0100) <Neil Shephard>
|\
| * c8f0697 - 5 | Removing comment from setup.cfg (2022-10-04 11:12:23 +0100) <Neil Shephard>
* |   aff8153 - Merge pull request #7 from RSE-Sheffield/subtract-mistake (2023-01-20 10:07:58 +0000) <bobturneruk>
|\ \
| |/
|/|
| * a45a8dd - introduce mistake in subtract issue (2023-01-20 09:50:03 +0000) <Robert (Bob) Turner>
| * 604a397 - introduce delibarate mistake (2022-12-21 10:29:34 +0000) <Robert (Bob) Turner>
|/
*   f06c0ab - Merge pull request #4 from RSE-Sheffield/simplify_deliberate_errors (2022-06-07 14:58:27 +0100) <David Wilby>
|\
| * f55c0d2 - remove missing colon and no newline deliberate errors (2022-05-06 11:50:24 +0100) <David Wilby>
|/
* 5c9ae75 - correct python testing instruction (2021-05-18 16:15:23 +0300) <Anna Krystalli>
* 86d7633 - add correct details to each issue (2021-05-18 16:01:50 +0300) <Anna Krystalli>
* a58d6e7 - add all github issue templates (2021-05-17 13:43:57 +0300) <Anna Krystalli>
* 9429ab4 - complete subtract issue template (2021-05-14 15:53:25 +0300) <Anna Krystalli>
* bb560b0 - simplify function (2021-05-14 15:53:01 +0300) <Anna Krystalli>
*   325d038 - Merge pull request #1 from RSE-Sheffield/tests_changes (2021-05-14 14:40:36 +0300) <Anna Krystalli>
|\
| * 608ad59 - Restructure so tests pass (2021-05-14 12:24:23 +0100) <Will Furnass>
|/
* 8584b0f - correct pull request branch spec (2021-05-14 12:45:21 +0300) <Anna Krystalli>
* cdc9ea3 - correct push branch specification (2021-05-14 12:40:01 +0300) <Anna Krystalli>
* c01ff62 - add instructions to README (2021-05-14 12:38:29 +0300) <Anna Krystalli>
* 585287a - add test and CI (2021-05-14 12:38:09 +0300) <Anna Krystalli>
* 3f4d54b - rename python_package folder (2021-05-14 12:37:48 +0300) <Anna Krystalli>
* 4b1707b - use requirements.txt instead of env.yml (2021-05-14 10:04:02 +0100) <davidwilby>
* 2556966 - remove build specs from conda env (2021-05-14 10:01:28 +0100) <davidwilby>
* b50e658 - move env.yml to right place.. (2021-05-14 09:54:59 +0100) <davidwilby>
*   0d2f520 - Merge branch 'main' of github.com:RSE-Sheffield/python-calculator into main (2021-05-14 09:53:44 +0100) <davidwilby>
|\
| * b1179a7 - add package name folder (2021-05-14 11:33:06 +0300) <Anna Krystalli>
* | c883789 - add conda environment yaml (2021-05-14 09:53:06 +0100) <davidwilby>
|/
* fdb8716 - draft commit (2021-05-14 11:23:42 +0300) <Anna Krystalli>
* 328e61b - Add subtraction issue template (2021-05-13 12:23:42 +0300) <Anna Krystalli>
* 31a4a93 - Initial commit (2021-05-13 12:14:08 +0300) <Anna Krystalli>
```

This is a little more challenging to interpret but reading the output carefully we have an indicator of where the
`origin/main` branch is where it reads `(origin/main, main)`. All subsequent commits are on the currently checked out
branch which is `multiply` and `origin/multiply` (i.e. the local copy of the branch is at the same point as the remote
on GitHub).

Knowing this we can see that the `multiply` branch diverged from the `fa76751` commit on `main` and that three commits
have been made on the `multiply` branch.

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: callout

There are a _lot_ of options for formatting the output of `git log` that allow you to restrict output to just the
information you are interested in and change the display. For this course we use `--graph` and the basic
`--pretty="%h %ad (%cr) %x09 %an : %s`.

For convenience you can save this as an alias using the following.

``` bash
alias gl='git log --graph --pretty="%h %ad (%cr) %x09 %an : %s"'
```

You can persist this between sessions by adding the above line to `~/.bashrc` or `~/.bash_aliases` if you use the
[Bash][bash] shell. If you use the [ZSH][zsh] shell the framework [Oh My Zsh][ohmyzsh] includes a number of aliases
for formatting Git log output (e.g. `glod`).

::::::::::::::::::::::::::::::::::::::::::::::::

## Working with Branches

The `git switch` command is the common method for working with branches. It allows you to list, create and delete
branches along with a few other tasks.

To list the branches that are available you can just type `git branch` or optionally include the `--list` option. In the
`python-maths` repository you  have cloned you should see a number of branches listed. The branch you are currently
checked out on is listed first with an asterisk (`*` )at the start and they are listed alphabetically. Later we will
change the default order to be more informative.

``` bash
git branch

divide
main
* multiply
ns-rse/initial-setup
```

### Creating Branches

You can create a new branch using `git switch -c <new_branch>`. By default it will use the branch you currently have
checked out as a basis for the new branch. If you wish to use a different branch as a basis you can do so by including
its name before the name of the new branch.

::::::::::::::::::::::::::::::::::::: callout

Most of the time when creating branches you should do so from the `main` branch. It is therefore important to make sure
your local copy of the `main` branch is up-to-date. Before creating a branch you should checkout the `main`branch and
ensure it is up-to-date.

``` bash
git switch main
git pull
```

This means you can omit the explicit statement of which branch you wish to use as the basis for the new branch,
typically `main`, when creating it as you will be already be checked out on that branch when `git pull`.

::::::::::::::::::::::::::::::::::::::::::::::::

To create a new branch called `ns-rse/test` you can use the following.

``` bash
git switch main
git pull
git switch -c main ns-rse/test
```

Git will use the current `HEAD` of the `main` branch as a basis for creating the `ns-rse/test` branch.

### Naming Branches

Branch names can not include spaces, you should use underscores or dashes instead. You can include some special
characters too but I would avoid using `#` as this is the character used by most shells to indicate a comment and you
would therefore have to _always_ double quote the branch name at the command line.

A useful convention when creating branches is to include some meta data about who owns the branch and what it is for and
to construct the branch name from your GitHub/GitLab username followed by a `/` and because you will typically be
working on a particular issue include the issue number followed by a short few words which describe the work or
issue. For example GitHub user `ns-rse` working on issue 1 to fix typehints might create a branch called
`ns-rse/1-fix-typehints` from main.

This structure is informative as it provides other people you collaborate with or who look at the repository an
indication of who created the branch, what issue they are working on and a very short indication of what it is concerned
about. With this information it is very easy to look up the relevant Issue.

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 3: Assign Issues, Create Branches and Complete the Tasks

In the `python-maths` repository you have cloned and setup on GitHub there are issue templates.

In your pairs assign the `01 Add zero division exception and test` to one person and the `02 Add a square root function
and test` to the other person.

Work through the tasks adding the necessary code, saving, staging and committing your changes then pushing to `origin`
(GitHub). **NB** only the first issue for zero division should have a Pull Request created, please do _not_ create a
pull request or merge the Square Root work.

Assign the person who worked on the Square root function to review the Zero Division exception and if everything looks
good merge the pull request.

:::::::::::::::::::::::: solution

## Solution - 01 Zero Division Exception

``` bash
git switch main
git switch -c main ns-rse/1-zero-divide-exception
# MAKE EDITS
git add -u
git commit -m "Add Zero division exception and test"
git push --set-upstream origin ns-rse/1-zero-divide-exception
```

:::::::::::::::::::::::::::::::::

:::::::::::::::::::::::: solution

## Solution - 02 Square root function

``` bash
git switch main
git pull
git switch -c ns-rse/2-square-root
# MAKE EDITS
git add -u
git commit -m "Adds square root function"
```

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

### Deleting branches

Branches are typically short lived as they are created to address small focused pieces of work such as fixing a bug or
implementing a new feature before being merged into the `main` branch. Over time you will accrue a number of redundant,
out-dated branches and it is therefore good practice to delete unwanted branches after they have been merged.

You can not delete a branch you currently have checked out so you must first checkout an alternative branch. Typically
this would be the `main` branch after your Pull Request has been merged and the changes you were working on have been
incorporated. You should `git pull` the `main` branch after merging changes so your locally copy is aware of any recent
merges from branches you are about to delete.

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 4: Delete a branch

Create a throw away branch from `main` and then delete it (hint see `git branch --help`). You can create a branch with
your username and `throwaway` (e.g. `ns-rse/throwaway`) with the following.

``` bash
git checkout main
git pull
git switch -c ns-rse/throwaway
```

Pretending the branch you just created has been merged into the `main` branch via a Pull Request delete the now
redundant branch (in this example `ns-rse/throwaway`).

:::::::::::::::::::::::: solution

## Solution 1

You can use the `-d` or `--delete` flag to delete a branch.

``` bash
git switch main
git branch -d ns-rse/throwaway
```

:::::::::::::::::::::::::::::::::

:::::::::::::::::::::::: solution

## Solution 2

You can use the shortcut `git switch -` to switch to the last branch you were on (this is a shortcut that is common to
the Bash shell when navigating directories too, `cd -` will change directory to the previous directory you were in).

``` bash
git switch -
git branch -d ns-rse/throwaway
```

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: callout

You were able to delete the branch you created because you hadn't made any changes to it. If you have made changes on a
branch and they have not been merged into `main` then Git will warn you of this and refuse to delete the branch. This
can be over-ridden with the `--force` flag or the shorthand `-D` which is the same as `--delete --force`.

``` bash
git switch -c ns-rse/throwaway
touch test_file
git add test_file
git commit -m "Adding test_file"
git switch -
git branch -d ns-rse/throwaway
error: the branch 'ns-rse/throwaway' is not fully merged
hint: If you are sure you want to delete it, run 'git branch -D ns-rse/throwaway'
hint: Disable this message with "git config advice.forceDeleteBranch false"
git -D ns-rse/0-divide
```

Be _very_ careful when forcing deletions, if you have not pushed your changes to the remote `origin` then you _will_
lose them.

::::::::::::::::::::::::::::::::::::::::::::::::

## Time Travelling - Losing your `HEAD`

A branch is a history of commits and you can use `git log` to see the commit history (and customise the output so it can
be easier to read), but what if you wanted to look at the state of the branch at a previous point in time? Well because
Git has kept track of everything you can do that and the command to do so is the same one for switching branches
i.e. `git switch` which takes a "reference" as an argument. So far you have been using branch names as references but
commit hashes are also references and so can be used to checkout the state of the repository in the past.

<!-- Source for Mermaid diagram :
     https://gist.github.com/ns-rse/08fb86b003a26f7855281eeea88566d0#file-git_graph_branching_diverging_checkout-js
-->
<!--
%%{init: {'theme': 'base', 'gitGraph': {'rotateCommitLabel': true}
         }
}%%
    gitGraph
       commit id: "0-472f101"
       commit id: "1-51816d6"
       commit id: "2-6769ff2"
       commit id: "3-8c52dce"
       commit id: "4-8ec389a"
       commit id: "5-2315fa0"
       commit id: "6-93e787c"
       commit id: "7-bc43901"
       commit id: "8-a80cef8" tag: "HEAD"

-->

![A linear Git History on the `main` branch showing the position of `HEAD`.](https://mermaid.ink/img/pako:eNp1kT1rwzAQhv-KOTBerCDJ1oe1lSa0Q7duxYsiyYlobAdHgabG_71ygguFWpPuuecOjncE01sHCtJ09J0PKhmzcHSty1SS7fXFZXmSHXx4GfT5mM3doQ86uOe-bX1403t3ijQMVzfVXbK8-J_S9AGW4d-2uY8m3qqkBoxKQRuCSQ3_CwQxIgm3fE2giAteNQ1dEwokDaPWuDWhRNKZQlZ6TWCIFoQ1Gq8JHFWFE1KYNUGgvSmLav1MibTExjWyhiTow4xed0_bRYccWje02tsY1TizGu4x1TCrVg-fszpF73q2MZ-d9aEfQM3J5KCvoX-_dWapH87W68OgW1CNPl0iPevuo-__1KBG-AJFRblhFDMuZSk445TlcItYlhuBaYEJpixex6ccvu8b8EYyIbGsGClwbFA6_QAk7q7t?type=png){alt="A linear Git History on the `main` branch showing the position of `HEAD`."}

Here we have a simple linear history and the `HEAD` of branch is on commit `8-a80cef8` If you want to checkout commit
`4-8ec389a` then you would `git switch 4-8ec389a` and you will see the following useful and informative warning
message.

``` bash
git switch 4-8ec389a
Note: switching to 4-8ec389a'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 4-8ec389a complete subtract issue template
```

Have you lost your head because it is now `detached`? No, `HEAD` is just a special reference that points to a specific
commit, tags are the same, they are short hand ways of referring to commits, what has happened is that Git has moved the
commit it points to from `8-a40cef8` to `4-8ec389a`. If you make changes to this branch they will be lost when you
switch back to the commit `HEAD` points to and you are told you can do this with `git switch -`. If you want to make
changes and save them you are advised to create a new branch to do so.

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 5: Checkout old commits

- Look at the history of the `python-maths` repository and find out who the author of commit `585287a` was.
- Checkout this commit and look at the contents of the file `tests/test_add.py` (you can use `cat tests/test_add.py`).
- Switch back to `HEAD` has anything changed in the `tests/test_add.py` file?

:::::::::::::::::::::::: solution

``` bash
git switch 585287a
cat tests/test_add.py

import src.python_calculator.add as add


def test_add():
    assert add.add(1, 3) == 4

git switch -
cat tests/test_add.py

cat: tests/test_add.py: No such file or directory
```

The file `tests/test_add.py` has an import statement and defines the `test_add()` function which checks if the
`add.add()` function returns the value of 4 when given the numbers 1 and 3.

The `tests/test_add.py` file no longer exists on the `HEAD` of the `main` branch!

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: callout

You are not restricted to switch to commits on the same branch you are currently on. You can checkout any commit in
the history as long as you know the commit hash.

::::::::::::::::::::::::::::::::::::::::::::::::

## Comparing References

This is quite a convoluted way of comparing branches though and in this instance the difference is quite simple the file
no longer exists, but imagine you wanted to compare a file between branches or commits _without_ having to switch
branches and try and hold in your head what the file looked like on one branch whilst you look at the other. That would
probably be very challenging.

Fortunately Git can help you here with `git diff`. This takes one or two arguments, which are commits or references that
you want to compare. If only one argument is given it compares the currently checked out commit to the supplied
commit/reference.

Thus to compare the `HEAD` of the `divide` branch you would

``` bash
git checkout ns-rse/1-zero-divide-exception
git diff 585287a
# Equivalent to...
git diff HEAD 585287a
```

## Ooops! I Did It Again

Nothing to do with Brittney Spears but you are at some stage likely to commit changes to the wrong branch. This can
easily happen when starting to work on an issue without first creating a new branch to contain the work and you commit
the changes to either the `main` branch, which is often protected so you won't be able to push your changes or the last
branch you were working on.

### `git reset`

One solution to solve this with Git is to `git reset` the branch to which you have just mistakenly made the commit. This
removes reference to the changes from the Git history but leaves the changes to the files in place and they appear as
`unstaged` files. It is ideal if you have only _one_ commit you wish to undo.

#### Relative Refs

Normally you are working on the `HEAD` of a branch which is the most recent commit that has been made along with any
staged, but uncommitted changes. Git has a simple way of referring to previous commits relative to `HEAD` using the `~`
and counting backwards.

<!-- Source for Mermaid diagram :
     https://gist.github.com/ns-rse/08fb86b003a26f7855281eeea88566d0#file-git_graph_branching_relative_refs-js
-->
<!--
%%{init: {'theme': 'base', 'gitGraph': {'rotateCommitLabel': true}
         }
}%%
    gitGraph
       commit id: "0-472f101" tag: "~8"
       commit id: "1-51816d6" tag: "~7"
       commit id: "2-6769ff2" tag: "~6"
       commit id: "3-8c52dce" tag: "~5"
       commit id: "4-8ec389a" tag: "~4"
       commit id: "5-2315fa0" tag: "~3"
       commit id: "6-93e787c" tag: "~2"
       commit id: "7-bc43901" tag: "~1"
       commit id: "8-a80cef8" tag: "HEAD"

-->
![Relative Refs on a Git
Branch](https://mermaid.ink/img/pako:eNp10k1rgzAYB_CvIg8UL6bkxbzobaxlO-y22_CSxtiGVS02hXXiPvtii5uDmVPyf34JJE96MG1pIYfVqneN83nUx_5gaxvnUbzTZxsnUbx3_qnTp0M8VrvWa28f27p2_kXv7DGkvrvYoWiiaYT5sFrdg2nzT9nctkauzKMCMEolrQgmBURe78foSxXwPyaIE0VEKWZYLmGKhBRZVdEZFkuYIWU4LY2dYb6EU6SsYSrTM5wuYY4oI7zSeIbZEhYoY1YqaWaYLmGJdiZl2Z-nI0tYIa2wsZX6xc_bh83EIYHadrV2ZfgK_ZgVcPsGBYy01N37SIfgLqcy9H9bOt92kI-dT0BffPt6bcy0vpuN0_tO15BX-ngO6Uk3b237Zw15Dx-QU5muOcVcKJVKwQXlCVxDrNK1xJRhgikPNxVDAp-3E_BacamwyjhhOBQoHb4BB-fKEQ?type=png){alt="Relative references on the `main` branch with 9 commits showing the commit hash and the reference relative to the `HEAD`"}

If you want to undo the _last_ commit then you can do this using `git reset --soft HEAD~1`.

``` bash
touch test_file
git add test_file
git commit -m "Adding test_file"
git reset --soft HEAD~1
```

::::::::::::::::::::::::::::::::::::: callout

There are three options to `git reset` that influence how the changes in commits are handled these are `--soft`,
`--mixed` (the default) and `--hard`.

For a detailed exposition of `git reset` see the excellent [Atlassian | Git reset][atlassian_git_reset] article.

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 6: Commits on the wrong branch

- Switch to the `main` branch of the `pytest-maths` repository.
- Create a new file using `echo "# How to Contribute to this repo" > CONTRIBUTING.md`
- Stage and commit the file to the `main` branch of your repository. **NB** to do this you will have to disable the
  `pre-commit` checks with the `-n` flag.

Ooops you've just committed to the `main` branch which is protected so you can't push your changes. Now move the commit
to a new branch so you can push them.

- Reset the change.
- Create a new branch called `<github_user>/contributing`.
- Stage and commit the file to `<github_user>/contributing`.

:::::::::::::::::::::::: solution

## Solution

You can `git reset --mixed` to `HEAD~1`, i.e. the previous commit, which removes the `CONTRIBUTING.md` file from the
commit history, leaving it unstaged, then create a new branch and add it to that.

``` bash
git switch main
echo "# How to Contribute to this repo" > CONTRIBUTING.md
git add CONTRIBUTING.md
git commit -n -m "docs: Adding contributing guideline template"
git reset --mixed HEAD~1
git switch -c ns-rse/contributing
git add CONTRIBUTING.md
git commit -m "docs: Adding contributing guideline template"
```

:::::::::::::::::::::::::::::::::

:::::::::::::::::::::::: solution

## Solution

Alternatively you can checkout the previous commit _before_ you added the file by mistake, create the
`<github_user>/contributing` branch,  and `git cherrypick` the commit from `main` which contains the `CONTRIBUTING.md`
file and _then_ remove the commit from main.

``` bash
git switch main
echo "# How to Contribute to this repo" > CONTRIBUTING.md
git add CONTRIBUTING.md
git commit -n -m "docs: Adding contributing guideline template"
git log              # Note the commit of the mistaken hash
git revert HEAD~1    # Checkout the previous commit on the main branch
git switch -c ns-rse/contributing
git cherrypick <hash>
git switch main
git reset --hard HEAD~1
```

:::::::::::::::::::::::::::::::::

:::::::::::::::::::::::: solution

## Solution

A third similar option is checkout the previous commit _before_ you added the file by mistake, create the
`<github_user>/contributing` branch, and copy the `CONTRIBUTING.md` file from the `HEAD` of `main` using `git restore`
and _then_ remove the commit from main.

``` bash
# TODO get commit hash of last commit
git checkout HEAD~1
git switch -c ns-rse/contributing
git restore -s main -- CONTRIBUTING.md # Copy the file from HEAD of main branch or
git add CONTRIBUTING.md
git commit -m "Adding contributing guideline template"
git switch -   # Switch back to main
git
# TODO : Complete solution and add output once sample repository is in place
```

**NB** You could also copy the file using the older `git checkout main -- CONTRIBUTING.md`.

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

You then have to decide how to add the changes to a branch. If they are brand new then you can create a new branch and
add them. If however they were meant to be added to an existing branch you face a slight problem as if you try to switch
branches you will be told that this would over-write the changes to the files you have just modified and unstaged and
you don't want to lose your work.

The solution here is to use `git stash` to temporarily store the unstaged changes, switch branches to the target branch
they should be on, and you can then un-stash them (known as `pop`ing) onto the correct branch.

### `git revert`

`git reset` is destructive, you can lose work using it and it is advisable _not_ to use it when you have more than one
commit you wish to undo as you lose the intermediary work between commits as you are restored to the commit you reset
to. Fortunately Git has the `revert` option is a non-destructive approach to undoing changes in your Git
history. Instead it takes a specified commit and inverts the changes, i.e. goes back to the previous state and rather
than discarding the changes it makes a new "revert" commit to record the inversion and this new "revert" commit becomes
the `HEAD` of the branch. `git revert` _has_ to have a reference in order to work, whether that is absolute (i.e. a
hash) or relative.

::::::::::::::::::::::::::::::::::::: callout
The differences between `reset` and `revert` is that one (`reset`) is destructive and loses changes the other (`revert`)
undoes the changes and makes a new commit recording these changes.

Be _very_ careful when forcing deletions, if you have not pushed your changes to the remote `origin` then you will lose
them.
::::::::::::::::::::::::::::::::::::::::::::::::

## Diverging Branches

As you and your collaborator(s) work on your repository you may find that changes others have made get merged into the
`main` before you have finished your work. This has in fact just happened, the work to add a Zero Division exception
has been merged via a Pull Request, but the work to address the Square Root function hasn't and is in effect behind the
`main` branch. The following is a representation of the current state, albeit from a single developer.

<!-- Source for Mermaid diagram :
     https://gist.github.com/ns-rse/08fb86b003a26f7855281eeea88566d0#file-git_graph_branching_diverging_branches-js
-->
<!--
%%{init: {'theme': 'base', 'gitGraph': {'rotateCommitLabel': true}
         }
}%%
    gitGraph
       commit id: "0-472f101"
       commit id: "1-51816d6"
       commit id: "2-6769ff2 (base)"
       branch "ns-rse/1-zero-division"
       branch "ns-rse/2-square-root"
       checkout "ns-rse/1-zero-division"
       commit id: "3-8c52dce"
       checkout "ns-rse/2-square-root"
       commit id: "4-8ec389a"
       checkout "ns-rse/1-zero-division"
       commit id: "5-2315fa0"
       checkout "ns-rse/2-square-root"
       commit id: "6-93e787c"
       checkout main
       merge "ns-rse/1-zero-division" id: "7-bc43901" tag: "Merge zero division"
       commit id: "HEAD"

-->

![Diverging branches in `python-maths` `ns-rse/2-square-root` is now behind the `main` branch which has incorporated the changes from `ns-rse/1-zero-division.`](https://mermaid.ink/img/pako:eNqtklFv2yAUhf8KQoq8SSYDbDD4bVqr9WF72tvkF4JxglqbDOOpreX_PkiaKpXadNJmP9j38N1zD9KdoXatgTVcrWY72FCDOQs705usBtlGjSbLQba14atX-12WTr0LKpgvru9t-KY25i6qwU9maQZweuL_slodhVPz87E-tALb1qCBGJUV7QgmDXwdIIgRQXjL3wIo4hWXXUfBh5T34xm38WrQu8gMI_Kj-UTQo_EOtfa3Ha0bLpAUjb8m5Q3yzoXzyTujb90U_sbzRcoCCc1oq81FszfHnluVSBhdCKn-Ry6GaEFYp_C_5-JIFqYSlX7Nqld2eFZ747fmQtQnwwptdFnItBsgqG2Svh86Ew7eu9nN9eerBsIcxmlxeht3fE5kAw_73cAEtcrfJoMlctO-jYt93drgPKzTSudQTcH9eBj0qT4yV1ZtvepP4l4NP52LZafuxmMN6xnewxqVAq8lq-KLZcmpECyHD7DmeM0xZfFqRMr4kUsOHw8WdI1piQvBSMEKLmW1_AHrjxWc?type=png){alt-text="Diverging branches in `python-maths` `ns-rse/2-square-root` is now behind the `main` branch which has incorporated the changes from `ns-rse/1-zero-division`."}

In this example the `main` branch now includes the commits `3-8c52dce` and `5-2315fa0` from the `ns-rse/1-zero-division`
branch as well as the commit `7-bc43901` which was made when the `ns-rse/1-zero-division` branch was merged in. The
`ns-rse/2-square-root` branch does _not_ contain these commits.

In this particular example that is not necessarily a problem, the two features/issues are completely independent and it
would be possible to merge the `ns-rse/2-square-root` branch into `main` without any merge conflicts because neither
have modified the same files in the same location.

That will not always the case though, sometimes merge conflicts might arise if the second branch is changing some of the
same files as the first branch. Another scenario might be that whilst work was being done on adding a new feature branch
a critical bug was fixed that the new feature depends on and the changes now in `main` need incorporating in the feature
branch.

There are two approaches to solving this merging (`git merge`)  and rebasing (`git rebase`).

### Merging

<!-- Source for Mermaid diagram :
     https://gist.github.com/ns-rse/08fb86b003a26f7855281eeea88566d0#file-git_graph_branching_merging-js
-->
<!--
%%{init: {'theme': 'base', 'gitGraph': {'rotateCommitLabel': true}
         }
}%%
    gitGraph
       commit id: "0-472f101"
       commit id: "1-51816d6"
       commit id: "2-6769ff2 (base)"
       branch "ns-rse/1-zero-division"
       branch "ns-rse/2-square-root"
       checkout "ns-rse/1-zero-division"
       commit id: "3-8c52dce"
       checkout "ns-rse/2-square-root"
       commit id: "4-8ec389a"
       checkout "ns-rse/1-zero-division"
       commit id: "5-2315fa0"
       checkout "ns-rse/2-square-root"
       commit id: "6-93e787c"
       checkout main
       merge "ns-rse/1-zero-division" id: "7-bc43901" tag: "v0.1.1"
       commit id: "HEAD"
       checkout "ns-rse/2-square-root"
       merge "main" id: "8-a80cef8" tag: "Merge main"

-->

![Merging `README.md` into `main` then merging `main` into `LICENSE` to bring it up-to-date](https://mermaid.ink/img/pako:eNqtU01vnDAU_CvI0opWwtQ2-ANuVRO1h_bUW8XFa8yulYC3xkRNEP-9BkK6lbKrqq1PvHkz8wbLbwTK1hqUYLcbTWd8GY2xP-pWx2UU72Wv4ySKD8Z_dPJ0jOeus156_cG2rfGf5V7fB9S7QU9VF20nfE-73Qps4pe2WqSRqcuoAgjmnDQY4Qq8TsCQYoFZzS4RCGScFU1Dojdz3rdnvL2TnToGTtdD1-t3GD5pZ2FtHkxvbHeFSWD_fZBOQ2etP5981OrODv5PPH9LmUGhKKmVvmp2cey5VQ6FVpko5P_IRSHJMG0k-vdcDBaZ5oKr16xaaboXtNXuoK9EfTbkcK_yrJjfRuTlYYYeUIrTi2_l0-37m7_6jy3QknIbL6AUSOlG_Br_ZeGtrFUMEhDEAanDDo0zVoFlfyowC2rp7mbqFHjDqQ6Lc1sbbx0o55VJgBy8_frYqa1eOTdGHpxsN_Aku2_WhrKR9_1ag3IEP0BJeJ5SgigTIueMMkIT8BhgkacckQxhRGi4QDYl4GlxQKmgXCBRUJyh0CBk-gnhvTAe?type=png){alt-text="Merging `README.md` into `main` then merging `main` into `LICENSE` to bring it up-to-date"}

The syntax of `git merge` is

``` bash
git merge <OPTIONS> <ref>
```

Where `<ref>` is one of a commit, branch name or tag (both of which are references to commits). There is an option for
how the merge is made known as `fast-forward`. Fast-forward is the default action unless annotated tags are being merged
that is in the incorrect hierarchy. To explicitly enabled this behaviour (`--ff`) and the branch pointer, that is where
the current branch diverged from the the `main` branch) is updated to point to the most recent commit on the `main`
branch.

Typically though the `main` branch contains work from someone else's branch and so the

Lets go through this process by...

1. Making a new repository.
2. Create `branch1`, add a `README.md` and commit it.
3. Switch back to `main`.
4. Create `branch2`, add a `LICENSE` and commit it.
5. Merge `branch1` into `main` (equivalent to making a Pull Request).
6. Merge `main`, which now contains `README.md`, into `branch2`.
7. Merge `branch2` into `main`.
8. Delete `branch1` and `branch2`.

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Remember to take the time to show the contents of the files and how they "disappear" when switching branches, in
particular after having added `README.md` to `branch1` and switching back to `main`.

Also use `glod` alias (or other form of `git log` that shows branches) to show the changes and explain the point at
which each of the branches is at with reference to the `*` indicating commits, the branch names and where they sit and
the date/time stamps.

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

#### 1. Make a new repository

``` bash
cd ~/work/git/hub/ns-rse
mkdir git-merge-test
cd git-merge-test
git init --initial-branch=main
git commit --allow-empty -m "Initial commit"
```

#### 2. Create `branch1`, add a `README.md` and commit it

``` bash
git switch -c branch1
echo "# Just a test" > README.md
git add README.md
git commit -m "docs: Adding README.md"
glod
```

#### 3. Switch back to `main`

Check the contents of `README.md` (there is no such file as the it exists on `branch1`).

``` bash
git switch main
cat README.md   # Note that `README.md` does not currently exist on this branch
glod
```

#### 4. Create `branch2`, add a `LICENSE` and commit it

``` bash
git switch -c branch2
echo "YOU CAN DO WHAT YOU WANT WITH THIS CODE" > LICENSE
git add LICENSE
git commit -m "Adding a LICENSE"
```

#### 5. Merge `branch1` into `main`

Switch back to `main` and merge `branch1` (this is equivalent to merging a Pull Request). The file `README.md` now
exists on the `main` branch.

``` bash
git switch main
git merge branch1
cat README.md
glod
```

#### 6. Merge `main`, which now contains `README.md`, into `branch2`

Switch to `branch2` which has now diverged as it contains changes of its own _and_ `main` contains the changes made on
`branch1`.  We want to merge the changes on `main` and "fast-forward" if possible.

``` bash
git switch branch2
git merge --ff main # Merge changes merged into main from branch1 into branch2
glod

*   d914fee - (HEAD -> branch2) Merge branch 'main' into branch2 (2024-03-01 12:02:08 +0000) <Neil
|\
| * 7817070 - (main, branch1) Adding a README.md (2024-03-01 11:57:35 +0000) <Neil Shephard>
* | a14a643 - Adding a LICENSE (2024-03-01 12:00:39 +0000) <Neil Shephard>
|/
* 1bd6bb8 - Initial commit (2024-03-01 11:57:06 +0000) <Neil Shephard>
```

#### 7. Merge `branch2` into `main`

We now have the changes from `branch1` included in `branch2` by virtue of having merged `main`. If we switch back to
`main` we can merge the changes from `branch2`.

``` bash
git switch main
git merge branch2
glod
*   d914fee - (HEAD -> main, branch2) Merge branch 'main' into branch2 (2024-03-01 12:02:08 +0000) <Neil Shephard>
|\
| * 7817070 - (branch1) Adding a README.md (2024-03-01 11:57:35 +0000) <Neil Shephard>
* | a14a643 - Adding a LICENSE (2024-03-01 12:00:39 +0000) <Neil Shephard>
|/
* 1bd6bb8 - Initial commit (2024-03-01 11:57:06 +0000) <Neil Shephard>
```

#### 8. Delete `branch1` and `branch2`

As we're done with `branch1` and `branch2` we can delete them.

``` bash
# Delete the two branches
git branch -d branch{1,2}
glod
*   d914fee - (HEAD -> main) Merge branch 'main' into branch2 (2024-03-01 12:02:08 +0000) <Neil Shephard>
|\
| * 7817070 - Adding a README.md (2024-03-01 11:57:35 +0000) <Neil Shephard>
* | a14a643 - Adding a LICENSE (2024-03-01 12:00:39 +0000) <Neil Shephard>
|/
* 1bd6bb8 - Initial commit (2024-03-01 11:57:06 +0000) <Neil Shephard>
```

Having used `git merge` we couldn't perform a simple fast-forward because the history of `main` now contained changes
that were made on `branch1` and so a separate commit (`d914fee`) was made to merge the `main` branch into `main`
(commits are denoted by `*` and so you can see the commits were made on separate branches). We can see from the graph
that `README.md` was added from a separate `branch1` and `LICENSE` was added from `branch2`, although after deleting the
branches they are no longer shown by name in the `git log --graph` output.

### Rebasing

Rebasing moves the point at which the branch diverged from its original position to another, in this case the `HEAD` of
the `main` branch. You are changing the `base` commit, hence the name `git rebase`.

<!-- Source for Mermaid diagram :
     https://gist.github.com/ns-rse/08fb86b003a26f7855281eeea88566d0#file-git_graph_branching_rebase-js
-->
<!--
%%{init: {'theme': 'base', 'gitGraph': {'rotateCommitLabel': true}
         }
}%%
    gitGraph
       commit id: "0-472f101"
       commit id: "1-51816d6"
       commit id: "2-6769ff2 (base)"
       branch "ns-rse/1-zero-division"
       checkout "ns-rse/1-zero-division"
       commit id: "3-8c52dce"
       checkout "ns-rse/1-zero-division"
       commit id: "5-2315fa0"
       checkout main
       merge "ns-rse/1-zero-division" id: "7-bc43901" tag: "v0.1.1"
       commit tag: "Rebase"
       branch "ns-rse/2-square-root"
       checkout "ns-rse/2-square-root"
       commit id: "4-8ec389a"
       checkout "ns-rse/2-square-root"
       commit id: "6-93e787c"

-->

![Git Rebase to bring the diverged branch up-to-date with `main`.](https://mermaid.ink/img/pako:eNqtks1unDAUhV8FWRrRSpjYBv_ANqm66ardVWw8tpmxEvDEmKgJ4t1rZkI0i8koUuIV99zPx_j4TkA5bUANNpvJ9jbUyZSGvelMWifpVg4mzZJ0Z8NPLw_7dOl6F2Qwt67rbPglt-YhqsGPZm76ZF3xe95sTsK6-a2tjlsTq-ukAQiWnLQY4QZcBjCkWGCm2XsAgYyzqm1J8m353-9n3NbLXu0j0w_QD-YGwxfjHdT2yQ7W9eeOe6Pu3Rg-xJ6fXkChKNHKfIUZhaTAtJXoklknbf-mdsbvzBX_V0MOt6osqiXdJMjdIj2hHOcX0n5t_zZLiFcyJHB4HKU30DsXrl76XfL8yiUURhWikp-3YrAqDBdcrQDIQMwp5qbjfE-L1oDjbDdg4bX09ws6R2486DjUP7QNzoN6GecMyDG4P8-9WusTc2flzstuFQ-y_-tcLFv5MJxqUE_gH6gJL3NKEGVClJxRRmgGnqMsypwjUiCMCI1Pw-YMvBwdUC4oF0hUFBcoNgiZ_wPXKhKQ?type=png){alt="Git Rebase to bring the diverged branch up-to-date with `main`."}

`git rebase` takes a different approach to bringing branches up-to-date.

Lets go through this process by...

1. Make a new repository.
2. Create `branch1`, add a `README.md` and commit it.
3. Switch back to `main`.
4. Create `branch2`, add a `LICENSE` and commit it.
5. Merge `branch1` into `main` (equivalent to making a Pull Request).
6. Rebase `branch2` onto `main` so it includes the `README.md` and the point of divergence is updated.
7. Merge `branch2` into `main`.

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Again remember to take the time to show the contents of the files and how they "disappear" when switching branches, in
particular after having added `README.md` to `branch1` and switching back to `main`.

Also use `glod` alias (or other form of `git log` that shows branches) to show the changes and explain the point at
which each of the branches is at with reference to the `*` which denote commits, the branch names and where they sit and
the date/time stamps.

It can be useful at the end to open a second terminal and show the history of the two `git-merge-test`  and
`git-rebase-test` repositories to show how they differ in terms of branches.

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

#### 1. Make a new repository

``` bash
cd ~/work/git/hub/ns-rse
mkdir git-rebase-test
cd git-rebase-test
git init --initial-branch=main
git commit --allow-empty -m "Initial commit"
```

#### 2. Create `branch1`, add a `README.md` and commit it

``` bash
git switch -c branch1
echo "# Just a test" > README.md
git add README.md
git commit -m "docs: Adding README.md"
```

#### 3. Switch back to `main`

As before `README.md` does not exist on the `main` branch.

``` bash
git switch main
cat README.md   # Note that `README.md` does not currently exist on this branch
```

#### 4. Create `branch2`, add a `LICENSE` and commit it

``` bash
git switch -c branch2
echo "YOU CAN DO WHAT YOU WANT WITH THIS CODE" > LICENSE
git add LICENSE
git commit -m "docs: Adding a LICENSE"
```

#### 5. Merge `branch1` into `main` (equivalent to making a Pull Request)

Switch back to `main` and merge `branch1` (this is equivalent to merging a Pull Request). The file `README.md` now
exists on the `main` branch.

``` bash
git switch main
git merge branch1  # Merge branch1 into main, equivalent to a Pull Request
cat README.md
```

#### 6. Rebase `branch2` onto `main` so it includes the `README.md` and the point of divergence is updated

Switch to `branch2` which has now diverged as it contains changes of its own _and_ `main` contains the changes made on
`branch1`.  We want to rebase `branch2` onto `main` so that it appears as if `branch2` forked _after_ the changes from
`branch1` were merged.

``` bash
git switch branch2
git rebase main # Rebase branch2 onto main
glod

* 12f5202 - (HEAD -> branch2) Adding a LICENSE (2024-03-01 12:19:12 +0000) <Neil Shephard>
* 4e8e933 - (main, branch1) Adding README.md (2024-03-01 12:18:37 +0000) <Neil Shephard>
* 2459609 - Initial commit (2024-03-01 12:18:37 +0000) <Neil Shephard>

```

#### 7. Merge `branch2` into `main`

We now have the changes from `branch1` included in `branch2` by virtue of having rebased onto `main` _after_ the changes
in `branch1` were merged in. If we switch back to `main` we can merge the changes from `branch2`.

``` bash
git switch main
git merge branch2
glod

* 12f5202 - (HEAD -> main, branch2) docs: Adding a LICENSE (2024-03-01 12:19:12 +0000) <Neil Shephard>
* 4e8e933 - (branch1) docs: Adding README.md (2024-03-01 12:18:37 +0000) <Neil Shephard>
* 2459609 - Initial commit (2024-03-01 12:18:37 +0000) <Neil Shephard>
```

#### 8. Delete `branch1` and `branch2`

As we're done with `branch1` and `branch2` we can delete them.

``` bash
git branch -d branch{1,2}
glod

* 12f5202 - (HEAD -> main) Adding a LICENSE (2024-03-01 12:19:12 +0000) <Neil Shephard>
* 4e8e933 - Adding README.md (2024-03-01 12:18:37 +0000) <Neil Shephard>
* 2459609 - Initial commit (2024-03-01 12:18:37 +0000) <Neil Shephard>
```

As you can see the history of the `main` branch is now linear.

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 7: Diverging Branches

In your pairs bring the `square-root` branch up-to-date and incorporate the changes that have been merged into
`main` from the `zero-division` branch and then create a Pull Request to merge the updated `square-root` changes into
`main` on GitHub, review it and merge it.

The person who has been working on the `square-root` issue/branch will be at the helm for this, but work together to
come up with a solution. You can use either of the two strategies `git merge` or `git rebase` to do this.

:::::::::::::::::::::::: solution

## Solution : `git merge`

The first thing to do is make sure `main` is up-to-date and has the changes that have been merged from the
`zero-division` branch locally.

``` bash
cd ~/work/git/hub/ns-rse/python-maths
git switch main
git pull
```

Then you can switch branches to the `square-root` branch and merge the main branch in.

``` bash
git switch ns-rse/square-root
git merge main
```

You can now push the changes that are on the `square-root` branch to GitHub and make a Pull Request for approval

``` bash
git push --set-upstream origin ns-rse/square-root
```

:::::::::::::::::::::::::::::::::
:::::::::::::::::::::::: solution

## Solution : `git rebase`

The first thing to do is make sure `main` is up-to-date and has the changes that have been merged from the
`zero-division` branch locally.

``` bash
cd ~/work/git/hub/ns-rse/python-maths
git switch main
git pull
```

Then you can switch branches to the `square-root` branch and merge and rebase onto main.

``` bash
git switch ns-rse/square-root
git rebase main
```

You can now push the changes that are on the `square-root` branch to GitHub and make a Pull Request for approval

``` bash
git push --set-upstream origin ns-rse/square-root
```

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

### Oh no I've got a `merge conflict`

Both the `git merge` and `git rebase` strategies in the worked examples and the `python-maths` repositories were fairly
painless because none of the changes that were made touched the same files. In real-life things are often likely to be a
bit more messy and when you want to update your diverged branch you will often find that files you have been working on
have been modified and merged into `main` by others. This results in a "merge conflict" where Git can not determine
which lines are required and therefore requires manual intervention.

If you have undertaken the [Git & GitHub Through GitKraken - From Zero to Hero!][zerohero] course you will have
encountered merge conflicts when working through the "_Python Calculator_" exercise and have some idea of how to resolve
them. We will however now go through resolving the issue when updating diverged branches.

#### 1. Create a new repository

``` bash
cd ~/work/git/hub/ns-rse
mkdir git-rebase-test-conflict
cd git-rebase-test-conflict
git init --initial-branch=main
git commit --allow-empty -m "Initial commit"
```

#### 2. Create `branch1` and add a `README.md`

Again we add a `README.md` but this time we make two commits to it, adding an extra line.

``` bash
git switch -c branch1
echo "# Just a test\n\n" > README.md
git add README.md
git commit -m "docs: Adding README.md"
echo "Lets add another line in a separate commit" >> README.md
git add README.md
git commit -m "docs: Ooops, missed a line from the README.md"
```

#### 3. Switch back to `main`

Again `README.md` doesn't exist on this branch yet.

``` bash
git switch main
cat README.md
```

#### 4. Create `branch2` and add a `README.md`

We now set ourselves up for a conflict by creating a `README.md` on `branch2`, knowing full well that such a file
already exists on `branch1`. We put different text into it.

``` bash
git switch -c branch2
echo "# Just a test\n\nBut we're creating a merge conflict\n" > README.md
git add README.md
git commit -m "This repo needs a README.md"
cat README.md
```

#### 5. Merge `branch1` into `main`

Merge `branch1` into `main`. The `README.md` has the text from `branch1`. As we are done with this branch we can delete
it now.

``` bash
# Switch to main and merge branch1
git switch main
git merge branch1
cat README.md
git branch -d branch1
```

#### 6. Switch to `branch2` and add another line to `README.md`

Switch back to `branch2` and add another line to `README.md`, stage and commit it. The history now shows that we have
two commits on this branch after the "Initial commit".

``` bash
# Switch to branch2 add more to `README.md` and rebase
git switch branch2
echo "Lets add another commit to make things messier" >> README.md
git add README.md
git commit -m "Bulking out README.md with more information"
glod

* bce21bd - (HEAD -> branch2) Bulking out README.md with more information (2024-03-01 13:26:01 +0000) <Neil Shephard>
* 29b2e32 - This repo needs a README.md (2024-03-01 13:23:16 +0000) <Neil Shephard>
* 57e68aa - Initial commit (2024-03-01 13:20:14 +0000) <Neil Shephard>

```

#### 6. Rebase `branch2` onto `main`

We now want to update `branch2` by rebasing onto `main` so that we have the new changes from `main` (i.e. those merged
from `branch1`). In this instance though we _know_ both `branch1` and `branch2` have modified the file `README.md` and
so we expect to get a conflict and sure enough we do.

``` bash
git rebase main

Auto-merging README.md
CONFLICT (add/add): Merge conflict in README.md
error: could not apply fcfe2db... This repo needs a README.md
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
Recorded preimage for 'README.md'
Could not apply fcfe2db... This repo needs a README.md
```

Oh dear we have, as expected, encountered the dreaded "merge conflict" as both `branch1` and `branch2` made changes to
`README.md`. Lets take a look at what the file now looks like.

``` bash
cat README.md
<<<<<<< HEAD
# Just a test

Lets add another line in a separate commit
=======
# Just a test

But we're creating a merge conlict

>>>>>>> 29b2e32 (This repo needs a README.md)
```

Here `HEAD` refers to the branch that is being merged in (`main`) which contains the changes we made on `branch1` and
merged into `main`. The text that this refers to  is delimited by `<<<<<<<` and `=======` and is
`# Just a test` and `Lets add another line in a separate commit`. The commit (`fcfe2db`) on `branch2` which added _two_
lines (although technically its 4 since we also included blank lines) then follows and is delimited by `=======` and
`>>>>>>>` and includes the message.

We are given some useful information as to what we could do and there are three options.

1. `Resolve all conflicts manually, mark them as resolved with "git add/rm <conflicted files>", then run "git rebase
  --continue".`
2. `You can instead skip this commit: run "git rebase --skip".`
3. `To abort and get back to the state before "git rebase", run "git rebase --abort".`

These are really useful messages telling us how we can proceed. In this instance we want to take option 1, so we should
open the `README.md` and edit it to leave it in the state we want the file to be in.

#### 7. Resolve the conflict

You can use the `nano` editor to open the file with `nano README.md`. Edit it to look like this

``` bash
# Just a test

Lets add another line in a separate commit

But we're creating a merge conflict
```

You can use `Ctrl+k` to remove a whole line at once. Save the file and return to the command prompt (in `nano` this is
`Ctrl+O` then `Ctrl+X`).

::::::::::::::::::::::::::::::::::::: callout

[`nano`][nano] is a simple text editor found on most GNU/Linux and OSX systems that is quick and easy to use. A useful
bookmark to help whilst developing the muscle memory for the commands is the [nano shortcuts
cheatsheet](https://www.nano-editor.org/dist/latest/cheatsheet.html).

It is possible that your system may use a different editor than `nano` by default, e.g. `vim`. It does not matter which
text editor you use to edit and save the files and if you are comfortable using this then that is not a problem.

::::::::::::::::::::::::::::::::::::::::::::::::

#### 8. Add the conflicted file and continue with rebase

You can now continue with the advice and add the conflicted files back to Git and continue with the rebase.

``` bash
git add README.md
git rebase --continue

Recorded resolution for 'README.md'.
[detached HEAD d041adb] This repo needs a README.md
 1 file changed, 4 insertions(+)
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
error: could not apply 84a1592... Bulking out README.md with more information
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
error: could not parse conflict hunks in 'README.md'
Could not apply 84a1592... Bulking out README.md with more information
```

Hang on, we just resolved the merge conflict why are we being told there is another? Well the _first_ conflict with
commit `fcfe2db` was resolved and we are told as much in the line `Recorded resolution for 'README.md'`, however there
is now a conflict between that and commit `84a1592`. We get the same advice so lets take a look at the state of
`README.md`

``` bash
# Just a test

Lets add another line in a separate commit

But we're creating a merge conflict

<<<<<<< HEAD
>>>>>>> fcfe2db (This repo needs a README.md)
=======
Lets add another commit to make things messier
>>>>>>> 84a1592 (Bulking out README.md with more information)
```

Here we can see its the second line that we added to `README.md` under `branch2` that read
`Lets add another commit to make things messier` that is causing the problem. Its not in the `main` branch on which we
are rebasing so Git doesn't know whether it should be and we have to manually resolve this. Edit the file so that it
looks like the following.

``` bash
# Just a test

Lets add another line in a separate commit

But we're creating a merge conflict

Lets add another commit to make things messier
```

#### 9. Add the conflicted file and continue with the second stage of the rebase

Then add the conflicted file and continue with the rebase.

``` bash
git add README.md
git rebase --continue
[detached HEAD 0ccfe91] Bulking out README.md with more information
 1 file changed, 1 insertion(+), 1 deletion(-)
Successfully rebased and updated refs/heads/branch2.
```

We are told that the rebase has been successful and `branch2` now contains all commits from `main` (which includes those
merged from `branch1`). If we look at the contents of `README.md` it contains all of the lines we added to both branches
as that is how we chose to resolve the conflicts manually.

``` bash
cat README.md
# Just a test


Lets add another line in a separate commit

But we're creating a merge conflict

Lets add another commit to make things messier
```

The history/graph is linear now and shows that `branch2` is two commits ahead of `main`.

``` bash
glod
* 0ccfe91 - (HEAD -> branch2) Bulking out README.md with more information (2024-03-01 14:00:57 +0000) <Neil Shephard>
* d041adb - This repo needs a README.md (2024-03-01 13:59:31 +0000) <Neil Shephard>
* 64905e8 - (main) Ooops, missed a line from the README.md (2024-03-01 13:56:35 +0000) <Neil Shephard>
* e68485d - Adding README.md (2024-03-01 13:55:50 +0000) <Neil Shephard>
* dec5385 - Initial commit (2024-03-01 13:55:50 +0000) <Neil Shephard>
```

::::::::::::::::::::::::::::::::::::: callout

You may be wondering why when performing `git rebase` it mentions `git merge`. This is because a rebase will
sequentially merge all commits from the branch you are rebasing onto, in this case `main`, into the `HEAD` of your
current checked out branch (`branch2`).

::::::::::::::::::::::::::::::::::::::::::::::::

### Repeating yourself

You had to resolve two merge conflicts here, if the history you are merging has a lot of commits you may end up solving
the same merge conflict repeatedly. There is a way to avoid this though.

::::::::::::::::::::::::::::::::::::: callout

Bringing a diverged branch up-to-date can get _very_ messy and confusing if there is a large amount of divergence. The
best strategy to avoid this complication is two fold.

1. Break work down into small chunks and regularly merge them into `main`.
2. If this can not be avoided or lots of others are making changes you should `git merge` or `git rebase` your feature
   branch onto `main`  frequently.

::::::::::::::::::::::::::::::::::::::::::::::::

You may encounter this situation and find that you are repeatedly resolving the same conflict as you want the finer
grained control over `git rebase` and one option is to `git rebase --abort` and use `git merge` instead as you only have
to resolve the conflicts once, although there may be a lot of them. One disadvantage of this is it makes it look like
the commits stem from you and so many people prefer the rebase strategy.

Help is at hand though if you find you are repeatedly being asked to resolve the same conflict as you progress through a
rebase in the form of [rerere][rerere] which stands for "**re**use **re**corded **re**solution" and causes Git to
remember how it has resolved merge conflicts at a given point and the next time it is encountered it will use the
solution from the first instance.

You can enable this in your global configuration, which is covered in greater detail in the next episode, with the
following.

``` bash
git config --global rerere.enabled true
```

If you only wish to use this strategy on some repositories you can apply it to your local configuration from within the
working directory.

``` bash
git config --local rerere.enabled true
```

You can of course enable globally and disable locally as local configuration variables take precedence over global.

::::::::::::::::::::::::::::::::::::: callout

### Not Breaking Things

As you rebase your branch you can make sure that you don't break any of your code by running tests at each step. This is
achieved using the `-x` switch which will execute the command that follows.

``` bash
git rebase -x "pytest" <reference>
```

::::::::::::::::::::::::::::::::::::::::::::::::

## Switching Branches during Work in Progress

Sometimes you will be doing some work and a colleague will ask you to review a pull request or help them with a problem
they have on their branch. When performing pull request reviews it can be quite common to run tests to check everything
passes if you don't have Continuous Integration doing this automatically for you (we will come to that in another
episode).

But there is a challenge, in order to switch branches you have to stage and commit all changes to tracked files.

``` bash
git switch branch2
echo "Please feel free to contribute to this repository" >> CONTRIBUTING.md
git add CONTRIBUTING.md
git commit -m "Adding CONTRIBUTING.md"
echo "\nPlease don't break my repository though!" >> CONTRIBUTING.md
git switch main

error: Your local changes to the following files would be overwritten by checkout:
 CONTRIBUTING.md
Please commit your changes or stash them before you switch branches.
Aborting
```

Whilst you could commit your changes and subsequently `git commit --amend` (more on this in the next episode) there is
another option.

### `git stash`

`git stash` allows you to save your current changes in a temporary location and then reverts to the last commit (`HEAD`)
and allows you to move about to other branches and undertake work. There are lots of options to `git stash` but the
basics are pretty straight-forward. You start by `git stash push` (the `push` is actually optional) and you can include
a `--message` that explains what the stash contains, you are told if this has worked and on what branch the stash was
made and can then switch branches, pull down changes, create a new branch and do something different.

#### 1. Stash `CONTRIBUTING.md`

``` bash
git stash --message "CONTRIBUTING.md WIP"
Saved working directory and index state On branch2: CONTRIBUTION.md WIP
```

#### 2. Switch to `main` and create `branch3`

``` bash
git switch main
# If this weren't a dummy example you might git pull
git switch -c branch3
```

Undertake the work that is required on `branch3`.

#### 3. Return to `branch2`

When you have finished this other work you can return to `branch2` and `pop` the stash back. To see what stashes there
are you can use `git stash list`

``` bash
git stash list
stash@{0}: On branch2: CONTRIBUTION.md WIP
```

#### 4. `pop` the last stash

When you are ready to restore the work you can do so using `git stash pop` which by default will restore the _last_
stash.

``` bash
git stash pop
On branch branch2
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
 modified:   CONTRIBUTING.md

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (13c8c6fb23f9fcdd884b4528356db37527c9b3e4)
```

The changes to `CONTRIBUTING.md` and the corresponding entry are removed from the stash list.

### Multiple Stashes

Over time though you may collect multiple stashes.

#### 1. Make two stashes

We stash `CONTRIBUTING.md`, the last message is reused by default, then we add `ANOTHER.md` and stash it with a
different message.

``` bash
git stash --message "CONTRIBUTING.md WIP"
echo "Yet another file" > ANOTHER.md
git add ANOTHER.md
git stash --message "Stashing ANOTHER.md file"

stash@{0}: On branch2: Stashing ANOTHER.md file
stash@{1}: WIP on branch2: a8b6f5f Adding CONTRIBUTING.md
```

#### 2. Pop the `CONTRIBUTING.md` stash

There are now two stashes each with different names.

You may not want to restore the work stashed with the commit message `Stashing ANOTHER.md file` but rather restore the
earlier `Adding CONTRIBUTING.md` work first. You can do this by referring to the number associated with the stash that
is within the curly braces. For the `Adding CONTRIBUTING.md` this is `1`.

``` bash
git stash pop 1

On branch branch2
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
 modified:   CONTRIBUTING.md

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{1} (dd538beb8f14590f720e9b9f677ba7381240bd92)
```

Only the `CONTRIBUTING.md` file has been restored and not the `ANOTHER.md`.

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 8: Stashing

1. Clone the [`pytest-examples`](https://github.com/ns-rse/pytest-examples) repository.
2. Create a `contributing` branch.
3. Create a `CONTRIBUTING.md` with `echo "# Contributing\n\nContributions to this repository are welcome via Pull
   Requests." > CONTRIBUTING.md`.
4. Do _not_ add and commit, instead `git stash` your changes.
5. Create a `citation` branch.
6. Add a basic `CITATION.cff` with `echo "cff-version: 1.2.0\ntitle: Pytest Examples\ntype: software" > CITATION.cff`.
7. Add and commit this file.
8. Switch back to the `contributing` branch.
9. Unstash the `CONTRIBUTING.md` file.

:::::::::::::::::::::::: solution

## Solution : stashing

Lets create the `contributing` branch

```bash
git switch -c contributing
echo "# Contributing\n\nContributions to this repository are welcome via Pull Requests." > CONTRIBUTING.md
```

If we want to switch branches without making a commit but save our work in progress as we want to add more to the
`CONTRIBUTING.md` file later we can stash the changes with a message. We then switch to `main` and create a new branch
(`citation`) for and add a `CITATION.cff` file.

```bash
git stash -m "An example stash"
git switch main
git switch -c citation
echo "cff-version: 1.2.0\ntitle: Pytest Examples\ntype: software" > CITATION.cff
git add CITATION.cff
git commit -m "chore: Adding a CITATION.cff"
```

When we are ready to return to our `contributing` branch we can switch and `git pop` the work we stashed. By default the
last stash is popped, but its possible to view all the stashes and select which you wish to pop and restore to the
current branch.

```bash
git switch contributing
git pop
```

This works but its quite a few steps to make stashes and "pop" them once we've finished on the other branch.

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: callout

## Popping around and applying

- You can `git stash pop` work that has been stashed on one branch onto another, it applies to whichever branch you are
  currently checked out on.
- Use `git stash apply` to `pop` a stash but leave it in the stash list.

There are a lot of useful things `git stash` can be used for. Refer to the help pages (`git stash --help`) for more
information as well as the Further Resources.
::::::::::::::::::::::::::::::::::::::::::::::::

## Worktrees instead of Branches

Sometimes you will want to switch between branches that are all in development in the middle of work. If you've made
changes to files that you have not saved and committed Git will tell you that the changes made to your files
will be over-written if they differ from those on the branch you are switching to and it will refuse to switch branches.

This means either making a commit or as we've just seen stashing the work to come back to at a later date. Neither of
these are particularly problematic as you can `git pop` stashed work to restore it or `git commit --amend`, or `git
commit --fixup` and squash commits to maintain small atomic commits and avoid cluttering up the commit history with
commits such as "_Saving work to review another branch_" (more on this in the next episode!). But, perhaps
unsurprisingly, Git has another way of helping your workflow in this situation. Rather than having branches you can use
"_worktrees_".

Normally when you've `git clone`'d a repository all configuration files for working with the repository are saved to the
repository directory under `.git` _and_ all files in their current state on the `main` branch are also copied to the
repository directory. If we clone the [pytest-examples](https://github.com/ns-rse/pytest-examples) directory
we can look at its contents using `tree -afHD -L 2` (this limits the depth as we don't need to look deep inside the
`.git` or `mypy` directories which contain lots of files).

```bash
git clone git@github.com:ns-rse/pytest-examples.git
cd pytest-examples
tree -afhD -L 2
[4.0K Mar 11 07:26]  .
├── [ 52K Jan  5 11:26]  ./.coverage
├── [4.0K Mar 11 07:26]  ./.git
│   ├── [ 749 Jan  5 11:30]  ./.git/COMMIT_EDITMSG
│   ├── [ 394 Jan  5 11:28]  ./.git/COMMIT_EDITMSG~
│   ├── [ 479 Feb 17 14:08]  ./.git/config
│   ├── [ 556 Feb 17 14:06]  ./.git/config~
│   ├── [  73 Jan  1 13:24]  ./.git/description
│   ├── [ 222 Mar 11 07:26]  ./.git/FETCH_HEAD
│   ├── [  21 Mar 11 07:26]  ./.git/HEAD
│   ├── [4.0K Jan  1 13:27]  ./.git/hooks
│   ├── [1.3K Mar 11 07:26]  ./.git/index
│   ├── [4.0K Jan  1 13:24]  ./.git/info
│   ├── [4.0K Jan  1 13:24]  ./.git/logs
│   ├── [4.0K Mar 11 07:26]  ./.git/objects
│   ├── [  41 Mar 11 07:26]  ./.git/ORIG_HEAD
│   ├── [ 112 Jan  3 15:57]  ./.git/packed-refs
│   ├── [4.0K Jan  1 13:24]  ./.git/refs
│   └── [4.0K Jan  1 13:31]  ./.git/rr-cache
├── [4.0K Jan  2 11:52]  ./.github
│   └── [4.0K Jan  3 15:57]  ./.github/workflows
├── [3.0K Jan  2 12:06]  ./.gitignore
├── [1.0K Jan  1 13:24]  ./LICENSE
├── [ 293 Jan  2 12:06]  ./.markdownlint-cli2.yaml
├── [4.0K Jan  5 11:27]  ./.mypy_cache
│   ├── [ 12K Jan  5 11:28]  ./.mypy_cache/3.11
│   ├── [ 190 Jan  2 10:39]  ./.mypy_cache/CACHEDIR.TAG
│   └── [  34 Jan  2 10:39]  ./.mypy_cache/.gitignore
├── [1.7K Mar 11 07:26]  ./.pre-commit-config.yaml
├── [ 763 Jan  1 13:25]  ./.pre-commit-config.yaml~
├── [ 18K Jan  2 12:06]  ./.pylintrc
├── [4.8K Mar 11 07:26]  ./pyproject.toml
├── [4.7K Jan  1 17:36]  ./pyproject.toml~
├── [4.0K Jan  1 19:04]  ./.pytest_cache
│   ├── [ 191 Jan  1 19:04]  ./.pytest_cache/CACHEDIR.TAG
│   ├── [  37 Jan  1 19:04]  ./.pytest_cache/.gitignore
│   ├── [ 302 Jan  1 19:04]  ./.pytest_cache/README.md
│   └── [4.0K Jan  1 19:04]  ./.pytest_cache/v
├── [4.0K Mar 11 07:26]  ./pytest_examples
│   ├── [1.3K Mar 11 07:26]  ./pytest_examples/divide.py
│   ├── [ 179 Mar 11 07:26]  ./pytest_examples/__init__.py
│   ├── [4.0K Jan  5 11:18]  ./pytest_examples/__pycache__
│   ├── [ 491 Mar 11 07:26]  ./pytest_examples/shapes.py
│   └── [ 390 Jan  2 13:34]  ./pytest_examples/shapes.py~
├── [4.0K Jan  2 16:09]  ./pytest_examples.egg-info
│   ├── [   1 Jan  2 16:09]  ./pytest_examples.egg-info/dependency_links.txt
│   ├── [3.1K Jan  2 16:09]  ./pytest_examples.egg-info/PKG-INFO
│   ├── [ 481 Jan  2 16:09]  ./pytest_examples.egg-info/requires.txt
│   ├── [ 446 Jan  2 16:09]  ./pytest_examples.egg-info/SOURCES.txt
│   └── [  16 Jan  2 16:09]  ./pytest_examples.egg-info/top_level.txt
├── [ 602 Jan  3 15:57]  ./README.md
├── [   0 Jan  1 13:31]  ./README.md~
├── [4.0K Jan  1 13:30]  ./.ruff_cache
│   ├── [4.0K Jan  2 11:57]  ./.ruff_cache/0.1.8
│   ├── [  43 Jan  1 13:30]  ./.ruff_cache/CACHEDIR.TAG
│   └── [   1 Jan  1 13:30]  ./.ruff_cache/.gitignore
├── [4.0K Mar 11 07:26]  ./tests
│   ├── [ 681 Mar 11 07:26]  ./tests/conftest.py
│   ├── [  26 Jan  2 12:11]  ./tests/conftest.py~
│   ├── [4.0K Jan  5 11:26]  ./tests/__pycache__
│   ├── [1.7K Mar 11 07:26]  ./tests/test_divide.py
│   ├── [1.6K Mar 11 07:26]  ./tests/test_shapes.py
│   └── [   0 Jan  2 13:36]  ./tests/test_shapes.py~
└── [ 460 Jan  2 16:09]  ./_version.py

21 directories, 43 files
```

### The Worktree

Worktrees take a different approach to organising branches. They start with a `--bare` clone of the repository which
implies the `--no-checkout` flag and means that the files that would normally be found under the `<repository>/.git`
directory are copied but are instead placed in the top level of the directory rather than under `.git/`. No tracked
files are copied as they may conflict with these files. You have all the information Git has about the history of the
repository and the different commits and branches but none of the _actual_ files.

**NB** If you don't explicitly state a target directory to clone to it will be the repository name suffixed with `.git`,
i.e. in this example `pytest-examples.git`. I recommend sticking with the convention of using the same repository name
so will explicitly state it.

```bash
cd ..
mv pytest-examples pytest-examples-orig-clone
git clone --bare git@github.com:ns-rse/pytest-examples.git pytest-examples
cd pytest-examples
tree -afhD -L 2
[4.0K Mar 13 07:45]  .
├── [ 129 Mar 13 07:45]  ./config
├── [  73 Mar 13 07:45]  ./description
├── [  21 Mar 13 07:45]  ./HEAD
├── [4.0K Mar 13 07:45]  ./hooks
│   ├── [ 478 Mar 13 07:45]  ./hooks/applypatch-msg.sample
│   ├── [ 896 Mar 13 07:45]  ./hooks/commit-msg.sample
│   ├── [4.6K Mar 13 07:45]  ./hooks/fsmonitor-watchman.sample
│   ├── [ 189 Mar 13 07:45]  ./hooks/post-update.sample
│   ├── [ 424 Mar 13 07:45]  ./hooks/pre-applypatch.sample
│   ├── [1.6K Mar 13 07:45]  ./hooks/pre-commit.sample
│   ├── [ 416 Mar 13 07:45]  ./hooks/pre-merge-commit.sample
│   ├── [1.5K Mar 13 07:45]  ./hooks/prepare-commit-msg.sample
│   ├── [1.3K Mar 13 07:45]  ./hooks/pre-push.sample
│   ├── [4.8K Mar 13 07:45]  ./hooks/pre-rebase.sample
│   ├── [ 544 Mar 13 07:45]  ./hooks/pre-receive.sample
│   ├── [2.7K Mar 13 07:45]  ./hooks/push-to-checkout.sample
│   ├── [2.3K Mar 13 07:45]  ./hooks/sendemail-validate.sample
│   └── [3.6K Mar 13 07:45]  ./hooks/update.sample
├── [4.0K Mar 13 07:45]  ./info
│   └── [ 240 Mar 13 07:45]  ./info/exclude
├── [4.0K Mar 13 07:45]  ./objects
│   ├── [4.0K Mar 13 07:45]  ./objects/info
│   └── [4.0K Mar 13 07:45]  ./objects/pack
├── [ 249 Mar 13 07:45]  ./packed-refs
└── [4.0K Mar 13 07:45]  ./refs
    ├── [4.0K Mar 13 07:45]  ./refs/heads
    └── [4.0K Mar 13 07:45]  ./refs/tags

9 directories, 19 files
```

What use is that? Well from this point you can instead of using `git branch` use `git worktree add <branch_name>` and it
will create a _directory_ with the name of the branch which holds all the files in their current state on that branch.

```bash
git worktree add main
Preparing worktree (checking out 'main')
HEAD is now at 2f7c382 Merge pull request #6 from ns-rse/ns-rse/tidy-print
tree -afhD -L 2 main/
[4.0K Mar 13 08:13]  main
├── [  64 Mar 13 08:13]  main/.git
├── [4.0K Mar 13 08:13]  main/.github
│   └── [4.0K Mar 13 08:13]  main/.github/workflows
├── [3.0K Mar 13 08:13]  main/.gitignore
├── [1.0K Mar 13 08:13]  main/LICENSE
├── [ 293 Mar 13 08:13]  main/.markdownlint-cli2.yaml
├── [1.7K Mar 13 08:13]  main/.pre-commit-config.yaml
├── [ 18K Mar 13 08:13]  main/.pylintrc
├── [4.8K Mar 13 08:13]  main/pyproject.toml
├── [4.0K Mar 13 08:13]  main/pytest_examples
│   ├── [1.3K Mar 13 08:13]  main/pytest_examples/divide.py
│   ├── [ 179 Mar 13 08:13]  main/pytest_examples/__init__.py
│   └── [ 491 Mar 13 08:13]  main/pytest_examples/shapes.py
├── [ 602 Mar 13 08:13]  main/README.md
└── [4.0K Mar 13 08:13]  main/tests
    ├── [ 681 Mar 13 08:13]  main/tests/conftest.py
    ├── [1.7K Mar 13 08:13]  main/tests/test_divide.py
    └── [1.6K Mar 13 08:13]  main/tests/test_shapes.py

5 directories, 14 files
```

Each branch can have a worktree added for it and then when you want to switch between them its is simply a case of
`cd`ing into the worktree (/branch) you wish to work on. You use Git commands within the worktree directory to apply
them to that branch and Git keeps track of everything in the usual manner.

Lets create two worktree's, the `contributing` and `citation` we created above when working with branches. If you didn't

```bash
cd ../
mv pytest-examples pytest-examples-orig-clone
git clone --bare git@github.com:ns-rse/pytest-examples.git pytest-examples
cd pytest-examples
git worktree add contributing
git worktree add citation
```

You are now free to move between worktrees (/branches) and undertake work on each without having to `git stash` or `git
commit` work in progress. We can add the `CONTRIBUTING.md` to the `contributing` worktree then jump to the `citation`
worktree and add the `CITATION.cff`

```bash
cd contributing
echo "# Contributing\n\nContributions to this repository are welcome via Pull Requests." > CONTRIBUTING.md
cd ../citation
echo "cff-version: 1.2.0\ntitle: Pytest Examples\ntype: software" > CITATION.cff
```

Neither branches have had the changes committed so Git will not show any differences between them, but we can use `diff
-qr` to compare the directories.

```bash
diff -qr contributing citation
Only in citation: CITATION.cff
Only in contributing: CONTRIBUTING.md
Files contributing/.git and citation/.git differ
```

If we commit the changes to each we can `git diff` them.

```bash
cd contributing
git add CONTRIBUTING.md
git commit -m "Adding basic CONTRIBUTING.md"
cd ../citation
git add CITATION.cff
git commit -m "Adding basic CITATION.cff"
git diff citation contributing
CITATION.cff --- Text
1 cff-version: 1.2.0
2 title: Pytest Examples
3 type: software

CONTRIBUTING.md --- Text
1 # Contributing
2
3 Contributions to this repository are welcome via Pull Requests
```

**NB** The output of `git diff` may depend on the difftool that you have configured, I use and recommend the brilliant
[`difftastic`](https://difftastic.wilfred.me.uk/) which has easy [integration with
Git](https://difftastic.wilfred.me.uk/git.html).

### Listing Worktrees

Just as you can `git branch --list` you can `git worktree list`

```bash
git worktree list
/mnt/work/git/hub/ns-rse/pytest-examples               (bare)
/mnt/work/git/hub/ns-rse/pytest-examples/citation      19ff076 [citation]
/mnt/work/git/hub/ns-rse/pytest-examples/contributing  ad56b91 [contributing]
/mnt/work/git/hub/ns-rse/pytest-examples/main          2f7c382 [main]
```

### Moving Worktrees

You can move worktrees to different directories, these do _not_ even have to be within the bare repository that you
cloned as Git keeps track of these in the `worktrees/` directory which has a folder for each of the worktrees you create
and the file `gitdir` points to the location of that particular worktree.

```bash
cd pytest-examples   # Move to the bare repository
tree -afhD -L 2 worktrees
[4.0K Mar 13 09:27]  worktrees
├── [4.0K Mar 13 09:31]  worktrees/citation
│   ├── [  26 Mar 13 09:31]  worktrees/citation/COMMIT_EDITMSG
│   ├── [   6 Mar 13 09:27]  worktrees/citation/commondir
│   ├── [  55 Mar 13 09:27]  worktrees/citation/gitdir
│   ├── [  25 Mar 13 09:27]  worktrees/citation/HEAD
│   ├── [1.4K Mar 13 09:31]  worktrees/citation/index
│   ├── [4.0K Mar 13 09:27]  worktrees/citation/logs
│   ├── [   0 Mar 13 09:31]  worktrees/citation/MERGE_RR
│   ├── [  41 Mar 13 09:27]  worktrees/citation/ORIG_HEAD
│   └── [4.0K Mar 13 09:27]  worktrees/citation/refs
├── [4.0K Mar 13 09:30]  worktrees/contributing
│   ├── [  29 Mar 13 09:30]  worktrees/contributing/COMMIT_EDITMSG
│   ├── [   6 Mar 13 09:27]  worktrees/contributing/commondir
│   ├── [  59 Mar 13 09:27]  worktrees/contributing/gitdir
│   ├── [  29 Mar 13 09:27]  worktrees/contributing/HEAD
│   ├── [1.4K Mar 13 09:30]  worktrees/contributing/index
│   ├── [4.0K Mar 13 09:27]  worktrees/contributing/logs
│   ├── [   0 Mar 13 09:30]  worktrees/contributing/MERGE_RR
│   ├── [  41 Mar 13 09:27]  worktrees/contributing/ORIG_HEAD
│   └── [4.0K Mar 13 09:27]  worktrees/contributing/refs
└── [4.0K Mar 13 08:13]  worktrees/main
    ├── [   6 Mar 13 08:13]  worktrees/main/commondir
    ├── [  51 Mar 13 08:13]  worktrees/main/gitdir
    ├── [  21 Mar 13 08:13]  worktrees/main/HEAD
    ├── [1.3K Mar 13 08:13]  worktrees/main/index
    ├── [4.0K Mar 13 08:13]  worktrees/main/logs
    ├── [  41 Mar 13 08:13]  worktrees/main/ORIG_HEAD
    └── [4.0K Mar 13 08:13]  worktrees/main/refs

10 directories, 19 files
```

If we look at the `gitdir` file in each `worktree` sub-directory we see where they point to.

```bash
cat worktrees/*/gitdir
/mnt/work/git/hub/ns-rse/pytest-examples/citation/.git
/mnt/work/git/hub/ns-rse/pytest-examples/contributing/.git
/mnt/work/git/hub/ns-rse/pytest-examples/main/.git
```

These mirror the locations reported by `git worktree list`, albeit with `.git` appended.

If you want to move a worktree you can do so, here we move `citation` to `~/tmp`.

```bash
git worktree move citation ~/tmp
```

### Removing worktrees

It's simple to remove a worktree after the changes have been merged or it is no longer needed, make sure to "prune" the
tree after having done so.

```bash
git worktree remove citation
git worktree prune
git worktree list
/mnt/work/git/hub/ns-rse/pytest-examples               (bare)
/mnt/work/git/hub/ns-rse/pytest-examples/contributing  ad56b91 [contributing]
/mnt/work/git/hub/ns-rse/pytest-examples/main          2f7c382 [main]

```

::::::::::::::::::::::::::::::::::::: callout

## References - a revelation

Whilst we have focused on consolidating our understanding of branches in this introductory episode there have been hints
as to the _true_ nature of branches in Git, have you worked out what this is yet?

Internally Git does not have branches at all! Branches are merely a reference to a series of commits and each commit in
a "branch" references the commit prior to it. In fact everything in Git that allows us to look at the
different states of the repository and move between them is a reference, whether that is a named branch, or a tag which
is a relative reference. They all point to a commit.

This was a revelation that came to me as I wrote the material for this Episode and thought it worth sharing.

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Branches and how they relate to each other are fundamental to collaborating using Git.
- The history of a branch is a series of commits and extends all the way back to the very first commit and _not_ the
  point at which it forked from its parents.
- Branches can be easily created, merged and deleted.
- Commits all have references and Git can move you between these references using `git commit` or compare them using
  `git diff`.
- Branches can become outdated as work progresses, they can be brought up-to-date with either `git merge` or `git rebase`.
- Worktrees can help alleviate some of the problems encountered when working with multiple branches.
- It is possible to track multiple origins.

::::::::::::::::::::::::::::::::::::::::::::::::

## Links

- [The Case for Pull Rebase](https://megakemp.com/2019/03/20/the-case-for-pull-rebase/)
- [Dealing with diverged git branches](https://jvns.ca/blog/2024/02/01/dealing-with-diverged-git-branches/)
- [git branches: intuition & reality](https://jvns.ca/blog/2023/11/23/branches-intuition-reality/)
- [git rebase: what can go wrong?](https://jvns.ca/blog/2023/11/06/rebasing-what-can-go-wrong-/)
- [Getting solid at Git rebase
  vs. merge](https://medium.com/@porteneuve/getting-solid-at-git-rebase-vs-merge-4fa1a48c53aa)

[atlassian_git_reset]: https://www.atlassian.com/git/tutorials/undoing-changes/git-reset
[bash]: https://www.gnu.org/software/bash/
[nano]: https://www.nano-editor.org/
[ohmyzsh]: https://ohmyz.sh/
[rerere]: https://git-scm.com/book/en/v2/Git-Tools-Rerere
[zerohero]: https://srse-git-github-zero2hero.netlify.app/
[zsh]: https://zsh.sourceforge.io/
