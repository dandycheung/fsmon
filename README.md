# fsmon

![ci](https://github.com/nowsecure/fsmon/actions/workflows/ci.yml/badge.svg?branch=master)

Low level filesystem monitor utility for Linux, Android, iOS and macOS.

- **Author**: pancake @ nowsecure
- **License**: MIT

Designed for

- System administrators and incident responders
- Security researchers and forensic analysts
- Developers debugging I/O-heavy applications
- Reverse engineers interested in observing filesystem access behavior


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

## 🔍 Key Features of fsmon

`fsmon` is a low-level, cross-platform filesystem monitor designed for developers, forensic analysts, and reverse engineers. It works by hooking into the OS kernel's tracing facilities or file notification APIs.

### ✅ Supported Platforms

- **Android**: via `inotify`, `fanotify` is not always supported
- **Linux**: via `inotify` and `fanotify`
- **macOS**: using `kdebug`, `FSEvents`, `kqueue`, and `/dev/fsevents`
- **iOS** (limited support through FSEvent APIs)

### Core Capabilities

- **Real-Time File Monitoring**
  Detects and reports file operations such as creation, deletion, modification, attribute changes, and renames in real-time.

- **Multi-Backend Support**

  Automatically selects the best available monitoring backend or allows users to choose:
  - `inotify`, `fanotify` (Linux)
  - `fsevapi`, `kdebug`, `devfsev`, `kqueue` (iOS / macOS)

  The list of backends can be listed with `fsmon -L`.

- **Process-Level Insights**
  Associates file events with process names, PIDs, and UIDs, where possible.

- **Recursive Monitoring**
  Monitors entire directory trees recursively, dynamically adding new directories.

- **JSON Output Format**
  Supports structured logging in JSON or JSON stream mode for easy integration with other tools (e.g. `jq`, `ELK`, etc).

- **Filename Filtering & Formatting**
  Optionally strips full paths, shows only filenames, and colorizes output based on event type.

- **Selective Monitoring**
  Filter events by:
  - Specific process name (`-P`)
  - Specific PID (`-p`)
  - Child processes (`-c`)
  - Files under a given path

- **Backup on Event**
  Automatically copies affected files to a backup directory when changes are detected (`-b`).

- **Timestamping**
  Adds timestamps to each event to facilitate forensic analysis.

- **Minimal Dependencies**
  Written in portable C with no runtime dependencies beyond standard libraries.

- **Graceful Shutdown & Signal Handling**
  Handles `SIGINT`, `SIGTERM`, and `SIGALRM` to allow clean exits and timed monitoring sessions.

### 🧪 Event Types Tracked

Examples of events that `fsmon` can detect:

- `CREATE_FILE`, `DELETE`, `RENAME`
- `OPEN`, `CLOSE`, `STAT_CHANGED`
- `CHOWN`, `CHMOD`, `XATTR_MODIFIED`
- `CONTENT_MODIFIED`, `EXCHANGE`, `FINDER_INFO_CHANGED`

## Compilation

fsmon is a portable tool. It works on iOS, OSX, Linux and Android (x86, arm, arm64, mips)

```bash
$ make
```

Crosscompilation to iOS/Android is made easy by just running `make ios` or `make android`:

```bash
$ make android NDK_ARCH=<ARCH> ANDROID_API=<API>
```

## License

This tool is free software developed by [NowSecure](https://nowsecure.com) and distributed under the MIT license.

You can reach out **Sergi Alvarez** via email [pancake@nowsecure.com](mailto:pancake@nowsecure.com)
