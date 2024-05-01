# Install from sources

If Packager is not distributed for your operating system, you can download and install it from sources using `git`.

!!! note
    If you want to add a distribution for your operating system, [please get in touch](https://gitlab.com/opensavvy/system/packager/-/issues/new). 
    However, please understand that this requires quite a bit of automation, so it is much more likely to succeed if you have created packages for your operating system already and are able to teach us how to do it.

You will need the following dependencies installed:

- Bash
- Git
- The [OpenSavvy Toolkit](https://gitlab.com/opensavvy/system/dotfiles)

Once you have followed the instructions to install the OpenSavvy Toolkit, clone the Packager repository:
```shell
git clone https://gitlab.com/opensavvy/packager.git
```

Finally, add the repository's `src` directory to your `$PATH`. 

For example, if you are using Bash:
```shell title="~/.bashrc"
export PATH="$PATH:replace_this_by_where_you_cloned_the_project/src"
```

To ensure everything is working, you can run:
```shell
packager help
```
to get the list of commands and overall syntax.
