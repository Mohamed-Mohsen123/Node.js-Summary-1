# Node.js

**Node.js®** is a free, open-source, cross-platform JavaScript runtime environment that lets developers create servers, web apps, command line tools, and scripts.

Node.js is a runtime environment that lets you run JavaScript **outside the browser**, mainly on your computer or server.

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
