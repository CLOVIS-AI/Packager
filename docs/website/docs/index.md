---
template: home.html
---

# Welcome!

Do you often switch operating systems, package managers, and language ecosystems? Are you tired of having to relearn new commands each time when trying to do the same operations?

Packager is a simple CLI to upgrade your system or install packages in a single command, no matter if you're on ArchLinux, Debian, using Python, or any other supported backend.

Packager **always** asks for confirmation and prints every command it executes, so you always know what is happening.

To install a package, simply type:
```bash
packager install foo
```
and let the guide show you around!

## Simplicity

Just remember these commands:

- [`packager install`](commands/install.md)
- [`packager upgrade`](commands/upgrade.md)
- [`packager search`](commands/search.md)
- [`packager remove`](commands/remove.md)

and simply pass one or more packages as argument.

Packager will automatically detect which of the supported backends are available on your system, ask you which one you want to use, and translate your request into the correct command for it.

## Supported backends

- [Archlinux](backends/archlinux.md): Pacman and Yay
- [Debian](backends/debian.md): Aptitude and APT
- [JVM development](backends/java.md): SDKMAN!
- [JavaScript development](backends/js.md): NPM and Yarn
- [Racket development](backends/racket.md): Raco

and the best part is: if you want to use something not listed here, [it's simple to add it yourself](contribute/add-backend.md)!
