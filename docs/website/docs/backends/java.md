# JVM

Packager supports [SDKMAN!](https://sdkman.io/), which allows [installing JDKs from various vendors](https://sdkman.io/jdks) as well as a [various CLI tools](https://sdkman.io/sdks).

??? note "Why don't I get the option to use this backend?"
    If this backend does not appear in the choices, run the following command to understand why:
    ```shell
    packager-sdkman enabled
    ```
    If the command is not found, check your installation.
    Otherwise, it should print the reason why it doesn't appear.
