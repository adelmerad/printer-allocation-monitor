# 🖨️ Printer Allocation Monitor

![HTML](https://img.shields.io/badge/HTML-E34F26?logo=html5&logoColor=white)
![CSS](https://img.shields.io/badge/CSS-1572B6?logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)

An interactive simulator of a synchronization monitor that allocates shared printers, with priority for system processes over user processes. Watch requests block, wake up and compete for resources, step by step.

**🌐 [Live demo](https://adelmerad.github.io/printer-allocation-monitor/)**

## ✨ Features

- **Step-by-step animation:** each step of `Release` runs with a visible delay, at instant, fast, normal or slow speed
- **Animated Canvas view:** colored tokens move between the queues and the printer pool, drawn with the native Canvas 2D API
- **Configurable pool:** 1 to 20 printers, without reloading the page
- **Starvation detector:** a warning appears when the user queue goes unserved across consecutive releases, a classic scheduling problem
- **Usage gauge and statistics:** total requests, immediate allocations, unblocked processes and peak usage
- **Demo scenario** in one click, and an **auto simulation** that generates random requests and releases
- **Log export** to a `.txt` file
- **Light and dark themes**, remembered between visits
- **Accessibility:** live log region, labelled printers and Enter key support on the K and m fields

It's a single HTML file with no dependencies: open `index.html` in a browser.

## ⚙️ State

| Name | Default | Description |
|------|---------|-------------|
| N | 5 (1 to 20) | Total number of printers |
| Available | 0 to N | Printers currently free |
| System queue | FIFO | Blocked system processes and their remaining need |
| User queue | FIFO | Blocked user processes |

## 🔁 Procedures

**Request(K, type)** — a process asks for K printers.

- If `Available >= K`: immediate allocation, `Available -= K`
- If `Available < K` and the type is system: enqueue the remaining need `(K - Available)`, set `Available = 0`, block
- If `Available < K` and the type is user: enqueue K, block

**Release(m)** — a process returns m printers. Four steps, in order:

| Step | Condition | Action |
|------|-----------|--------|
| 0 | always | `Available += m` |
| 1 | system queue not empty **and** `Available >= Head` | unblock the system process at the head, repeat |
| 2 | system queue not empty **and** `Available < Head` | reserve: `Head -= Available`, `Available = 0` |
| 3 | system queue empty **and** `Available >= Head` of the user queue | unblock user processes, repeat |

**Safety guard:** `Release(m)` is refused if `m` exceeds the number of printers actually in use (`N - Available`), which guarantees the invariant `0 ≤ Available ≤ N`.

## 🧪 Demo Scenario

With N = 5, the **▶ Demo scenario** button plays:

1. Request 3 (system) → `Available` drops to 2
2. Request 3 (system) → blocked, remaining need of 1 in the queue, `Available` drops to 0
3. Request 2 (user) → blocked in the queue
4. Release 2 → step 1 unblocks the system process, leaving `Available` at 1. The user process needs 2, so it stays blocked

Release 1 more printer and step 3 unblocks the user process.

## 👤 Author

**Adel Merad** — [GitHub](https://github.com/adelmerad)
