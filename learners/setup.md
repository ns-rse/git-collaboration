---
title: Setup
---

## Setup

Whilst there are many Git clients (porcelains) out there and many Integrated Development Environments (IDEs) support Git
actions and facilitate using the bewildering array of options this course teaches the Command Line Interface (CLI) to
Git as it means we have a consistent interface to teach the _principles_ that you can take away to your own choice of
Git client. Below there are instructions for installing Git on each of the three most common operating systems as well
as a link to download s

## Data Sets

<!--
FIXME: place any data you want learners to use in `episodes/data` and then use
       a relative link ( [data zip file](data/lesson-data.zip) ) to provide a
       link to it, replacing the example.com link.
-->
Download the [data zip file](https://example.com/FIXME) and unzip it to your Desktop


::::::::::::::::::::::::::::::::::::::: discussion

### Details

A sample repository has been made available for this course as a GitHub template. To which end you will need a GitHub
account in order to use it.

:::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::: solution

Navigate to [here]() and click on **Use This Template** which will make a copy of the repository to your own GitHub
account.

:::::::::::::::::::::::::

## Software Setup

::::::::::::::::::::::::::::::::::::::: discussion

### Details

Setup for different systems can be presented in dropdown menus via a `solution`
tag. They will join to this discussion block, so you can give a general overview
of the software used in this lesson here and fill out the individual operating
systems (and potentially add more, e.g. online setup) in the solutions blocks.

:::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::: solution

### Windows

You can use the [official binary][gitWin] but we encourage you to use [Git for Windows][git4windows] which includes the Bash
shell and there are excellent
[instructions](https://carpentries.github.io/workshop-template/install_instructions/#shell) on how to install this on
the Carpentries installation instructions for the Bash Shell (**NB** the Git instructions for Windows on this page
direct the reader to the Bash Shell instructions as they are bundled together).

:::::::::::::::::::::::::

:::::::::::::::: solution

### MacOS

[Git][git] is not included in the MacOS distribution by default. The [official instructions][gitMac] suggest using
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

[git]: https://git-scm.com
[gitkraken]: https://www.gitkraken.com/
[gitMac]: https://git-scm.com/download/mac
[gitWin]: https://git-scm.com/download/win
[git4windows]: https://carpentries.github.io/workshop-template/install_instructions/#shell
