# Add a new package manager

To be recognized by Packager, a package manager must:
- Be an executable on the system
- Be prefixed with `packager-`
- Implement all standard commands (listed below)

To contribute a package manager to this repository, we add the following rules:
- Must be written in Bash (`#!/usr/bin/env bash`) or SH (`#!/usr/bin/env sh`)

To be consistent with each other, they should follow these conventions:
- Print all commands executed
- Always ask for confirmation before taking any changing actions
- Choices, printing, … should be implemented using the [OpenSavvy Toolkit](https://gitlab.com/opensavvy/dotfiles)

Before starting, make sure you configure the project (see the main README for the installation instructions).

[TOC]

## Creating the script

For this tutorial, we will create a package manager for Raco, the Racket library downloader.
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
	# The 'hash' command checks if 'raco' is in the PATH
	# It returns a non-zero exit code if it isn't
	hash raco 2>/dev/null || (
		# This message will be displayed if Packager is started in verbose mode.
		# It's good to be clear, so it's easy for a user to understand why a manager is marked as unavailable.
		echo "'raco' is not installed."
		# The exit code '1' means that the manager is unavailable, Packager will not list it.
		exit 1
	)
	;;
name)
	# Simply print the full name of the manager.
	echo -en "Racket dependencies"
	;;
# […]
esac
```

If you are writing a package manager which is able to install itself, you may want to always report that it is available, and automatically install it when another command is called. Example: [package-sdkman](../packager-sdkman).

### Installing packages

The main command is for installing one or more packages.
Because Raco is already able to install multiple packages at once, we just need to transmit the list.

```shell
case $1 in
# […]
install)
	# Remove 'install' from the list of arguments
	shift
	# Use 'os_info run' to print the command before executing it
	# Simply pass the full list of arguments to Raco
	os_info run "raco pkg install $*"
	# If the commands needs to be executed as root, use 'os_info sudo' instead of 'os_info run'
	;;
# […]
esac
```

### Upgrading packages

Upgrade can either take a list of packages, in which case they should be updated, or no arguments, in which case all packages should be updated.
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

"Searching" can mean different things in different situations:
- Fulltext search ("which packages have Java in their name?")
- Detailed search ("show me the detailed information of the package named `python`")
- File search ("which package should I install to get the file `nmap`?")
- Contents search ("what are the files which exist in the package `maven`?")
- …

Because package managers rarely implement all of these, it's not really possible to create a command for each.
Instead, each package manager should ask the user with a list of supported features, and let the user decide on a case-by-case basis.

Raco supports none of these searches, so we will take another example: [packager-pacman](../packager-pacman).
Making a menu happens in two commands: `os_menu` to ask and `os_menu_answer` to get the answer.

```shell
case $1 in
# […]
search)
	shift
	# Create a menu
	# The title is displayed before the options
	# Each argument is an option the user can choose
	#   The first word is the ID of the answer, the rest of the line is the description
	# Here, we mark the 'all' answer to be selected by default 
	os_menu --title "Where do you want to search?" --default all \
		"all Search for a package with this name" \
		"installed Give me information on this package I installed" \
		"file Where can I find this file?" \
		"files Show the contents of this package"
	# The variable 'os_menu_answer' contains the first word of the selected answer
	case ${os_menu_answer:?} in
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

We now have implemented all the commands. We can see the results in the [packager-raco](../packager-raco) script.

## Contributing your script to the repository

To learn how to submit your script to our repository, read [our wiki](https://gitlab.com/opensavvy/wiki/-/blob/main/README.md#learn-how-to-contribute).
