# fsmon

![ci](https://github.com/nowsecure/fsmon/actions/workflows/ci.yml/badge.svg?branch=master)

Filesystem Monitor utility for Linux, Android, iOS and macOS.

Brought to you by *Sergi Àlvarez* at **Nowsecure** and distributed under the **MIT** license.

**Contact**: pancake@nowsecure.com

## Installation

On **macOS** you can now install it via brew with these commands:

```bash
brew tap nowsecure/fsmon https://github.com/nowsecure/fsmon
brew install nowsecure/fsmon/fsmon
```

Alternatively just run `make` or pick the builds from the [release](https://github.com/nowsecure/fsmon/releases) page.

```bash
make
make install PREFIX=/usr
```

## Usage

The tool retrieves file system events from a specific directory and shows them in colorful format or in JSON.

It is possible to filter the events happening from a specific program name or process id (PID).

```console
$ ./fsmon -h
Usage: ./fsmon-macos [-Jjc] [-a sec] [-b dir] [-B name] [-p pid] [-P proc] [path]
 -a [sec]  stop monitoring after N seconds (alarm)
 -b [dir]  backup files to DIR folder (EXPERIMENTAL)
 -B [name] specify an alternative backend
 -c        follow children of -p PID
 -f        show only filename (no path)
 -h        show this help
 -j        output in JSON format
 -J        output in JSON stream format
 -n        do not use colors
 -L        list all filemonitor backends
 -p [pid]  only show events from this pid
 -P [proc] events only from process name
 -v        show version
 [path]    only get events from this path
Examples:
 fsmon /data
 fsmon -J / | jq -r .filename
 fsmon -B fanotify /home
$
```

## Backends

fsmon filesystem information is taken from different backends depending on the operating system and apis available.

This is the list of backends that can be listed with `fsmon -L`:

* inotify (linux / android)
* fanotify (linux > 2.6.36 / android with custom kernel)
* devfsev (osx /dev/fsevents - requires root)
* kqueue (xnu - requires root)
* kdebug (bsd?, xnu - requires root)
* fsevapi (osx filesystem monitor api)

## Compilation

fsmon is a portable tool. It works on iOS, OSX, Linux and Android (x86, arm, arm64, mips)

*Linux*

```bash
$ make
```

*OSX + iOS fatbin*

```bash
$ make
```

*iOS*

```bash
$ make ios
```

*Android*

```bash
$ make android NDK_ARCH=<ARCH> ANDROID_API=<API>
```

To get fsmon installed system wide just type:

```bash
$ make install
```

Changing installation path...

```bash
$ make install PREFIX=/usr DESTDIR=/
```
