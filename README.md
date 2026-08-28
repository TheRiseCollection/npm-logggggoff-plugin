# logggggoff

[![npm version](https://img.shields.io/npm/v/logggggoff.svg)](https://www.npmjs.com/package/logggggoff)
[![npm downloads per week](https://img.shields.io/npm/dw/logggggoff.svg)](https://www.npmjs.com/package/logggggoff)

A colorful, cross-platform CLI tool designed to help you **list**, **understand**, and **log off** running processes, grouped into helpful categories.

Part of the **processLogger** tooling from [THE RISE COLLECTION](https://www.therisecollection.co/portfolio/processlogger).

## Features
- **Categorized process list**: Quickly see running processes grouped as Browser, Editor/IDE, Office, System, and Other.
- **Readable descriptions**: Common apps include a short description so you know what each process does at a glance.
- **Single-process shutdown**: Terminate a specific process by PID directly from the CLI.
- **Graceful shutdown**: Uses `SIGTERM` on Unix-like systems for a more graceful stop when possible.
- **Cross-platform**: Works on **Windows**, **macOS**, and **Linux**.

## Installation

```bash
npm install -g logggggoff
```

## Usage

After installing globally, use the `logggggoff` command in your terminal.

### List running processes

This is the default behavior when no subcommand is provided:

```bash
logggggoff
```

Or explicitly:

```bash
logggggoff list
```

You will see a categorized, color-coded list of running processes:

```text
Logggggoff Process List

Running Processes:
1. chrome (PID: 1234) - Google Chrome web browser. [Browser]
2. code (PID: 5678) - Visual Studio Code editor. [Editor/IDE]
...
```

### Kill a specific process by PID

To terminate a single process by its PID:

```bash
logggggoff 525 run
```

This will attempt to gracefully stop the process with PID `525` and print a success or error message.

> **Note**: Use this carefully. Always double-check the PID before terminating a process.

## Development

```
npm install
node index.js --help
node index.js list          # enumerate running processes
npm link                    # put `logggggoff` on your PATH while iterating
```

Test on more than one OS before publishing. Process enumeration and termination are
the least portable things a CLI can do, and `ps-list` smooths over less than it looks
like it does.

## Decisions of record

* **`child_process` is a Node builtin and must never appear in `dependencies`.** It
  was listed as `"child_process": "latest"` through v1.0.4. The code always resolved
  the builtin, so the entry bought nothing — but it made every install fetch whatever
  the registry serves under that name, unpinned. That name is currently an npm-owned
  security holder whose description says npm will "probably give it to you if you
  want it." Removed 2026-08-28; the published 1.0.4 still carries it until someone
  republishes.

* **Killing a process is explicit and by PID.** The tool lists first and terminates
  only what you name. Pattern-matched or bulk termination would make a single typo
  expensive in a way this tool's convenience does not justify.

* **Read-only by default.** `list` is the entry point people reach for; nothing
  destructive happens without a second, specific command.
