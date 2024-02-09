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
     https://gist.github.com/ns-rse/08fb86b003a26f7855281eeea88566d0#file-git_graph_branching_01-js
-->
<!-- https://mermaid.live/edit#pako:eNqlUD1vgzAQ_SvWScgLinBpCvaaRlm6dau8HMYNVmIbOUZqivjvNSC6RR1607v3NbwRlG81CMiy0TgTBRlp7LTVVBDa4E3TnNCziaeAfUdnNfiIUR-8tSa-YaOviY1h0JN0ZLuEpyxbiS38K6sl-uhtAjrVEQkrkPAw12l18UMkFo370_S_PsjB6pCENi01zqKEZSUJIsEWw2UunpJv6Ns0z7E10QcQ8zA54BD9-92p7V89rwbPAS2IT7zeEtuj-_Debqb0ghjhCwR7qncvnLGiKjhnVV3vc7iDKBnb8bJgBX_el2VVVFMO30sBm34AnPOOqQ -->
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

You can change branches by using `git checkout <branchname>`.

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 1: What is the first and last commit on branch `divide`?

``` bash
git checkout divide
git log --pretty="%h %ad (%cr) %x09 %an : %s"
# TODO : Complete solution and add output once sample repository is in place
```

:::::::::::::::::::::::::::::::::

## Challenge 2: What commit did the `multiply` branch diverge from `master`?

Use `git log` to determine the commit that `multiply` diverged from `master`.

:::::::::::::::::::::::: solution

``` bash
git checkout multiply
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

The `git branch` command is the canonical method for working with branches it allows you to list, create and delete
branches along with a few other tasks.

To list the branches that are available you can just type `git branch` or optionally include the `--list` option. In the
`python-maths` repository you  have cloned you should see a number of branches listed. The branch you are currently
checked out on is listed first with an asterisk (`*` )at the start.

``` bash
git branch
# TODO : Complete output once sample repositopry is in place
```

### Creating Branches

You can create a new branch using `git branch -c <new_branch>`. By default it will use the branch you currently have
checked out as a basis for the new branch. If you wish to use a different branch as a basis you can do so by including
its name before the name of the new branch. To create a new branch called `ns-rse/test` you can use the following.

``` bash
git branch -c main ns-rse/test
```

::::::::::::::::::::::::::::::::::::: callout
Most of the time when creating branches you should do so from the `main` branch. It is therefore important to make sure
your local copy of the `main` branch is up-to-date. Before creating a branch you should therefore checkout `main` the
main branch and ensure it is up-to-date.

``` bash
git checkout main
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

``` bash
git checkout main
git pull
git branch -c ns-rse/0-divide
git checkout ns-rse/0-divide
# TODO : Complete solution and add output once sample repository is in place
```

:::::::::::::::::::::::::::::::::

:::::::::::::::::::::::: solution

## Method 2 : `git checkout -b`

When you use `git branch -c` the branch only gets created for you, you then have to switch branch yourself. A shortcut
to both create and checkout a branch at the same time is provided by `git checkout -b <new_branch>` so you could instead
have used the following.

``` bash
git checkout main
git pull
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

## Method 1 : `git branch -d`

``` bash
git checkout main
git pull
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
git checkout main
git -D ns-rse/0-divide
```

Be _very_ careful when forcing deletions, if you have not pushed your changes to the remote `origin` then you will lose
them.
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 5: Assign and complete Issues

The `python-maths` repository has some issue templates setup which include instructions to guide you through the
Once you have been assigned the tasks please work through the instructions, ticking off the tasks.  **NB** note that
only one of the tasks includes making a pull request, please do _not_ merge the other task.

:::::::::::::::::::::::: solution

### TODO

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

As you and your collaborator(s) work on your repository you may find that changes others have made get merged into the
`main` before you have finished your work. This has in fact just happened, the work to add a Zero Division exception
has been merged via a Pull Request, but the work to address the Square Root function hasn't yet been

<!-- Source for Mermaid diagram :
     https://gist.github.com/ns-rse/08fb86b003a26f7855281eeea88566d0#file-git_graph_branching_02-js
<!-- https://mermaid.live/edit#pako:eNqtkl1vmzAYhf8KshSxSZjaBn_A3bRW28Xueldx49gmsVpwZky1FvHfZ9Imy6QmqtT6Cp_38XkP0pmActqAGqxWk-1tqJMpDVvTmbRO0rUcTJol6caGH17utuky9S7IYL67rrPhl1ybh6gGP5q56ZPDid_zavUiHB4fx2r_NLG6ThqAYMlJixFuwNsAhhQLzDQ7BxDIOKvaliRflrxfT7i1l73aRqYfoB_MFYbPxjuo7aMdrOsvkAQOv0fpDfTOhdPNW6Pu3Rje4_lfygIKRYlW5qLZ2bWnViUURhWikp-Ri0JSYNpK9PFcDFaF4YKrt6w6afuj2hm_MReivhpyuFZlUS3dSILcLNIjynF-tis_b75dH2YgA3FNXKtjuadFa8C-2A1YUC39_YLOkZNjcLdPvQL10uMMjDsdG35t5cbLDtStfBiO6o22wfkDuZP9nXP_mHgH9QT-gJrwMqcEUSZEyRllhGbgKcqizDkiBcKI0PhrbM7A894B5YJygURFcYHigJD5LybqESw -->
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
     https://gist.github.com/ns-rse/08fb86b003a26f7855281eeea88566d0#file-git_graph_branching_03-js
-->
<!-- https://mermaid.live/edit#pako:eNqtk81unDAUhV8FWRrRSpjaBv_ArmqidtGuuqvYeIyZsRLw1JioCeLda5gymUqZUdWEFZz73XOPke8IlK01KMFmM5rO-DIaY7_XrY7LKN7KXsdJFO-M_-zkYR_PVWe99PqTbVvjv8qtvg-qd4Oeqi5an_A-bTZHYW0-ldXSGpm6jCqAYM5JgxGuwMsAhhQLzGp2CSCQcVY0DYnezXnfn3FbJzu1D0zXQ9frDxg-aWdhbR5Mb2x3hSSw_zlIp6Gz1p9P3mt1Zwf_L55_pcygUJTUSl81uzj23CqHQqtMFPItclFIMkwbiV6fi8Ei01xw9ZJVK013UlvtdvpK1D-GHG5VnhXz3Yi83M3SA0pxevGufLn9ePNf51gDLSnX8QJKgZRuxPP4bwt3pI7NIAGhOSh12KFx1iqw7E8F5oZaursZnQInB2-_P3YKlPO6JGA41GGRbozcOdmCspH3_Um9rY23biUPsvth7TMTvkE5gl-gJDxPKUGUCZFzRhmhCXgMsshTjkiGMCI0_EE2JeBpcUCpoFwgUVCcoVAgZPoNELowaQ -->
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

Describe how to keep a development branch up-to-date, two strategies

- `git merge` the `main` branch into development.
- `git rebase` development branch on to `main`.

Useful resources...

## Ooops I Did It Again

Nothing to do with Brittney Spears but you are at some stage likely to commit changes to the wrong branch. This can
easily happen when start work on an issue without first creating a new branch to contain the work and you commit the
changes to either the `main** branch or the last branch you were working on.

### Reset and Commit

One solution to solve this with Git is to `git reset` the branch to which you have just mistakenly made the commit. This
removes reference to the changes from the Git history but leaves the changes to the files in place and they appear as
`unstaged` files.

You then have to decide how to add the changes to a branch. If they are brand new then you can create a new branch and
add them. If however they were meant to be added to an existing branch you face a slight problem as if you try to switch
branches you will be told that this would over-write the changes to the files you have just modified and unstaged and
you don't want to lose your work.

The solution here is to use `git stash` to temporarily store the unstaged changes, switch branches to the target branch
they should be on, and you can then un-stash them (known as `pop`ing) onto the correct branch.

### Cherrypicking

## Worktrees

Sometimes you will want to switch between branches that are all in development in the middle of work. If you've made
changes to files that you have not saved and committed Git is likely to tell you that the changes made to your files
will be over-written.

## Tracking multiple Origins

Describe how to track multiple origins

- First
- Second
- Third
- Fourth

::::::::::::::::::::::::::::::::::::::::::::::::

[bash]: https://www.gnu.org/software/bash/
[ohmyzsh]: https://ohmyz.sh/
[zsh]: https://zsh.sourceforge.io/
