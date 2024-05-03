# Debian

The Debian ecosystem has multiple package managers that fit different goals and are compatible with each other.
For a comparison between them, see [this Reddit thread](https://www.reddit.com/r/debian/comments/9whj01/comment/e9q2bd2).

## Aptitude

Aptitude is a package manager for Debian with smart conflict-resolution capabilities. We recommend using it if possible.

??? note "Why don't I get the option to use this backend?"
    If this backend does not appear in the choices, run the following command to understand why:
    ```shell
    packager-aptitude enabled
    ```
    If the command is not found, check your installation.
    Otherwise, it should print the reason why it doesn't appear.

## APT

APT is the original package manager for Debian.

??? note "Why don't I get the option to use this backend?"
    If this backend does not appear in the choices, run the following command to understand why:
    ```shell
    packager-apt enabled
    ```
    If the command is not found, check your installation.
    Otherwise, it should print the reason why it doesn't appear.
