# Packager

Simple CLI to upgrade your system or install packages in a single command, no matter if you're on ArchLinux, Debian or just using Python.

Packager __always__ asks for confirmation and prints every command it executes, so you always know what is happening.

Supported backends:
- `apt` and `aptitude`: Debian-based distributions
- `pacman`: ArchLinux official repositories
- `yay`: ArchLinux user repository (AUR)
- `pip`: Python libraries
- `raco`: Racket libraries
- `sdkman`: Java/JVM libraries

Packager is made to be easy to extend: adding a backend is as simple as writing a simple shell script ([example](packager-raco)).
We welcome contributions that add support for other package managers!

[TOC]

## User guide

Packager translates package manager commands to an underlying implementation.
Each implementation is a separate script, and a [main script](packager) provides command parsing.

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

## Contributing

This project is managed on [our GitLab instance](https://gitlab.com/opensavvy/packager).

The various ways to contribute to this project are documented [in our wiki](https://gitlab.com/opensavvy/wiki).

## Licensing

This project is licensed under the GNU Affero General Public License, version 3.
The full text is available in the [LICENSE.txt](LICENSE.txt) file.
