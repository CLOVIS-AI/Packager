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

Packager is made to be easy to extend: adding a backend is as simple as writing a small shell script.
We welcome contributions that add support for other package managers!

**To learn more, please visit [the documentation website](https://opensavvy.gitlab.io/system/packager/docs)!**

## Licensing

This project is licensed under the GNU Affero General Public License, version 3.
The full text is available in the [LICENSE.txt](LICENSE.txt) file.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).
- To learn more about our coding conventions and workflow, see the [OpenSavvy website](https://opensavvy.dev/open-source/index.html).
- This project is based on the [OpenSavvy Playground](docs/playground/README.md), a collection of preconfigured project templates.

If you don't want to clone this project on your machine, it is also available using [DevContainer](https://containers.dev/) (open in [VS Code](https://code.visualstudio.com/docs/devcontainers/containers) • [IntelliJ & JetBrains IDEs](https://www.jetbrains.com/help/idea/connect-to-devcontainer.html)). Don't hesitate to create issues if you have problems getting the project up and running.
