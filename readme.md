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
