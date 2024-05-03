# ArchLinux

## Official repositories

Packager supports delegating to `pacman` to install packages from the official repositories.

??? note "Why don't I get the option to use this backend?"
    If this backend does not appear in the choices, run the following command to understand why:
    ```shell
    packager-pacman enabled
    ```
    If the command is not found, check your installation. 
    Otherwise, it should print the reason why it doesn't appear.

## The ArchLinux User Repository (AUR)

Packager supports delegating to `yay` to install packages from the [AUR](https://wiki.archlinux.org/title/Arch_User_Repository).

??? note "Why don't I get the option to use this backend?"
    If this backend does not appear in the choices, run the following command to understand why:
    ```shell
    packager-yay enabled
    ```
    If the command is not found, check your installation.
    Otherwise, it should print the reason why it doesn't appear.
