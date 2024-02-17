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
       commit id: "5-2315fa0"
       checkout main
       commit id: "6-93e787c"
-->

[![Basic GitHub Branches](https://mermaid.ink/img/pako:eNqVkTtrwzAUhf-KuWDcgh30sPVa29KlW7fiRZHkWKS2gitDU-P_XjshJUNKqab7-M65gjOBCdaBgjSdfO-jSqYstq5zmUqyrf5wWZ5kOx-fB31os3U7hKijewhd5-OL3rr3ZRqH0c11n1zeUs9peh5cxD9rc5Im3qqkBlSUnDQY4RpuA7iosMDMst8AUjDOZNOQ5G797_0Vtx10b9qFORe_OdBCmIpY466B1pl9GGPSad_flpWFcIYKqW_J_rpZFYTiqtHofzdZIanjgpsaIIfODQtql_SmFa_hlFwNK2n1sF-954UbD3aJ7Mn6GAZQa1g56DGG12NvLv2ZefR6N-juMjzo_i2E6xbUBJ-gWLXBkmAqCUKIlLLM4QiKSLmhAqFKcsIk54LOOXydDPCmZJRhxEqJkRCC8fkbTTe3dw?type=png)](https://mermaid.live/edit#pako:eNqVkTtrwzAUhf-KuWDcgh30sPVa29KlW7fiRZHkWKS2gitDU-P_XjshJUNKqab7-M65gjOBCdaBgjSdfO-jSqYstq5zmUqyrf5wWZ5kOx-fB31os3U7hKijewhd5-OL3rr3ZRqH0c11n1zeUs9peh5cxD9rc5Im3qqkBlSUnDQY4RpuA7iosMDMst8AUjDOZNOQ5G797_0Vtx10b9qFORe_OdBCmIpY466B1pl9GGPSad_flpWFcIYKqW_J_rpZFYTiqtHofzdZIanjgpsaIIfODQtql_SmFa_hlFwNK2n1sF-954UbD3aJ7Mn6GAZQa1g56DGG12NvLv2ZefR6N-juMjzo_i2E6xbUBJ-gWLXBkmAqCUKIlLLM4QiKSLmhAqFKcsIk54LOOXydDPCmZJRhxEqJkRCC8fkbTTe3dw)

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
means the most recent commit in the history of that branch.

You can change branches by using `git switch <branchname>`.

::::::::::::::::::::::::::::::::::::: callout

`git switch` was introduced in [Git
v2.23.0](https://github.blog/2019-08-16-highlights-from-git-2-23/#experimental-alternatives-for-git-checkout) along with
`git restore` to provide two separate commands for the functionality that was originally available in `git checkout`.
The main reason was to separate the functionality of `git checkout` which could 'switch' branches, including creating
branches using the `--branch`/`-b` flag, and change individual files with `git checkout [treeish] -- <filename>` (more
on this later).

Splitting this functionality means that `git switch` is solely for 'switch'ing branches whilst `git restore` is solely
concerned with `restore`ing files.

`git checkout` has not been deprecated and is still available and many people still use it.

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 1: What is the first and last commit on branch `divide`?

:::::::::::::::::::::::: solution

## Solution

``` bash
git switch divide
git log --pretty="%h %ad (%cr) %x09 %an : %s"
# TODO : Complete solution and add output once sample repository is in place
```

:::::::::::::::::::::::::::::::::

## Challenge 2: What commit did the `multiply` branch diverge from `master`?

Use `git log` to determine the commit that `multiply` diverged from `master`.

:::::::::::::::::::::::: solution

## Solution

``` bash
git switch multiply
git log --graph --pretty="%h %ad (%cr) %x09 %an : %s"
# TODO : Complete solution and add output once sample repository is in place
```

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: callout

There are a _lot_ of options for formatting the output of `git log` that allow you to restrict output to just the
information you are interested in and change the display. For this course we use the basic
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

The `git switch` command is the canonical method for working with branches it allows you to list, create and delete
branches along with a few other tasks.

To list the branches that are available you can just type `git branch` or optionally include the `--list` option. In the
`python-maths` repository you  have cloned you should see a number of branches listed. The branch you are currently
checked out on is listed first with an asterisk (`*` )at the start.

``` bash
git branch
# TODO : Complete output once sample repositopry is in place
```

### Creating Branches

You can create a new branch using `git switch -c <new_branch>`. By default it will use the branch you currently have
checked out as a basis for the new branch. If you wish to use a different branch as a basis you can do so by including
its name before the name of the new branch. To create a new branch called `ns-rse/test` you can use the following.

``` bash
git switch -c main ns-rse/test
```

::::::::::::::::::::::::::::::::::::: callout

Most of the time when creating branches you should do so from the `main` branch. It is therefore important to make sure
your local copy of the `main` branch is up-to-date. Before creating a branch you should therefore checkout `main` the
main branch and ensure it is up-to-date.

``` bash
git switch main
git pull
```

This means you can omit the branch you wish to use as the basis for the new branch when creating.
::::::::::::::::::::::::::::::::::::::::::::::::

### Naming Branches

Branch names can not include spaces, you should use underscores or dashes instead. You can include some special
characters too but I would avoid using `#` as this is the character used by most shells to indicate a comment and you
would therefore have to _always_ double quote the branch name at the command line.

A useful convention to use when creating branches is to construct the branch name from your GitHub/GitLab username
followed by a `/` and because you will typically be working on a particular issue include that number next followed by a
short few words which describe the work or issue. For example GitHub user `ns-rse` working on issue 1 to fix typehints
might create a branch called `ns-rse/1-fix-typehints` from main.

This structure is informative as it provides other people you collaborate with or who look at the repository an
indication of who created the branch what issue they are looking on if they wish to find out more about it and a very
short indication of what it is concerned about.

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 3: Create a new branch

Using the suggested nomenclature create a branch for an imaginary issue numbered `0` with the title `Add divide
function` and checkout the branch.

:::::::::::::::::::::::: solution

## Solution 1

``` bash
git switch main
git pull
git branch -c ns-rse/0-divide
git switch ns-rse/0-divide
# TODO : Complete solution and add output once sample repository is in place
```

:::::::::::::::::::::::::::::::::

:::::::::::::::::::::::: solution

## Solution 2

When you use `git branch -c` the branch only gets created for you, you then have to switch branch yourself. A shortcut
to both create and checkout a branch at the same time is provided by `git switch -c <new_branch>` so you could instead
have used the following.

``` bash
git switch main
git pull
git switch -c ns-rse/0-divide
# TODO : Complete solution and add output once sample repository is in place
```

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

### Deleting branches

Branches are typically short lived as they are created to address small focused pieces of work such as fixing a bug or
implementing a new feature before being merged into the `main` branch. Over time you will therefore accrue a number of
redundant, out-dated branches and it is therefore good practice to delete unwanted branches after they have been merged.

You can not delete a branch you currently have checked out so you must first checkout an alternative branch. Typically
this would be the `main` branch after your Pull Request has been merged and the changes you were working on have been
incorporated.

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 4: Delete the branch you just created

Pretending the branch you just created has been merged into the `main` branch via a Pull Request delete the now
redundant branch.

:::::::::::::::::::::::: solution

## Solution 1

You can use the `-d` or `--delete` flag to delete a branch.

``` bash
git switch main
git branch -d ns-rse/0-divide
# TODO : Complete solution and add output once sample repository is in place
```

:::::::::::::::::::::::::::::::::

:::::::::::::::::::::::: solution

## Solution 2

You can use the shortcut `git switch -` to switch to the last branch you were on (this is shortcut that is common to the
Bash shell when navigating directories `cd -` will change directory to the previous directory you were in).

``` bash
git switch -
git branch -d ns-rse/0-divide
# TODO : Complete solution and add output once sample repository is in place
```

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: callout
You were able to delete the branch you created because you hadn't made any changes to it. If you have made changes on a
branch and they have not been merged into `main` then Git will warn you of this and refuse to delete the branch. This
can be over-ridden with the `--force` flag or the shorthand `-D` which is the same as `--delete --force`.

``` bash
git switch main
git -D ns-rse/0-divide
```

Be _very_ careful when forcing deletions, if you have not pushed your changes to the remote `origin` then you will lose
them.
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
<!-- https://mermaid.live/edit#pako:eNp1kT1rwzAQhv-KOTBerCDJ1oe1lSa0Q7duxYsiyYlobAdHgabG_71ygguFWpPuuecOjncE01sHCtJ09J0PKhmzcHSty1SS7fXFZXmSHXx4GfT5mM3doQ86uOe-bX1403t3ijQMVzfVXbK8-J_S9AGW4d-2uY8m3qqkBoxKQRuCSQ3_CwQxIgm3fE2giAteNQ1dEwokDaPWuDWhRNKZQlZ6TWCIFoQ1Gq8JHFWFE1KYNUGgvSmLav1MibTExjWyhiTow4xed0_bRYccWje02tsY1TizGu4x1TCrVg-fszpF73q2MZ-d9aEfQM3J5KCvoX-_dWapH87W68OgW1CNPl0iPevuo-__1KBG-AJFRblhFDMuZSk445TlcItYlhuBaYEJpixex6ccvu8b8EYyIbGsGClwbFA6_QAk7q7t
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

[![Git Branch with History to Checkout](https://mermaid.ink/img/pako:eNp1kT1rwzAQhv-KOTBerCDJ1oe1lSa0Q7duxYsiyYlobAdHgabG_71ygguFWpPuuecOjncE01sHCtJ09J0PKhmzcHSty1SS7fXFZXmSHXx4GfT5mM3doQ86uOe-bX1403t3ijQMVzfVXbK8-J_S9AGW4d-2uY8m3qqkBoxKQRuCSQ3_CwQxIgm3fE2giAteNQ1dEwokDaPWuDWhRNKZQlZ6TWCIFoQ1Gq8JHFWFE1KYNUGgvSmLav1MibTExjWyhiTow4xed0_bRYccWje02tsY1TizGu4x1TCrVg-fszpF73q2MZ-d9aEfQM3J5KCvoX-_dWapH87W68OgW1CNPl0iPevuo-__1KBG-AJFRblhFDMuZSk445TlcItYlhuBaYEJpixex6ccvu8b8EYyIbGsGClwbFA6_QAk7q7t?type=png)](https://mermaid.live/edit#pako:eNp1kT1rwzAQhv-KOTBerCDJ1oe1lSa0Q7duxYsiyYlobAdHgabG_71ygguFWpPuuecOjncE01sHCtJ09J0PKhmzcHSty1SS7fXFZXmSHXx4GfT5mM3doQ86uOe-bX1403t3ijQMVzfVXbK8-J_S9AGW4d-2uY8m3qqkBoxKQRuCSQ3_CwQxIgm3fE2giAteNQ1dEwokDaPWuDWhRNKZQlZ6TWCIFoQ1Gq8JHFWFE1KYNUGgvSmLav1MibTExjWyhiTow4xed0_bRYccWje02tsY1TizGu4x1TCrVg-fszpF73q2MZ-d9aEfQM3J5KCvoX-_dWapH87W68OgW1CNPl0iPevuo-__1KBG-AJFRblhFDMuZSk445TlcItYlhuBaYEJpixex6ccvu8b8EYyIbGsGClwbFA6_QAk7q7t)

Here we have a simply linear history and the `HEAD` of branch is on commit `8-a80cef8` If you want to checkout commit
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
switch back the commit `HEAD` points to and you are told you can do this with `git switch -`. If you want to make
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

The `tests/test_add.py` file no longer exists on the `HEAD` of the `main** branch!

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

Fortunately Git can help you here with `git diff`.

## Ooops! I Did It Again

Nothing to do with Brittney Spears but you are at some stage likely to commit changes to the wrong branch. This can
easily happen when start working on an issue without first creating a new branch to contain the work and you commit the
changes to either the `main` branch, which is often protected so you won't be able to push your changes or the last
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
[![Relative Refs on a Git Branch](https://mermaid.ink/img/pako:eNp10k1rgzAYB_CvIg8UL6bkxbzobaxlO-y22_CSxtiGVS02hXXiPvtii5uDmVPyf34JJE96MG1pIYfVqneN83nUx_5gaxvnUbzTZxsnUbx3_qnTp0M8VrvWa28f27p2_kXv7DGkvrvYoWiiaYT5sFrdg2nzT9nctkauzKMCMEolrQgmBURe78foSxXwPyaIE0VEKWZYLmGKhBRZVdEZFkuYIWU4LY2dYb6EU6SsYSrTM5wuYY4oI7zSeIbZEhYoY1YqaWaYLmGJdiZl2Z-nI0tYIa2wsZX6xc_bh83EIYHadrV2ZfgK_ZgVcPsGBYy01N37SIfgLqcy9H9bOt92kI-dT0BffPt6bcy0vpuN0_tO15BX-ngO6Uk3b237Zw15Dx-QU5muOcVcKJVKwQXlCVxDrNK1xJRhgikPNxVDAp-3E_BacamwyjhhOBQoHb4BB-fKEQ?type=png)](https://mermaid.live/edit#pako:eNp10k1rgzAYB_CvIg8UL6bkxbzobaxlO-y22_CSxtiGVS02hXXiPvtii5uDmVPyf34JJE96MG1pIYfVqneN83nUx_5gaxvnUbzTZxsnUbx3_qnTp0M8VrvWa28f27p2_kXv7DGkvrvYoWiiaYT5sFrdg2nzT9nctkauzKMCMEolrQgmBURe78foSxXwPyaIE0VEKWZYLmGKhBRZVdEZFkuYIWU4LY2dYb6EU6SsYSrTM5wuYY4oI7zSeIbZEhYoY1YqaWaYLmGJdiZl2Z-nI0tYIa2wsZX6xc_bh83EIYHadrV2ZfgK_ZgVcPsGBYy01N37SIfgLqcy9H9bOt92kI-dT0BffPt6bcy0vpuN0_tO15BX-ngO6Uk3b237Zw15Dx-QU5muOcVcKJVKwQXlCVxDrNK1xJRhgikPNxVDAp-3E_BacamwyjhhOBQoHb4BB-fKEQ)

If you want to undo the _last_ commit then you can do this using `git reset --soft HEAD~1`.

``` bash
git reset --soft HEAD~1
## TODO : Complete output
```

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 6: Commits on the wrong branch

- Checkout the `main` branch.
- Create a new file using `echo "# How to Contribute to this repo" > CONTRIBUTING.md`
- Stage and commit the file to the `main` branch of your repository.

Ooops you've just committed to the `main` branch which is protected so you can't push your changes. Now move the commit
to a new branch so you can push them.

- Reset the change.
- Create a new branch called `<github_user>/contributing`.
- Stage and commit the file to `<github_user>/contributing`.

:::::::::::::::::::::::: solution

## Solution

You can use the `-d` or `--delete` flag to delete a branch.

``` bash
git switch main
echo "# How to Contribute to this repo" > CONTRIBUTING.md
git add CONTRIBUTING.md
git commit -m "Adding contributing guideline template"
git reset --hard HEAD~1
git switch -c ns-rse/contributing
git add CONTRIBUTING.md
git commit -m "Adding contributing guideline template"
```

:::::::::::::::::::::::::::::::::

:::::::::::::::::::::::: solution

## Solution

Alternatively you can checkout the previous commit _before_ you added the file by mistake, create the `<github_user>/contributing`
branch,  and `git cherrypick` the commit from `main` which contains the `CONTRIBUTING.md` file and _then_ remove the
commit from main.

``` bash
# TODO get commit hash of last commit
git checkout HEAD~1
git switch -c ns-rse/contributing
git cherrypick <hash>
git
# TODO : Complete solution and add output once sample repository is in place
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

**NB** You could also copy the file using the older `git checkoug main -- CONTRIBUTING.md`.

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

You then have to decide how to add the changes to a branch. If they are brand new then you can create a new branch and
add them. If however they were meant to be added to an existing branch you face a slight problem as if you try to switch
branches you will be told that this would over-write the changes to the files you have just modified and unstaged and
you don't want to lose your work.

The solution here is to use `git stash` to temporarily store the unstaged changes, switch branches to the target branch
they should be on, and you can then un-stash them (known as `pop`ing** onto the correct branch.

### `git revert`

`git reset` is destructive, you can lose work using it and it is advisable _not_ to use it when you have more than one
commit you wish to undo as you lose the intermediary work between commits as you are restored to the commit you reset
to. Fortunately Git has

::::::::::::::::::::::::::::::::::::: callout
The differences between `reset` and `revert` is that one is destructive

``` bash
git checkout main
git -D ns-rse/0-divide
```

Be _very_ careful when forcing deletions, if you have not pushed your changes to the remote `origin` then you will lose
them.
::::::::::::::::::::::::::::::::::::::::::::::::

## Diverging Branches

As you and your collaborator(s) work on your repository you may find that changes others have made get merged into the
`main` before you have finished your work. This has in fact just happened, the work to add a Zero Division exception
has been merged via a Pull Request, but the work to address the Square Root function hasn't yet been

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 7: Diverge Branches

The `python-maths` repository has some issue templates setup which include instructions to guide you through the steps.

Once you have been assigned the tasks please work through the instructions, ticking off the tasks.  **NB** note that
only one of the tasks includes making a pull request, please do _not_ merge the other task.

:::::::::::::::::::::::: solution

## Solution

Assign the _Zero Division_ issue to one person and the _Square root_ to the other

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

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

[![Branch `ns-rse/2-square-root` is behind `main`](https://mermaid.ink/img/pako:eNqtkl1vmzAYhf8KshSxSZjaBn_A3bRW28Xueldx49gmsVpwZky1FvHfZ9Imy6QmqtT6Cp_38XkP0pmActqAGqxWk-1tqJMpDVvTmbRO0rUcTJol6caGH17utuky9S7IYL67rrPhl1ybh6gGP5q56ZPDid_zavUiHB4fx2r_NLG6ThqAYMlJixFuwNsAhhQLzDQ7BxDIOKvaliRflrxfT7i1l73aRqYfoB_MFYbPxjuo7aMdrOsvkAQOv0fpDfTOhdPNW6Pu3Rje4_lfygIKRYlW5qLZ2bWnViUURhWikp-Ri0JSYNpK9PFcDFaF4YKrt6w6afuj2hm_MReivhpyuFZlUS3dSILcLNIjynF-tis_b75dH2YgA3FNXKtjuadFa8C-2A1YUC39_YLOkZNjcLdPvQL10uMMjDsdG35t5cbLDtStfBiO6o22wfkDuZP9nXP_mHgH9QT-gJrwMqcEUSZEyRllhGbgKcqizDkiBcKI0PhrbM7A894B5YJygURFcYHigJD5LybqESw?type=png)](https://mermaid.live/edit#pako:eNqtkl1vmzAYhf8KshSxSZjaBn_A3bRW28Xueldx49gmsVpwZky1FvHfZ9Imy6QmqtT6Cp_38XkP0pmActqAGqxWk-1tqJMpDVvTmbRO0rUcTJol6caGH17utuky9S7IYL67rrPhl1ybh6gGP5q56ZPDid_zavUiHB4fx2r_NLG6ThqAYMlJixFuwNsAhhQLzDQ7BxDIOKvaliRflrxfT7i1l73aRqYfoB_MFYbPxjuo7aMdrOsvkAQOv0fpDfTOhdPNW6Pu3Rje4_lfygIKRYlW5qLZ2bWnViUURhWikp-Ri0JSYNpK9PFcDFaF4YKrt6w6afuj2hm_MReivhpyuFZlUS3dSILcLNIjynF-tis_b75dH2YgA3FNXKtjuadFa8C-2A1YUC39_YLOkZNjcLdPvQL10uMMjDsdG35t5cbLDtStfBiO6o22wfkDuZP9nXP_mHgH9QT-gJrwMqcEUSZEyRllhGbgKcqizDkiBcKI0PhrbM7A894B5YJygURFcYHigJD5LybqESw)

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

There are two approaches to solving this `git merge` (merging) and `git rebase` (rebasing)

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

[![Merging `main` into `ns-rse/2-square-root`](https://mermaid.ink/img/pako:eNqtk81unDAUhV8FWRrRSpjaBv_ArmqidtGuuqvYeIyZsRLw1JioCeLda5gymUqZUdWEFZz73XOPke8IlK01KMFmM5rO-DIaY7_XrY7LKN7KXsdJFO-M_-zkYR_PVWe99PqTbVvjv8qtvg-qd4Oeqi5an_A-bTZHYW0-ldXSGpm6jCqAYM5JgxGuwMsAhhQLzGp2CSCQcVY0DYnezXnfn3FbJzu1D0zXQ9frDxg-aWdhbR5Mb2x3hSSw_zlIp6Gz1p9P3mt1Zwf_L55_pcygUJTUSl81uzj23CqHQqtMFPItclFIMkwbiV6fi8Ei01xw9ZJVK013UlvtdvpK1D-GHG5VnhXz3Yi83M3SA0pxevGufLn9ePNf51gDLSnX8QJKgZRuxPP4bwt3pI7NIAGhOSh12KFx1iqw7E8F5oZaursZnQInB2-_P3YKlPO6JGA41GGRbozcOdmCspH3_Um9rY23biUPsvth7TMTvkE5gl-gJDxPKUGUCZFzRhmhCXgMsshTjkiGMCI0_EE2JeBpcUCpoFwgUVCcoVAgZPoNELowaQ?type=png)](https://mermaid.live/edit#pako:eNqtk81unDAUhV8FWRrRSpjaBv_ArmqidtGuuqvYeIyZsRLw1JioCeLda5gymUqZUdWEFZz73XOPke8IlK01KMFmM5rO-DIaY7_XrY7LKN7KXsdJFO-M_-zkYR_PVWe99PqTbVvjv8qtvg-qd4Oeqi5an_A-bTZHYW0-ldXSGpm6jCqAYM5JgxGuwMsAhhQLzGp2CSCQcVY0DYnezXnfn3FbJzu1D0zXQ9frDxg-aWdhbR5Mb2x3hSSw_zlIp6Gz1p9P3mt1Zwf_L55_pcygUJTUSl81uzj23CqHQqtMFPItclFIMkwbiV6fi8Ei01xw9ZJVK013UlvtdvpK1D-GHG5VnhXz3Yi83M3SA0pxevGufLn9ePNf51gDLSnX8QJKgZRuxPP4bwt3pI7NIAGhOSh12KFx1iqw7E8F5oZaursZnQInB2-_P3YKlPO6JGA41GGRbozcOdmCspH3_Um9rY23biUPsvth7TMTvkE5gl-gJDxPKUGUCZFzRhmhCXgMsshTjkiGMCI0_EE2JeBpcUCpoFwgUVCcoVAgZPoNELowaQ)

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

[![Git Rebase to bring the diverged branch `ns-rse/2-square-root` upto date with `main`](https://mermaid.ink/img/pako:eNqtkk9v2yAYxr-KhRS5k4wH2Pyxr2u1y07bbfKFAE5Qa5NiXK21_N0HaVNlUhpN2jjB8_54gId3AcppA1qw2Sx2tKHNljzszWDyNsu3cjJ5keU7G756edjnqepdkMF8ccNgwze5NQ9RDX42azdmpxHn62bzKpw2v5fVcWtmdZt1AMGakx4j3IHLAIYUC8w0-wggkHHW9D3JbtJ9P51xWy9HtY_MOEE_mc8YvhjvoLZPdrJuPHfcG3Xv5vBX7PnpFRSKEq3M_zCjkFSY9hJdMhukHd_VwfidueL_ZsjhVtVVk9LNgtwl6QmVuLyQ9lv5u0khXsmQwOlxlt5A71y4-ugPyfMn11AYVYlG_rsVg01luODqBIACxJxibjr295K0Dhx7uwOJ19LfJ3SN3HzQsanvtA3Ogza1cwHkHNyP51Gd1q_MrZU7LwfQ9vJhiupBjj-d-2MN2gX8Ai3hdUkJokyImjPKCC3Ac5RFXXJEKoQRofFv2FqAl6MDKgXlAomG4grFAiHrb-1-Ets?type=png)](https://mermaid.live/edit#pako:eNqtkk9v2yAYxr-KhRS5k4wH2Pyxr2u1y07bbfKFAE5Qa5NiXK21_N0HaVNlUhpN2jjB8_54gId3AcppA1qw2Sx2tKHNljzszWDyNsu3cjJ5keU7G756edjnqepdkMF8ccNgwze5NQ9RDX42azdmpxHn62bzKpw2v5fVcWtmdZt1AMGakx4j3IHLAIYUC8w0-wggkHHW9D3JbtJ9P51xWy9HtY_MOEE_mc8YvhjvoLZPdrJuPHfcG3Xv5vBX7PnpFRSKEq3M_zCjkFSY9hJdMhukHd_VwfidueL_ZsjhVtVVk9LNgtwl6QmVuLyQ9lv5u0khXsmQwOlxlt5A71y4-ugPyfMn11AYVYlG_rsVg01luODqBIACxJxibjr295K0Dhx7uwOJ19LfJ3SN3HzQsanvtA3Ogza1cwHkHNyP51Gd1q_MrZU7LwfQ9vJhiupBjj-d-2MN2gX8Ai3hdUkJokyImjPKCC3Ac5RFXXJEKoQRofFv2FqAl6MDKgXlAomG4grFAiHrb-1-Ets)

Useful resources...

## Making Life Easier

### Worktrees

Sometimes you will want to switch between branches that are all in development in the middle of work. If you've made
changes to files that you have not saved and committed Git is likely to tell you that the changes made to your files
will be over-written.

## References - a revelation

Whilst we have focused on consolidating our understanding of branches in this introductory episode there have been hints
as to the _true_ nature of branches in Git, have you worked out what this is yet?

Internally Git does not have branches at all! Branches are merely a reference to the most recent commit on a series of
commits, each of which references the commit prior to it. In fact everything in Git that allows us to look at the
different states of the repository and move between them is a reference, whether that is a named branch, or a tag a
relative reference. They all point to a commit.

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

[bash]: https://www.gnu.org/software/bash/
[ohmyzsh]: https://ohmyz.sh/
[zsh]: https://zsh.sourceforge.io/
