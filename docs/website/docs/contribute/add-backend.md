# Adding a new backend

This guide is a step-by-step tutorial to add support for a new package manager to Packager.

## Before starting

### Understanding the requirements

To be recognized by Packager, a package must:

- Be an executable on the system
- Be prefixed by `packager-`
- Implement all the standard commands (described in this document)

Notice that backends may be written in any language. This guide describes how to create a backend in Bash, but you may use Python or any other language if you prefer so.

??? note "Additional requirements for publishing your backend"
    If you intend to contribute your backend to the official repository, so it becomes available to all users, it must be written in Bash, and start with the following line:
    ```shell
    #!/usr/bin/env bash
    ```
    or, it may be written in pure SH and start with the following line:
    ```shell
    #!/usr/bin/env sh
    ```

### Rules

To be consistent with each other and the overall user experience, backends should follow these rules:

- Print all executed commands
- Always prompt the user for confirmation before making any changes to the system
- All interactions with the user should be implemented through the [OpenSavvy Toolkit](https://gitlab.com/opensavvy/system/dotfiles).

## Creating the script

For this tutorial, we will create a package manager for [Raco](https://docs.racket-lang.org/raco/), the Racket library downloader.
First, we create a file named `packager-raco` with the basic outline:
```shell
#!/usr/bin/env bash

# Import the OpenSavvy Toolkit
. ${os_toolkit_import:?}

# The list of all commands to be implemented (in the next sections)
case $1 in
enabled)
	;;
name)
	;;
install)
	;;
upgrade)
	;;
purge)
	;;
remove)
	;;
search)
	;;
*)
	# Fail cleanly on invalid commands
	os_info "Invalid command '$1'."
	exit 2
	;;
esac
```

Now that we have the outline, we can implement each command separately.

### Metadata

The first two commands let the main script know how the package manager is called, and whether it is currently available or not.
If it is not available, it is not displayed in the UI.

```shell
case $1 in
enabled)
	hash raco 2>/dev/null || ( #(1)!
		echo "'raco' is not installed." #(2)!
		exit 1 #(3)!
	)
	;;
name)
	# Print the full name of the manager (used in the prompt).
	echo -en "Racket dependencies"
	;;
# […]
esac
```

1. The `hash` command is standard POSIX for checking if a command is in the `PATH`.
   It returns a non-zero exit code if the command is missing.
2. This message will be displayed if Packager is started in [verbose mode](../commands/help.md#verbose-mode).
   It's good to be clear, so it's easy for a user to understand why a backend is marked as unavailable.
3. The exit code `1` means that the backend is unavailable. Packager will not list it to the user.

??? note "Package managers that are able to install themselves"
    If you are writing a package manager which is able to install itself, you may want to always report that it is available, and automatically install it when another command is called ([example](https://gitlab.com/opensavvy/system/packager/-/blob/main/src/packager-sdkman)).

### Installing packages

The main command is for [installing one or more packages](../commands/install.md).
Because Raco is already able to install multiple packages at once, we just need to transmit the list.

```shell
case $1 in
# […]
install)
	shift #(1)!
	os_info run "raco pkg install $*" #(2)!
	;;
# […]
esac
```

1. Removes `install` from the list of arguments. [Learn more](https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_09_07.html).
2. Use `os_info run` to print the command before executing it.
   In this example, Raco already knows how to deal with multiple packages at once, so we just pass the rest of the arguments.

??? note "Running a command as root"
    If the command must be executed as root, use `os_info sudo` instead of `os_info run`.

### Upgrading packages

[Upgrade](../commands/upgrade.md) can either take a list of packages, in which case they should be updated, or no arguments, in which case all packages should be updated.
These are different commands for Raco, so we need to use an `if`:
```shell
case $1 in
# […]
upgrade)
	shift
	if [[ $# == 0 ]]; then
		os_info run "raco pkg update --all"
	else
		os_info run "raco pkg update $*"
	fi
	;;
# […]
esac
```

### Removing packages

Packager makes a difference between two removals:

- "soft removal" which keeps dependencies
- "hard removal" which removes dependencies

The user is always prompted to choose between when using the [Remove](../commands/remove.md) command. 

These are two different commands:
```shell
case $1 in
# […]
purge) # remove the package, its dependencies, etc
	shift
	os_info run "raco pkg remove --auto $*"
	;;
remove) # only remove the package, keep everything else
	shift
	os_info run "raco pkg remove $*"
	;;
# […]
esac
```

### Searching packages

[Searching](../commands/search.md) can mean different things in different situations:

- Fulltext search ("which packages have Java in their name?")
- Detailed search ("show me the detailed information of the package named `python`")
- File search ("which package should I install to get the file `nmap`?")
- Contents search ("what are the files which exist in the package `maven`?")
- …

Because package managers rarely implement all of these, it's not really possible to create a command for each.
Instead, each package manager should ask the user with a list of supported features, and let the user decide on a case-by-case basis.

Raco supports none of these searches, so we will take [another example](https://gitlab.com/opensavvy/system/packager/-/blob/main/src/packager-pacman).
Making a menu happens in two commands: `os_menu` to ask and `os_menu_answer` to get the answer.

```shell
case $1 in
# […]
search)
	shift
	# Create a menu: (1) 
	os_menu --title "Where do you want to search?" --default all \
		"all Search for a package with this name" \
		"installed Give me information on this package I installed" \
		"file Where can I find this file?" \
		"files Show the contents of this package"
	case ${os_menu_answer:?} in #(2)!
	all)
		os_info run "pacman -Ss $*"
		;;
	installed)
		os_info run "pacman -Qi $*"
		;;
	file)
		os_info run "pacman -F $*"
		;;
	files)
		os_info run "pacman -Ql $*"
		;;
	esac
	;;
# […]
esac
```

1. Creates a menu.
   The title is displayed before the options.
   Each argument is an option the user can choose: its first word is its ID, the rest of the line is its description.
   We also mark the answer identified by `all` as the default.
2. The variable `os_menu_answer` is automatically created by the `os_menu` command, and contains the first word of the line selected by the user.

## Conclusion

We now have implemented all the commands. We can see the results in the [packager-raco](https://gitlab.com/opensavvy/system/packager/-/blob/main/src/packager-raco) script.

To ensure the script can run, don't forget to make it executable: 
```shell 
chmod u+x path/to/the/script
```

You have successfully implemented support for a new backend into Packager. If you'd like to support the Packager project, please contribute it to our repository, so we can share it with our users! To do so, please visit [our wiki](https://gitlab.com/opensavvy/wiki/-/blob/main/README.md#learn-how-to-contribute).
