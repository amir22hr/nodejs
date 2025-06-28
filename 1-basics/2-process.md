# **Process**
> Node.js `process` Object

## Properties

| Property | Description |
| --- | --- |
| [`process.argv`](#processargv) | Array of command-line arguments |
| [`process.env`]() | Object containing environment variables |
| [`process.pid`]() | Process ID (PID) of the current process |
| [`process.ppid`]() | Parent Process ID (PPID) |
| [`process.title`]() | Process title (changeable) |
| [`process.arch`]() | CPU architecture (`arm`, `x64`, `ia32`, etc...) |
| [`process.platform`]() | OS platform (`linux`, `win32`, `darwin`, etc.) |
| [`process.version`]() | Node.js version |
| [`process.versions`]() | Object with versions of `node`, `v8`, `zlib`, etc. |
| [`process.execPath`]() | Absolute path to the Node.js executable |
| [`process.execArgv`]() | Node-specific CLI flags (e.g., --inspect) |
| [`process.cwd()`]() | Current working directory |
| [`process.chdir()`]() | Changes the working directory |
| [`process.uptime()`]() | Process uptime (seconds) |
| [`process.hrtime()`]() | High-resolution time (nanoseconds precision) |

### process.argv
###
###
###
###
###
###
###
###
###

---

## Memory & Resource Usage

| Property/Method | Description |
| --- | --- |
| [`process.memoryUsage()`](#processmemoryusage) | Returns memory usage (`heapUsed`, `rss`, etc.) |
| [`process.resourceUsage()`](#processresourceusage) | CPU, memory, and other resource stats |

### process.memoryUsage()
### process.resourceUsage()

---

## I/O Streams

| Property | Description |
| --- | --- |
| [`process.stdin`](#processstdin) | Readable stream for standard input |
| [`process.stdout`](#processstdout) | Writable stream for standard output |
| [`process.stderr`](#processstderr) | Writable stream for standard error |

### process.stdin 
### process.stdout
### process.stderr

---

## Methods (Process Control)

| Method | Description |
| --- | --- |
| [`process.exit([code])`]() | Terminates the process (default exit code: `0`) |
| [`process.kill(pid, [signal])`]() | Sends a signal to another process (e.g., SIGTERM) |
| [`process.abort()`]() | Forcefully terminates the process (generates a core dump) |
| [`process.nextTick(callback)`]() | Schedules `callback` to run in the next event loop iteration |

### 
### 
###
###

---

## Events (Process LifeCycle) => proccess.on(event, callback)

| Property/Method | Description |
| --- | --- |
| [`exit`]() | Emitted when the process is about to exit (cannot be stopped) |
| [`beforeExit`]() | Emitted when the event loop is empty (can be delayed) |
| [``]() | ss |
| [``]() | ss |
