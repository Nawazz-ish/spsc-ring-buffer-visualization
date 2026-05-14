# SPSC Ring Buffer Visualization

An interactive, animated visualization of a Single-Producer Single-Consumer (SPSC) lock-free ring buffer.

Demonstrates:
- The producer/consumer pointer dance (`head` chases `tail`)
- The **cached pointer optimization** — why `cached_tail_` and `cached_head_` exist
- The **double-check pattern** for detecting full/empty without paying for an atomic load on the fast path
- Power-of-2 capacity with bitmask wrap-around

## Run it

Open [`index.html`](index.html) in any modern browser.

## Controls

- **try_push()** — step the producer forward one operation
- **try_pop()** — step the consumer forward one operation
- **Auto run** — play a scripted sequence
- **Reset** — clear state

Watch the log at the bottom for narrated step-by-step output, including when the cache lets us skip the expensive atomic load (⚡ "cache hit") versus when we have to refresh it.
