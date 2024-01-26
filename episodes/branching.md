---
title: "Collaborative Git : Branching"
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

<!-- Source for Mermaid diagram : -->
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

Inline instructor notes can help inform instructors of timing challenges
associated with the lessons. They appear in the "Instructor View"

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 1: What is the first and last commit on branch `divide`?

Use `git log` to determine the first and last commit on the `divide` branch. You can use the
`--pretty="%h %ad (%cr) %x09 %an : %s"` option and formatting to simplify the output to a single line.

:::::::::::::::::::::::: solution

## Output

```output
git checkout divide
git log --pretty="%h %ad (%cr) %x09 %an : %s"
# TODO : Complete solution and add output once sample repository is in place
```

:::::::::::::::::::::::::::::::::

## Challenge 2: What commit did the `multiply` branch diverge from `master`?

Use `git log` to determine the commit that `multiply` diverged from `master`.

:::::::::::::::::::::::: solution

## Output

``` output
git checkout multiply
git log --graph --pretty="%h %ad (%cr) %x09 %an : %s"
# TODO : Complete solution and add output once sample repository is in place
```

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: callout

There are a _lot_ of options for formatting the output of `git log` that allow you to restrict output to just the
information you are interested in and change the display. For this course we use the basic
`--pretty="%h %ad (%cr) %x09 %an : %s` but the [ZSH][zsh] framework [Oh My Zsh][ohmyzsh] includes a number of aliases
for formatting Git log output.

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

[zsh]: https://zsh.sourceforge.io/
[ohmyzsh]: https://ohmyz.sh/
