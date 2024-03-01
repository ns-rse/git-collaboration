# Git Collaboration

This lesson uses [The Carpentries Workbench][workbench] to develop training material for an intermediary level course on
using Git effectively for collaboration.

## Course Description

[Git][git] is a powerful tool for distributed, collaborative and version controlled "software" development. As with any
powerful tool the amount of different options and ways of using it are many resulting in an often bewildering or
overwhelming experience for those trying to learn how to use it.

This course seeks to develop material that focuses on some of the more advanced topics and tasks involved with using Git
and many of the tasks that make the process of _collaborating_ using [Git][git] easier to understand.

+ Consolidate concepts of branches and working with them.
+ Introduce some simple practices for Git hygeine that keep the repository, issues and pull requests tidy and easier to
  understand.
+ The power and uses of Git Hooks.
+ Leveraging Continuous Integration in work flows.
+ Effective Reviewing
+ Project Management tools

## Contributions

Contributions are welcome please refer to the [contribution guidelines](CONTRIBUTING.md) of how to do so and ensure you
adhere to the [Code of Conduct](CODE_OF_CONDUCT.md).

### Building Locally

To render these pages locally you need to have [R][r] installed. Instructions are
[available](https://carpentries.github.io/workbench/#installation) but some additional steps have been taken to make
sure the environment is reproducible.

Once you have installed the dependencies you can render the pages locally by starting R in the project root and
running...

``` r
sandpaper::serve()
```

This will build the pages and start a local web-server in R and open it in your default browser. These pages are "live"
and as you edit and save them the web-site will be rebuilt and the pages updated.

[git]: https://git-scm.com
[r]: https://www.r-project.org/
[workbench]: https://carpentries.github.io/workbench/
