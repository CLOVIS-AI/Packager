# Packager

Simple CLI to upgrade your system or install packages in a single command, no matter if you're on ArchLinux, Debian or just using Python.

Packager __always__ asks for confirmation and prints every command it executes, so you always know what is happening.

![](docs/packager-install.png "Packager install screenshot")

Supported:
- `apt` and `aptitude`: Debian-based distributions
- `pacman`: ArchLinux official repositories
- `yay`: ArchLinux user repository (AUR)
- `pip`: Python libraries
- `raco`: Racket libraries
- `sdkman`: Java/JVM libraries
- `yarn`: JavaScript libraries
- `npm`: Node.js libraries

Packager is made to be easy to extend: adding a backend is as simple as writing a small shell script ([tutorial](docs/add-package-manager.md)).
We welcome contributions that add support for other package managers!

[TOC]

## User guide

Packager translates package manager commands to an underlying implementation.
Each implementation is a separate script, and a [main script](src/packager) provides command parsing.

All the following commands allow specifying multiple packages by separating them with spaces.

**Install a package.**
```shell
packager install foo
```

**Upgrade all packages.**
```shell
packager upgrade
```

**Upgrade specific packages.** May or may not be implemented for all backends. May or may not upgrade the entire system as well.
```shell
packager upgrade foo
```

**Remove packages.**
```shell
packager remove foo  # keep dependencies and configuration
packager purge foo   # remove dependencies and configuration
```

**Search for information.**
Depending on the backend, this may search for a package name, find in which package a file is available, or display information about a locally-installed package.
```shell
packager search foo
```

**Skip the package manager choice.**
By default, Packager prompts the user to select one the available backends. Instead, it can be explicitly mentioned with any command.
```shell
packager --tool packager-pacman install foo
```

## Installation guide

Click on your preferred installation method to expand it:

<details>
<summary>Manual installation</summary>

Before installing Packager, ensure you have installed its dependencies:
- Bash
- the [OpenSavvy Toolkit](https://gitlab.com/opensavvy/dotfiles)

Once this is done, you can clone this repository:
```shell
git clone https://gitlab.com/opensavvy/packager.git
```
Finally, add it the project to your `$PATH`. For example, when using Bash, add the following line to your `~/.bashrc`:
```shell
export PATH="$PATH:replace_this_by_where_you_cloned_the_project/src"
```

</details>

If you wish to add support for another installation method, please check for issues labelled with ~deployment (or create a new one).

## Licensing

This project is licensed under the GNU Affero General Public License, version 3.
The full text is available in the [LICENSE.txt](LICENSE.txt) file.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).
- To learn more about our coding conventions and workflow, see the [OpenSavvy Wiki](https://gitlab.com/opensavvy/wiki/-/blob/main/README.md#wiki).
- This project is based on the [OpenSavvy Playground](docs/playground/README.md), a collection of preconfigured project templates.
