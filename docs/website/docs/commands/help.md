# Commands

Packager commands take the shape:
```shell
packager <command> [packages…]
```

For example, to install a package called `foo`:
```shell
packager install foo #(1)!
```

1.  To learn more about this command, see [Install](install.md).

Multiple packages can be installed at once:
```shell
packager install foo bar
```

## Selecting a specific backend

By default, Packager attempts to load all backends and asks the user to select one of the enabled ones. If you already know which backend to use, you may specify it directly:
```shell
packager --backend <backend-name> <command> [packages…]
```

??? example
    If you want to upgrade your Debian system with the [Aptitude backend](../backends/debian.md):
    ```shell
    packager --backend packager-aptitude upgrade
    ```

## Verbose mode

Verbose mode can be enabled by specifying `-v` at the very start of the command, before the command and any other options:
```shell
packager -v upgrade #(1)!
```

1.  To learn more about this command, see [Upgrade](upgrade.md).

This may be helpful to understand why Packager considers a backend to be unavailable.

## Help

Finally, to get the information from this page directly in your CLI, print the Help:
```shell
packager help
```

The `help` command does not support the other options described in this page.
