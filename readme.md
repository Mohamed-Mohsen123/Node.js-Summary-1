# Node.js
**Node.js®** is a free, open-source, cross-platform JavaScript runtime environment that lets developers create servers, web apps, command line tools, and scripts.
Node.js runs the V8 JavaScript engine, Google Chrome's core, **outside the browser**. This allows Node.js to be very performant. A Node.js app runs in a single process, without creating a new thread for every request.
Node.js provides a set of asynchronous I/O primitives in its standard library that prevent JavaScript code from blocking and generally, libraries in Node.js are written using non-blocking paradigms, making blocking behavior the exception rather than the norm.
The usual way to run a Node.js program is to run the globally available node command (once you install Node.js) and pass the name of the file you want to execute.
| Environment | JavaScript runs in... | Provides |
|---|---|---|
| Browser (e.g. Chrome) | The browser | `window`, `document`, etc. |
| Node.js | Your machine directly | Backend, APIs, CLI tools, automation |
---
## What is a Runtime?
> A **runtime** = everything needed to execute code.
Node.js includes 4 main components:
---
### 1. JavaScript Engine (V8)
- Comes from Google Chrome
- Converts JS → machine code
- Makes execution fast
---
### 2. Event Loop (Non-blocking system)
Node.js uses an **event-driven, asynchronous model**.
Instead of waiting (blocking):
```js
// ❌ Blocking (bad)
readFileSync("file.txt");
```
It does this instead (non-blocking):
```js
// ✅ Non-blocking (good)
readFile("file.txt", callback);
```
> 👉 This allows Node.js to handle many requests **at the same time**.
---
### 3. Core Modules (Built-in tools)
Node.js gives you ready-made tools:
| Module | Purpose |
|---|---|
| `fs` | File system |
| `http` | Create servers |
| `path` | File paths |
**Example — creating a simple HTTP server:**
```js
const http = require("http");
http.createServer((req, res) => {
  res.end("Hello from Node.js");
}).listen(3000);
```
---
### 4. libuv (Async magic under the hood)
Handles:
- File I/O
- Networking
- Threads
> Makes Node.js **non-blocking**.
---
## How Node.js Works (Flow)
```
Your JS code runs
      ↓
Heavy tasks go to libuv
      ↓
Event loop keeps checking
      ↓
When done → callback runs
```

---

## Module Systems: CommonJS vs ES Modules

Node.js supports two module systems. Understanding the difference helps you choose the right one for your project and avoid common errors when mixing them.

| Feature | CommonJS (CJS) | ES Modules (ESM) |
|---|---|---|
| **Syntax** | `require()` / `module.exports` | `import` / `export` |
| **Loading** | Synchronous (blocking) | Asynchronous (non-blocking) |
| **Import Placement** | Anywhere in the file (e.g. inside `if` blocks) | Must be at the **top** of the file |
| **Standardization** | Node.js default (server-side tradition) | Official JS standard (works in browser + Node.js) |

**CommonJS example:**

```js
// Exporting
module.exports = { greet };

// Importing
const { greet } = require("./greet");
```

**ES Modules example:**

```js
// Exporting
export function greet() {}

// Importing
import { greet } from "./greet.js";
```

> 💡 ES Modules are the modern standard. Use them for new projects when possible.

---

## npm (Node Package Manager)

npm is the **standard package manager for Node.js**. It is two things:

1. An **online repository** for publishing open-source Node.js projects
2. A **command-line utility** for package installation, version management, and dependency management

A plethora of Node.js libraries and applications are published on npm, and many more are added every day.

---

## Installing Packages: Local vs Global

npm allows two methods of installing packages. Choosing the right one depends on whether the package is needed for a specific project or as a system-wide tool.

| Feature | Local Install | Global Install |
|---|---|---|
| **Command** | `npm install <package>` | `npm install -g <package>` |
| **Availability** | Only inside the current project | Anywhere on the machine |
| **Stored in** | `node_modules/` folder in the project | A system-level path |
| **Tracked in** | `package.json` under `dependencies` | Not tied to any project |
| **Best used for** | Libraries your project depends on | CLI tools used across projects |
| **Example** | `express`, `lodash`, `axios` | `nodemon`, `typescript`, `npm` itself |

**Local install example:**

```bash
npm install express
```

> Installs `express` into `./node_modules` and adds it to `package.json`.

**Global install example:**

```bash
npm install -g nodemon
```

> Installs `nodemon` system-wide — usable from any terminal session, any project.

> 💡 As a rule of thumb: if you need to `import` or `require` it in your code → **local**. If you run it as a command in the terminal → **global**.

---

## Synchronous vs Asynchronous Execution

Node.js runs on a **single main thread**. How you write code — synchronous or asynchronous — determines whether that thread gets blocked or stays free to handle other work.

---

### Synchronous (Blocking)

Code executes **line-by-line**. Each line must finish before the next one starts.

```js
const fs = require('fs');

console.log('first');
const data = fs.readFileSync('/path/to/file'); // ⛔ blocks here
console.log(data);
console.log('second');

// Output: first → file content → second
```

---

### Asynchronous (Non-Blocking)

Code **offloads heavy tasks** to the background and moves on immediately.

```js
const fs = require('fs');

console.log('first');
fs.readFile('/path/to/file', (err, data) => {
  console.log(data); // runs later, once file is ready
});
console.log('second');

// Output: first → second → file content
```

---

### Comparison

| | Synchronous (Blocking) | Asynchronous (Non-Blocking) |
|---|---|---|
| **Execution** | Line-by-line, waits for each task | Starts task, moves on immediately |
| **Main thread** | ⛔ Blocked during heavy tasks | ✅ Free to handle other work |
| **Performance** | Degrades under load | Scales well with many requests |
| **Heavy tasks stacked** | Times multiply (sequential) | Run concurrently |
| **Use case** | Simple scripts, startup config | Servers, DB queries, file I/O, APIs |
| **Example** | `fs.readFileSync()` | `fs.readFile()` + callback |

---

### How Async Works Under the Hood

When Node.js hits an async operation, it doesn't wait — it delegates:

```
Your JS Code
     ↓
Async operation encountered
     ↓
Delegated to libuv
     ↓
  ┌──────────────────────────────────┐
  │  Heavy CPU tasks (file, crypto)  │ → Thread Pool
  │  Network operations              │ → OS Kernel
  └──────────────────────────────────┘
     ↓
Task completes in background
     ↓
Callback added to Event Loop queue
     ↓
Your callback runs
```

---

### Why Node.js Is Built This Way

| Traditional Servers | Node.js |
|---|---|
| New thread per request | Single main thread for all requests |
| High memory usage at scale | Low memory footprint |
| Blocking is acceptable (isolated threads) | Blocking is critical to avoid — it freezes everything |

> 💡 Because Node.js serves **all users on one thread**, a single blocking operation can freeze the entire application. Async code keeps the Event Loop free to accept thousands of simultaneous connections without ever getting stuck.

---
 
## libuv & The Event Loop
 
### What is libuv?
 
**libuv** is a multi-platform C library focused entirely on managing asynchronous I/O operations. It was originally built to power Node.js, but proved so effective that other languages like Julia use it too.
 
libuv is the internal engine that lets Node.js bypass the single-thread limitation. When Node.js hits a blocking task, it offloads the work to libuv, which handles it through two distinct paths:
 
---
 
### How libuv Handles Tasks
 
```
Node.js encounters a task
          ↓
    Is it blocking?
    ┌─────┴──────┐
   Yes            No
    ↓              ↓
Offload to      Execute on
  libuv         main thread
    ↓
  ┌───────────────────────────────────────────┐
  │                  libuv                    │
  │                                           │
  │   File I/O, CPU tasks    Network I/O      │
  │   (crypto, compression)  (HTTP, sockets)  │
  │          ↓                    ↓           │
  │     Thread Pool          OS Kernel        │
  │    (4 threads default)   (epoll / kqueue  │
  │    (max 1024 threads)     / IOCP)         │
  └───────────────────────────────────────────┘
          ↓                    ↓
     Task completes       Task completes
          ↓                    ↓
        Callback pushed to Event Loop queue
                    ↓
          Executed on main JS thread
```
 
---
 
### libuv Internal Architecture
 
```
┌─────────────────────────────────────────────────────────┐
│                         libuv                           │
│                                                         │
│  ┌──────────────────────────────┐  ┌────┐ ┌───┐ ┌────┐ │
│  │         Network I/O          │  │File│ │DNS│ │User│ │
│  │  ┌─────┬─────┬─────┬──────┐ │  │ IO │ │Ops│ │code│ │
│  │  │ TCP │ UDP │ TTY │ Pipe │ │  └────┘ └───┘ └────┘ │
│  │  └─────┴─────┴─────┴──────┘ │                       │
│  └──────────────────────────────┘                       │
│                                                         │
│  ┌─────────────────────┐  ┌──────┐  ┌───────────────┐  │
│  │      uv__io_t       │  │      │  │               │  │
│  │  epoll │ kqueue     │  │ IOCP │  │  Thread Pool  │  │
│  │  event ports        │  │      │  │  (default: 4) │  │
│  └─────────────────────┘  └──────┘  └───────────────┘  │
│   Linux/macOS (async I/O)  Windows   File/CPU tasks     │
└─────────────────────────────────────────────────────────┘
```
 
| Mechanism | OS | Used for |
|---|---|---|
| `epoll` | Linux | Network I/O |
| `kqueue` | macOS | Network I/O |
| `IOCP` | Windows | Network I/O |
| Thread Pool | All platforms | File I/O, DNS, crypto, user code |
 
---
 
### Thread Pool
 
| Property | Detail |
|---|---|
| **Default threads** | 4 |
| **Max threads** | 1,024 (via env variable) |
| **Real limit** | Physical CPU cores on your machine |
| **Used for** | File I/O, cryptography, DNS lookups, CPU-heavy tasks |
 
> ⚠️ Increasing thread count beyond your CPU core count won't improve performance — tasks just share the same physical resources. This is why Node.js is generally not recommended for extremely CPU-bound applications.
 
---
 
### The Event Loop (Basic Architecture)
 
The Event Loop is the central orchestrator living inside libuv. It connects four components:
 
```
┌─────────────┐        ┌──────────────────────┐
│             │        │   Node APIs /        │
│  Call Stack │───────>│   Background Tasks   │
│   (LIFO)    │        │   (libuv / OS)       │
│             │        └──────────┬───────────┘
└──────┬──────┘                   │
       │                          ↓
       │                 ┌──────────────────┐
       │                 │  Callback Queue  │
       │                 │     (FIFO)       │
       │                 └────────┬─────────┘
       │                          │
       └──────────────────────────┘
                    ↑
              Event Loop
         (checks if call stack
          is empty, then pushes
          next callback onto it)
```
 
---
 
### The Event Loop Phases (Advanced)
 
```
  index.js starts
       ↓
┌─────────────────────┐   ← checked before EVERY phase
│   Microtask Queue   │     process.nextTick() first
│   (highest priority)│     then settled Promises
└──────────┬──────────┘
           ↓
┌──────────────────┐
│  1. Timers       │  setTimeout() / setInterval()
└────────┬─────────┘
         ↓ (microtask queue checked again)
┌──────────────────┐
│  2. I/O Queue    │  pending I/O callbacks
└────────┬─────────┘
         ↓ (microtask queue checked again)
┌──────────────────┐
│  3. Idle/Prepare │  internal use only
└────────┬─────────┘
         ↓ (microtask queue checked again)
┌──────────────────┐
│  4. I/O Polling  │  wait for new I/O events
└────────┬─────────┘
         ↓ (microtask queue checked again)
┌──────────────────┐
│  5. Check Queue  │  setImmediate() callbacks
└────────┬─────────┘
         ↓ (microtask queue checked again)
┌──────────────────┐
│  6. Close        │  socket/stream close events
│     Callbacks    │
└────────┬─────────┘
         ↓
  Pending work left?
  ┌───────┴───────┐
 Yes              No
  ↓               ↓
Next tick     Process exits
```
 
### Event Loop Phases — Summary Table
 
| Priority | Phase | What runs |
|---|---|---|
| 🔴 Highest | **Microtask Queue** | `process.nextTick()` → then Promises |
| 1 | **Timers** | `setTimeout()`, `setInterval()` |
| 2 | **Pending Callbacks** | Deferred I/O callbacks from last iteration |
| 3 | **Idle / Prepare** | Internal only |
| 4 | **I/O Polling** | New I/O results (file reads, HTTP responses) |
| 5 | **Check** | `setImmediate()` |
| 6 | **Close Callbacks** | `socket.on('close', ...)` |
 
> 💡 The Microtask Queue is checked and fully emptied **between every phase**. This gives `process.nextTick()` and Promises higher priority than any main phase callback.
