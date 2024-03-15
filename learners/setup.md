---
title: Setup
---

## Setup

There are many Git clients (porcelains) out there and many Integrated Development Environments (IDEs) support Git
actions and facilitate using the bewildering array of options, but this course teaches the Command Line Interface (CLI)
to Git as it means we have a consistent interface to teach the _principles_ that you can take away to your own choice of
Git client. Below there are instructions for installing Git on each of the three most common operating systems as well
as links to download the Git tool GitKraken which includes Git and a Terminal.

Please complete these setup tasks _before_ attending the course. If you have any issues getting setup please either
contact an instructor in advance or arrive early and seek assistance from an instructor.

## Example Repository

This course uses a GitHub Template that you will, in small groups, create a copy of and collaborate on.

## Software Setup

::::::::::::::::::::::::::::::::::::::: discussion

As this is a course about using [Git][git] you will need to have it installed on your computer. If you're already using
Git the chances are you already have it installed or it may be integrated into your Integrated Development Environment
(IDE).

For consistency across operating systems this course uses the Command Line Interface (CLI) to Git and instructions below
will guide you through installation on different Operating Systems.

An alternative option is provided using the [GitKraken][gitkraken] client as many of those attending the course may
already be using this software, but the Command Line Interface (Terminal) of that client will be used.

:::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::: solution

### Windows

You can use the [official binary][gitWin] but we encourage you to use [Git for Windows][git4windows] which includes the
Bash shell and there are excellent
[instructions](https://carpentries.github.io/workshop-template/install_instructions/#shell) on how to install this on
the Carpentries installation instructions for the Bash Shell (**NB** the Git instructions for Windows on this page
direct the reader to the Bash Shell instructions as they are bundled together).

:::::::::::::::::::::::::

:::::::::::::::: solution

### MacOS

[Git][git] is not included in the MacOS distribution by default. The [official instructions][gitMac] suggests using
either [Homebrew](https://brew.sh/), [MacPorts](https://www.macports.org/) or
[Xcode](https://developer.apple.com/xcode/).

#### Homebrew

``` bash
brew install git
```

#### MacPorts

``` bash
sudo port install git
```

:::::::::::::::::::::::::

:::::::::::::::: solution

### Linux

Most GNU/Linux distributions will have Git installed by default. If not you can install it using the package manager for
your distribution. This will vary between distributions but some common ones are shown below. They assume you have
`root` (administrator) access to your system via `sudo`. If `sudo` is not configured but you have `root` access then
`su` and remove the `sudo` prefix from the following commands.

#### Arch

``` bash
pacman -Syu git
```

#### Debian/Ubuntu

``` bash
apt-get install git
```

#### Fedora

``` bash
dnf install git
```

#### Gentoo

``` bash
emerge -av dev-vcs/git
```

:::::::::::::::::::::::::

:::::::::::::::: solution

### GitKraken

[GitKraken][gitkraken] is a GUI porcelain for using Git, but it comes bundled with its own version of [Git][git]. Whilst
it is not open source it uses a Freemium model and the free version contains all of the functionality required for this
course. It is available across all three of the mentioned platforms and once installed you can start a Terminal by
clicking on the [Terminal button](https://help.gitkraken.com/gitkraken-client/terminal/) in the toolbar.

:::::::::::::::::::::::::

## GitHub

::::::::::::::::::::::::::::::::::::::: discussion

You will also need an account on [GitHub][gh]. If you do not already have one please
[register](https://github.com/signup), if you have an academic email address such as `@<institute>.ac.uk` or
`@<institute.edu` then registering with this address will give you access to a few more features.

You should generate an SSH key and add the public component to your GitHub account.

:::::::::::::::::::::::::::::::::::::::::::::::::::

**TODO** Link to instructions on generating keys under different OSs.

:::::::::::::::: solution

### Windows

:::::::::::::::::::::::::

:::::::::::::::: solution

### OSX

:::::::::::::::::::::::::

:::::::::::::::: solution

### Linux

:::::::::::::::::::::::::

## Conda Environments

::::::::::::::::::::::::::::::::::::::: discussion

In order to teach this course we need some code that is version controlled and some tasks to complete. [Python][python]
has been chosen as the language to fulfill that task. You do _not_ need to know Python in order to complete the course,
the code you need to use is all provided and can be copy and pasted.

However, you _do_ need what is known as a "_Virtual Environment_" setup to be able to install various programmes and run
the checks that are part of this course. To that end you should install [Miniconda3][miniconda3] on your system prior to
attending the course.

If you are already familiar with Python and Virtual Environments you can simply create a fresh virtual environment to
use for the course.

:::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::: solution

### Installing Miniconda

Please follow the instructions at [Installing Miniconda](https://docs.anaconda.com/free/miniconda/miniconda-install/)
for your Operating System.

:::::::::::::::::::::::::

:::::::::::::::: solution

### Creating A Virtual Environment

You will have to create a virtual environment to undertake the course. If you have installed Miniconda as described
above you open a terminal (Windows use the Git Bash Shell) and create a Virtual Environment called `git-collaboation`,
activate it and install the Python Package Manager programme `pip` using the following commands.

``` bash
conda create --name git-collaboration python=3.11
conda activate git-collaboration
conda install pip
```

:::::::::::::::::::::::::

[gh]: https://github.com
[git]: https://git-scm.com/
[gitkraken]: https://www.gitkraken.com/
[gitMac]: https://git-scm.com/download/mac
[gitWin]: https://git-scm.com/download/win
[git4windows]: https://carpentries.github.io/workshop-template/install_instructions/#shell
[miniconda3]: https://docs.anaconda.com/free/miniconda/
[python]: https://python.org
