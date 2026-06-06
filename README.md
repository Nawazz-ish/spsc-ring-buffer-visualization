# Lock-Free Queue Visualization

An interactive, animated visualization of bounded lock-free queues across all four
producer/consumer cardinalities, with the source code highlighted line-by-line as
each operation steps.

## Modes

Switch between them with the tabs at the top. Each mode swaps in its own algorithm,
thread panels, code snippet, and buffer annotations.

| Mode | Producers | Consumers | Technique |
|------|-----------|-----------|-----------|
| **SPSC** | 1 | 1 | Lock-free, no CAS. Each side owns one index; **cached pointers** skip atomic loads on the fast path. |
| **MPSC** | N | 1 | Producers **CAS `head`** to reserve a slot, then publish it via a per-slot `ready` flag. The lone consumer reads `tail` freely. |
| **SPMC** | 1 | N | The lone producer writes `head` freely; consumers **CAS `tail`** to claim a slot. |
| **MPMC** | N | N | Dmitry Vyukov's bounded queue: every cell carries a **sequence number** that gates access; both sides CAS their position. |

## Code highlighting

The right-hand panel shows the `try_push` / `try_pop` source for the current mode.
As an operation animates, the active line lights up and the log narrates each step —
CAS attempts, retries, cache hits (⚡), full/empty failures, and publishes.

## Run it

Open [`index.html`](index.html) in any modern browser. No build step.

## Controls

- **`try_push()` / `try_pop()`** per thread (e.g. `P1`, `P2`, `C1`, `C2`) — step one operation.
- **⚡ Race** (multi-producer / multi-consumer modes) — runs two threads *concurrently*
  so you can watch one CAS fail and retry under real contention.
- **Auto run** — play a scripted push/pop sequence.
- **Reset** — clear state for the current mode.

## Reading the buffer

- **Green ↓ `head` / `enq`** — where the producer writes.
- **Blue ↓ `tail` / `deq`** — where the consumer reads.
- **Amber `c_tail` / `c_head`** (SPSC only) — cached local copies of the other side's index.
- **`s=` badge** (MPMC) — the cell's current sequence number.
- **● dot** (MPSC / SPMC) — slot has been published (`ready`).
