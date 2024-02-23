---
title: "Hooks"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions

- What the hell are hooks?
- How can hooks improve my development workflow?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Understand the concept of a hook.
- Know what the different types of hooks are and where they are stored.
- Understand how pre-commit fits in to the hooks framework.

::::::::::::::::::::::::::::::::::::::::::::::::

## What are hooks?

Hooks are actions, typically a one or more scripts, that are run in response to a particular event. Git has a number of
stages at which hooks can be run, events such as `commit`, `push`, `pull` all have hooks that can run `pre` (before) or
`post` (after) the action and these are _really_ useful for helping automate your workflow as they can capture problems
with linting and tests much earlier in the development cycle than for example Continuous Integration failing after pull
requests have been made.

In a Git repository hooks live in the `.git/hooks` directory and are short [Bash][bash] scripts that are executed at the
relevant stage. We can list the contents of this directory with `ls -lha .git/hooks` and you will see there are a number
of executable files with names that indicate at what stage they are run but all have the `.sample` extension which means
they are _not_ executed in response to any of the actions.

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Make sure the audience understands what the `commit`, `push` and `pull` events are and they they are actions for git to
make on the repository at different stages in the Git workflow.

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

``` output
❱ mkdir test
❱ cd test
❱ git init
❱ ls -lha .git/hooks
drwxr-xr-x neil neil 4.0 KB Fri Feb 23 10:40:42 2024 .
drwxr-xr-x neil neil 4.0 KB Fri Feb 23 10:40:46 2024 ..
.rwxr-xr-x neil neil 478 B  Fri Feb 23 10:40:42 2024 applypatch-msg.sample
.rwxr-xr-x neil neil 896 B  Fri Feb 23 10:40:42 2024 commit-msg.sample
.rwxr-xr-x neil neil 4.6 KB Fri Feb 23 10:40:42 2024 fsmonitor-watchman.sample
.rwxr-xr-x neil neil 189 B  Fri Feb 23 10:40:42 2024 post-update.sample
.rwxr-xr-x neil neil 424 B  Fri Feb 23 10:40:42 2024 pre-applypatch.sample
.rwxr-xr-x neil neil 1.6 KB Fri Feb 23 10:40:42 2024 pre-commit.sample
.rwxr-xr-x neil neil 416 B  Fri Feb 23 10:40:42 2024 pre-merge-commit.sample
.rwxr-xr-x neil neil 1.3 KB Fri Feb 23 10:40:42 2024 pre-push.sample
.rwxr-xr-x neil neil 4.8 KB Fri Feb 23 10:40:42 2024 pre-rebase.sample
.rwxr-xr-x neil neil 544 B  Fri Feb 23 10:40:42 2024 pre-receive.sample
.rwxr-xr-x neil neil 1.5 KB Fri Feb 23 10:40:42 2024 prepare-commit-msg.sample
.rwxr-xr-x neil neil 2.7 KB Fri Feb 23 10:40:42 2024 push-to-checkout.sample
.rwxr-xr-x neil neil 2.3 KB Fri Feb 23 10:40:42 2024 sendemail-validate.sample
.rwxr-xr-x neil neil 3.6 KB Fri Feb 23 10:40:42 2024 update.sample

```

If you create a repository on [GitHub][gh], [GitLab][gl] or another forge when you clone it locally these samples are
created on your system. They are _not_ part of the repository itself as files under the `.git` directory are not part of
the repository.

::::::::::::::::::::::::::::::::::::: challenge

## Checking out sample hooks

Lets take a look at the hooks in the [`python-maths`][pm] repository you have cloned for this course.

:::::::::::::::::::::::: solution

## Challenge 1: What does `.git/hooks/pre-push.sample` do?

Git will have populated the `.git/hooks` directory automatically when you cloned the [`python-maths`][pm].

1. Change directory to the clone `python-maths` directory.
2. Look at the file `.git/hooks/pre-push.sample`, what does this sample prevent from happening?

``` bash
❱ mkdir test
❱ cd test
❱ git init
❱ cat .git/hooks/pre-push.sample
#!/bin/sh

# An example hook script to verify what is about to be pushed.  Called by "git
# push" after it has checked the remote status, but before anything has been
# pushed.  If this script exits with a non-zero status nothing will be pushed.
#
# This hook is called with the following parameters:
#
# $1 -- Name of the remote to which the push is being done
# $2 -- URL to which the push is being done
#
# If pushing without using a named remote those arguments will be equal.
#
# Information about the commits which are being pushed is supplied as lines to
# the standard input in the form:
#
#   <local ref> <local oid> <remote ref> <remote oid>
#
# This sample shows how to prevent push of commits where the log message starts
# with "WIP" (work in progress).

remote="$1"
url="$2"

zero=$(git hash-object --stdin </dev/null | tr '[0-9a-f]' '0')

while read local_ref local_oid remote_ref remote_oid
do
    if test "$local_oid" = "$zero"
    then
        # Handle delete
        :
    else
        if test "$remote_oid" = "$zero"
        then
            # New branch, examine all commits
            range="$local_oid"
        else
            # Update to existing branch, examine new commits
            range="$remote_oid..$local_oid"
        fi

        # Check for WIP commit
        commit=$(git rev-list -n 1 --grep '^WIP' "$range")
        if test -n "$commit"
        then
            echo >&2 "Found WIP commit in $local_ref, not pushing"
            exit 1
        fi
    fi
done

exit 0
```

When enabled this hook will "_prevent push of commits where the log message starts with "WIP" (work in progress)_"

:::::::::::::::::::::::::::::::::

:::::::::::::::::::::::: solution

## Challenge 2: Enable the `pre-push` hook and test it

This sounds like a good idea as it, notionally, prevents people from pushing work that is in progress, if they are in
the habit of starting commit messages with "WIP".

1. Enable the hook.
2. Create a new branch `<github-user>/test-hook` to test the hook on.
3. Make an empty commit with a message that starts with `WIP` e.g. `git commit --allow-empty "WIP - testing the
   pre-push commit"`. Was the commit pushed?
4. Delete the branch you created.

``` bash
❱ cd ~/path/to/python-maths
❱ cp .git/hooks/pre-push.sample .git/hooks/pre-push
❱ git switch -c ns-rse/test-hook
❱ git commit --allow-empty -m "WIP - testing the pre-push hook"
❱ git push
ERROR: Permission to ns-rse/python-maths.git denied to slackline.
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
❱ git switch main
❱ git branch -D ns-rse/test-hook
```

:::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

## Pull before push

::::::::::::::::::::::::::::::::::::: callout

You may have encountered the [non-fast-forward
error](https://docs.github.com/en/get-started/using-git/dealing-with-non-fast-forward-errors) when attempting to push
your changes to a remote. As the message shows this is because there are changes to the remote branch that are not in
the local branch and you are advised to `git pull` before attempting to `git push` again.

``` bash
❱ git push origin main
> To https://github.com/USERNAME/REPOSITORY.git
>  ! [rejected]        main -> main (non-fast-forward)
> error: failed to push some refs to 'https://github.com/USERNAME/REPOSITORY.git'
> To prevent you from losing history, non-fast-forward updates were rejected
> Merge the remote changes (e.g. 'git pull') before pushing again.  See the
> 'Note about fast-forwards' section of 'git push --help' for details.
```

A simple addition you can add to the `.git/hooks/pre-push` script is to have it `git pull` before attempting to make a
`git push` which will mean you are unlikely to see the above message in the future.

::::::::::::::::::::::::::::::::::::::::::::::::

## Pre-Commit

Pre-commit hooks are _really_ useful to the extent that they require special discussion because there is a framework for
using such hooks, [pre-commit][pre-commit] that makes it incredibly easy to add (and configure) some really useful
pre-commit hooks to your workflow.

### Why are Pre-Commit hooks so important?

You may be wondering why running hooks prior to commits is so important. The short answer is that it reduces the
feedback loop and speeds up the pace of development. The long answer is that it only really becomes apparent after doing
so we're going to have a go at installing and enabling some `pre-commit` hooks on our code base, making some changes and
committing them.

#### Installation

[pre-commit][pre-commit] is written in [Python][python] and hooks are available that lint, check and test many languages
other than Python.

### Adding Hooks

Which hooks you use will depend largely on the language you are using.

### Pre-commit.ci

::::::::::::::::::::::::::::::::::::: keypoints

- Use hooks liberally, they save you time.
- Enable hooks in CI so contributions are checked.

::::::::::::::::::::::::::::::::::::::::::::::::

[bash]: https://www.gnu.org/software/bash/
[gh]: https://github.com
[gl]: https://gitlab.com
[pre-commit]: https://pre-commit.com
[python]: https://python.org
[pm]: https://github.com/ns-rse/python-maths
