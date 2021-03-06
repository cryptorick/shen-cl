[![Shen Version](https://img.shields.io/badge/shen-20.1-blue.svg)](https://github.com/Shen-Language)
[![Build Status](https://travis-ci.org/Shen-Language/shen-cl.svg?branch=master)](https://travis-ci.org/Shen-Language/shen-cl)

# Shen for Common Lisp

[Shen](http://www.shenlanguage.org) for Common Lisp by [Mark Tarver](http://marktarver.com/), with contributions by the [Shen Language Open Source Community](https://github.com/Shen-Language).

This codebase currently supports the following implementations:

  * [GNU CLisp](http://www.clisp.org/)
  * [Clozure Common Lisp](http://ccl.clozure.com/)
  * [Embeddable Common Lisp](https://common-lisp.net/project/ecl/)
  * [Steel Bank Common Lisp](http://www.sbcl.org/)

This port acts as the standard implementation of the Shen language. It is also the fastest known port, running the standard test suite in 4-8 seconds on SBCL, depending on hardware.

Bug reports, fixes and enhancements are welcome. If you intend to port Shen to another variety of Common Lisp, consider doing so as a pull request to this repo.

## Prerequisites

You will need to have recent versions of the Common Lisp implementations you want to work with installed and available as the `Makefile` requires. Installation is different depending on operating system.

### Linux

CLisp, ECL and SBCL are available through `apt`. Just run `sudo apt install clisp ecl sbcl`.

ECL requires `libffi-dev` to build, which can also be retrieved through `apt`.

There is a [separately available debian package](http://mr.gy/blog/clozure-cl-deb.html) for Clozure. Download and install with `dpkg -i`.

If the version of SBCL available throught `apt` is too old, a sufficiently new version is [available from debian](http://http.us.debian.org/debian/pool/main/s/sbcl/sbcl_1.3.14-2+b1_amd64.deb).

### macOS

CLisp, Clozure, ECL and SBCL can be acquired through Homebrew with `brew install clisp clozure-cl ecl sbcl`.

### Windows

CLisp has an installer and a zip package on [SoureForge](https://sourceforge.net/projects/clisp/files/clisp/2.49/). You'll have to include `clisp.exe` as well as `libintl-8.dll` and `libreadline6.dll` in on your PATH to ensure the clisp build of shen-cl will run.

Clozure will need to be installed manually:
  * Download the zip from [here](https://ccl.clozure.com/download.html) and extract it under `Program Files`.
  * Add the Clozure directory to your `PATH` or add a script named `ccl.cmd` to somewhere in your `PATH` containing something like:

```batch
@echo off
"C:\Program Files\ccl\wx86cl64.exe" %*
```

ECL needs to be [built from source](https://common-lisp.net/project/ecl/static/files/release/). Refer to the [appveyor.yml](https://gitlab.com/embeddable-common-lisp/ecl/blob/develop/appveyor.yml) config for build procedure. Requires [Visual Studio 2015+](https://www.visualstudio.com/downloads/) tools. ECL support is spotty on Windows and is not included in `make all` when on Windows.

SBCL has an msi package on its [download page](http://www.sbcl.org/platform-table.html).

The `Makefile` might not be entirely Windows-friendly, so a toolset like [GOW](https://github.com/bmatzelle/gow) can fill the gap, or use [MGWIN](http://www.mingw.org/) or [Cygwin](https://www.cygwin.com/).

## Building

The `Makefile` automates all build and test operations.

| Target    | Operation                             |
|:----------|:--------------------------------------|
| `fetch`   | Download and extract Shen sources.    |
| `build-X` | Build executable.                     |
| `test-X`  | Run test suite.                       |
| `X`       | Build and run test suite.             |
| `run-X`   | Start Shen REPL.                      |
| `release` | Creates archive of compiled binaries. |

`X` can be `clisp`, `ccl`, `ecl`, `sbcl` or it can be `all`, which will run the command for all of the preceding.

## Running

An executable is generated for each platform in its platform-specific output directory under `bin/` (e.g. `bin/sbcl/shen.exe`). Per typical naming conventions, it is named `shen.exe` on Windows systems and just `shen` on Unix-based systems.

Startup scripts can be specified on the command line by preceding them with a `-l` flag. If any startup scripts are specified this way, they will be loaded in order and then `(exit 0)` will be called. If none are, the Shen REPL will start as usual. Either way, all command line arguments will be accessible with `(command-line)`.

When starting Shen via `make`, command line arguments can be passed through like this: `make run-sbcl Args="-l bootstrap.shen -flag"`.

## Releases

Archives of pre-built binaries are created using the `make release` command. They will appear under `release/`, named with the operating system and current git tag or short commit hash.

Currently, only the license file and the SBCL build are included (named `shen[.exe]`).

Each tagged release on the project downloads page should have a set of pre-built archives. Be sure to archive the build of that specific commit, like this:

```shell
make pure
git checkout v1.2.3
make fetch
make sbcl
make release
```
