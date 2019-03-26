# Install MSYS2 in Windows

This tutorial will explain how to install MSYS2 on windows to have a basic environment for developing.

More info [here](www.github.com/msys2/msys2/wiki/MSYS2-installation).

## Download MSYS2

* [32 bits](www.repo.msys2.org/distrib/i686/)
* [64 bits](www.repo.msys2.org/distrib/x86_64/) *(32 bits support come inside it)*

## Update packages

Launch msys2 terminal with `msys2.exe`. Run `pacman -Syuu`. Follow the instructions. Repeat this step until it says there are no packages to update.

Pacman is the package manager tools.

* `pacman -S <package-name>` : install new page
* `pacman -Ss <partial-package-name>`: Search for package name
* `pacman -R <package-name>`: Remove package/

More info about package [here](www.github.com/msys2/msys2/wiki/Using-packages).

## Basic setups

### Install git

```bash
pacman -S git
git config --global user.name "Olivier Ldff"
git config --global user.email olivier.ldff@gmail.com
```

### Shortcut in Context Menu for MSYS2 MINGW32/64 shell

Very usefull feature is too be able to have MSYS2 bash in context menu to open it from Windows file explorer.

```bash
mkdir ~/dev
cd ~/dev
git clone https://github.com/njzhangyifei/msys2-mingw-shortcut-menus.git
cd msys2-mingw-shortcut-menus
./install
```

### MSYS2-packages

Every package are inside a famous [github repository](www.github.com/Alexpux/MSYS2-packages).

```bash
cd ~/dev
pacman -S --needed base-devel msys2-devel
git clone www.github.com/Alexpux/MSYS2-packages
cd MSYS2-packages
```

Then depending on which package you want to build

```
cd ${package-name}
makepkg
```

To install the built package(s).

```
pacman -U ${package-name}*.pkg.tar.xz
```

### Basic usefull packages

#### Install Development Tools (GNU GCC Compiler and others)

- [autoconf](https://www.archlinux.org/packages/core/any/autoconf/): A GNU tool for automatically configuring source code
- [automake](https://www.archlinux.org/packages/core/any/automake/): A GNU tool for automatically creating Makefiles
- [binutils](https://www.archlinux.org/packages/core/x86_64/binutils/): A set of programs to assemble and manipulate binary and object files
- [bison](https://www.archlinux.org/packages/core/x86_64/bison/): The GNU general-purpose parser generator
- [fakeroot](https://www.archlinux.org/packages/core/x86_64/fakeroot/): Tool for simulating superuser privileges
- [file](https://www.archlinux.org/packages/core/x86_64/file/): File type identification utility
- [findutils](https://www.archlinux.org/packages/core/x86_64/findutils/): GNU utilities to locate files
- [flex](https://www.archlinux.org/packages/core/x86_64/flex/): A tool for generating text-scanning programs
- [gawk](https://www.archlinux.org/packages/core/x86_64/gawk/): GNU version of awk
- [gcc](https://www.archlinux.org/packages/core/x86_64/gcc/): The GNU Compiler Collection - C and C++ frontends
- [gettext](https://www.archlinux.org/packages/core/x86_64/gettext/): GNU internationalization library
- [grep](https://www.archlinux.org/packages/core/x86_64/grep/): A string search utility
- [groff](https://www.archlinux.org/packages/core/x86_64/groff/): GNU troff text-formatting system
- [gzip](https://www.archlinux.org/packages/core/x86_64/gzip/): GNU compression utility
- [libtool](https://www.archlinux.org/packages/core/x86_64/libtool/): A generic library support script
- [m4](https://www.archlinux.org/packages/core/x86_64/m4/): The GNU macro processor
- [make](https://www.archlinux.org/packages/core/x86_64/make/): GNU make utility to maintain groups of programs
- [pacman](https://www.archlinux.org/packages/core/x86_64/pacman/): A library-based package manager with dependency support
- [patch](https://www.archlinux.org/packages/core/x86_64/patch/): A utility to apply patch files to original sources
- [pkgconf](https://www.archlinux.org/packages/core/x86_64/pkgconf/): Package compiler and linker metadata toolkit
- [sed](https://www.archlinux.org/packages/core/x86_64/sed/): GNU stream editor
- [texinfo](https://www.archlinux.org/packages/core/x86_64/texinfo/): GNU documentation system for on-line information and printed output
- [util-linux](https://www.archlinux.org/packages/core/x86_64/util-linux/): Miscellaneous system utilities for Linux
- [which](https://www.archlinux.org/packages/core/x86_64/which/): A utility to show the full path of commands

```
pacman -S base-devel msys2-devel cmake
```

#### MinGW 32bits

From MinGW32 Shell

```
pacman -Syuu
pacman -S unzip bzip2 base-devel mingw-w64-x86_64-toolchain mingw-w64-x86_64-libusb mingw-w64-x86_64-libusb-compat-git mingw-w64-x86_64-hidapi mingw-w64-x86_64-libftdi
```

#### MinGW 64bits

From MinGW64 Shell

```
pacman -Syuu
pacman -S unzip bzip2 base-devel mingw-w64-i686-toolchain mingw-w64-i686-libusb mingw-w64-i686-libusb-compat-git mingw-w64-i686-hidapi mingw-w64-i686-libftdi
```

## Sources

* [MSYS2 Website](www.msys2.org/): *complete installation tutorial*
* [MSYS2 Github Wiki](www.github.com/msys2/msys2/wiki/Using-packages): *all the documentation*
* [MSYS2 Packages](www.github.com/Alexpux/MSYS2-packages)