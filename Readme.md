# Simulation vs. Synthesis Behavior of Common RTL Issues

A quick reference guide comparing how digital design issues behave during RTL functional simulation versus gate-level synthesis.

---

## Issue Comparison Table

| Issue | Simulation Behavior | Synthesis Behavior |
| :--- | :--- | :--- |
| **Inferred latch** | **Ignore** (Treats variable as holding its value cleanly) | **Warning** (Reports creation of unintended latch) |
| **Width mismatch** | **Ignore / Warning** (Pads/truncates automatically, optional lint warning) | **Warning** (Reports bit truncation or extension) |
| **Combinational feedback loop** | **Warning / Error / Simulation Lockup** (Zero-delay loop / oscillation) | **Error** (Violates synchronous design rules) |
| **Undriven input** | **Ignore** (Evaluates as `X` unknown state) | **Warning / Error** (Flags floating pin or ties to constant) |
| **Incomplete sensitivity list** | **Warning** (Modern simulators/lint warn about missed updates) | **Ignore** (Synthesis builds full combinatorial logic automatically) |
| **Multi-driven** | **Ignore / Contention** (Resolves conflicting drivers to `X`) | **Error** (Fatal error; physical nets cannot have multiple drivers) |
| **Glitch (combinational clock)** | **Ignore** (Propagates every nanosecond glitch/toggle) | **Warning** (Flags asynchronous/combinational clock schemes) |
