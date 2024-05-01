# Search

> ```shell
> packager search foo
> ```
> Searches for more information about `foo`.

***

#### Subcommands

The `search` command offers multiple subcommands, which are not supported by all backends. If they are supported, the user is prompted to choose between one of them.

##### All

Searches an online database to find packages named similarly to the specified package.

##### Installed

Prints detailed information about the specified package.

##### File

When using this command, the argument given in the command line is interpreted as being a file name.
The command lists packages that contain this file name.

For example, if you want to install the command `ifconfig`, but do not know in which package it is bundled, run:
```shell
packager search ifconfig
```
and select the `file` subcommand.

##### Files

Prints all files contained in the specified package.
