---
title: "Continuous Integration"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions

- How can I get a computer to automate tasks?
- How do I shorten the feedback loop when developing code?
- How can I use GitHub Actions/GitLab CI?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Use and configure Pre-commit.ci
- Use Continuous Integration to have computers run checks and tests automatically.
- Building Websites with Actions
- Running actions locally

::::::::::::::::::::::::::::::::::::::::::::::::

## Continuous What?

We've seen how to run hooks locally and automatically in relation to events in the Git cycle, but it is also possible,
and indeed desirable, to run hooks automatically on GitHub in response to different events. This is known as _Continuous
Integration_ or _Continuous Delivery_ depending on the actions taken and their effects.

Examples of actions that might be undertaken in this manner include...

- Running the test suite for a package.
- Building and deploying a website.
- Building the software package and deploying it to the package repository (e.g. PyPI or CRAN).
- Uploading archives of work to a Figshare repository such as [ORDA][orda]
- Running pre-commit checks online.

The list of options is vast and there are whole ecosystems for the different Forge's as we shall discover.

::::::::::::::::::::::::::::::::::::: callout

This course focuses on [GitHub][gh] but there are similar systems available for other Forges. All use [YAML][yaml]
configuration files to configure the system and very similar syntax.

## GitLab

[GitLab][gl] has an equivalent system named [CI/CD][gl-cicd].

## ForgeJo

[Forgejo][fg] has a system very similar to GitHub using [action][fg-actions]

::::::::::::::::::::::::::::::::::::::::::::::::

## GitHub Actions

GitHub makes available a series of Virtual Machines in the cloud to undertake the tasks that you wish to perform via
[GitHub Actions][gh-actions]. These define the events which will run, the conditions under which they will run and can
call other actions that have been developed and shared by others on their repositories or even in the [Actions
Marketplace][gh-actions-market].

### Configuration

Configuration of actions that run in GitHub is via [YAML][yaml] files that reside in `.github/workflows/`.

::::::::::::::::::::::::::::::::::::: callout

Quite why the directory isn't `.github/actions/` is a mystery as it would align better.

The terms terms "workflow" and "action" may be used interactively when teaching the material but every effort has been
made in the written material to use the term `action` when unless specifically referring to the directory.

::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Make sure to ask the class who is familiar with YAML so you can gauge the level of experience in the room. If no one has
come across it before take things a little slower and explain how the structure works.

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

Lets take a look at the structure of the existing actions define in the `.github/workflow` directory of the
[python-maths][python-maths] repository we have been working with.

``` bash
ls -lha .github/workflows/
drwxr-xr-x neil neil 4.0 KB Sat Feb 17 07:28:30 2024  .
drwxr-xr-x neil neil 4.0 KB Sat Feb 17 07:28:30 2024  ..
.rw-r--r-- neil neil 639 B  Sat Feb 17 07:28:30 2024    test-python-package.yaml
```

There is just one file in this directory `test-python-package.yaml`, we can use `cat
.github/workflows/test-python-package.yaml` to concatenate the file and see its contents.

``` yaml
name: Python package

on:
  push:
    branches: main
  pull_request:
    branches: main

jobs:
  tests:
    name: Tests (${{ matrix.os }}, ${{ matrix.python-version }})
    runs-on: ${{ matrix.os }}
    strategy:
      fail-fast: false
      matrix:
        os: ["ubuntu-latest", "macos-latest", "windows-latest"]
        python-version: [3.10, 3.11, 3.12]

    steps:
      - uses: actions/checkout@v4
      - name: Set up Python ${{ matrix.python-version }}
        uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          python -m pip install .[dev,tests]
      - name: Test with pytest
        run: |
          pytest

```

::::::::::::::::::::::::::::::::::::: callout

[YAML][yaml] (which stands for _YAML Ain't Markup Language_) is a common format for defining hierarchical data
structures. It is a super-set of JSON (JavaScript Object Notation) that many find more flexible (in part because of the
ability to have comments).

::::::::::::::::::::::::::::::::::::::::::::::::

#### Fields

The syntax defined in the Workflow

- `name` : defines the name of the Action.
- `on` : defines the events the Action will be run on, here you can see that it will be run on both `push` events and
  `pull_request` which occur on the `main` branch.
- `jobs` : this defines the jobs that are undertaken and is a bit more complex.
  - `tests` : this is the name of the job that is subsequently defined, here it is `tests` because the section defines
    running the tests.
    - `name` : The name for the test, this is a combination of the subsequent `matrix.os` and `matrix.python-version`
    - `runs-on` : defines the operating system/virtual machine that will be used to run the job, here it is set to
      `ubuntu-latest` and will use the most recent Ubuntu image available.
    - `strategy` : defines how the job will run, it has two sub-settings.
      - `fail-fast` : Currently set to `false`, but if `true` then any step failing cancels all other jobs in the
        defined matrix.
      - `matrix` : This is a neat way of defining more than one operating system, and in this case Python version on
        which to run the tests under. These combine to increase the number of virtual machines that are spun up and the
        tests are run under.
        - `os` : defines the operating system on which to run the tests, there are many available, including older
          versions of each.
        - `python-version` : defines which Python versions to run the tests under.
    - `steps` : Defines the different steps that will be run on each virtual machine.
      - `uses` : This first instance uses the [`actions/checkout@v4`](https://github.com/actions/checkout) which is an
        action provided by GitHub that checks out the repository the workflow belongs to. You will want to include this
        as the first step in almost all of your actions.
      - `name` : A description of the next step, in this case _Set up Python_
      - `uses` : Runs the [`actions/setup-python@v5`](https://github.com/actions/setup-python) which will install
        Python, which version is defined under the `with` that follows.
      - `with` : Defines what version of the following items to use.
        - `python-version` : Uses one of the Python versions defined above under `matrix.python-version`
      - `name` : The next step is to _Install dependencies_
      - `run` : This step defines shell commands that are run, by virtue of the vertical bar (`|`), each command you
        wish to run should be on its own line. These next two lines upgrade `pip` the programme that installs Python
        packages and then installs the cloned package from the current directory, along with all dependencies, including
        those required for `dev` and `tests`.
      - `name` : The last step is to _Test with pytest_ and runs the tests.
      - `run` : Another short shell command that runs the tests by invoking `pytest` which will have been installed as
        one of the dependencies in the previous step.

#### Actions...in Action

Earlier in the course you will have made Pull Requests and merged changes into the `main` branch. These will have
triggered actions and we can now go and look at the log-files from running those actions.

In the GitHub repository of `python-maths` that you are collaborating on navigate to the _Actions_ tab and you should
see a list of actions listed.

### MarketPlace

## Pre-commit.ci

::::::::::::::::::::::::::::::::::::: keypoints

- Continuous Integration/Delivery is a useful method of checking code _before_ it enters the `main` branch.
- GitHub uses Actions that are defined by YAML configuration files under `.github/workflow/`.
- Actions can be restricted to events/branches/tags.
-

::::::::::::::::::::::::::::::::::::::::::::::::

[fg]: https://forgejo.org
[fg-actions]: https://forgejo.org/docs/next/user/actions/
[gh]: https://github.com
[gh-actions]: https://docs.github.com/en/actions
[gh-actions-market]: https://github.com/marketplace?type=actions
[gl]: https://gitlab.com
[gl-cicd]: https://docs.gitlab.com/ee/ci/
[orda]: https://orda.shef.ac.uk/
[python-maths]: https://github.com/ns-rse/python-maths
[yaml]: https://yaml.org

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Instructor Template
::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 1: Can you do it?

What is the output of this command?

```r
paste("This", "new", "lesson", "looks", "good")
```

:::::::::::::::::::::::: solution

## GitHub

GitHub uses `~/.github/workflows/*.yaml` to define different Actions to run/

:::::::::::::::::::::::::::::::::

:::::::::::::::::::::::: solution

## GitLab

GitLab uses `.gitlab-cy.yaml` for configuring Continuous Integration

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Runners

::::::::::::::::::::::::::::::::::::: callout

Callout sections can highlight information.

::::::::::::::::::::::::::::::::::::::::::::::::
