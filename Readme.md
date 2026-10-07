# HDL Design Issues: Synthesis vs. Simulation Reference

A quick-reference guide for hardware design engineers detailing how common RTL coding issues, design flaws, and hazards behave across **Synthesis** and **Simulation** toolchains.

## Summary Table

| Issue | Synthesis Side | Simulation Side | Primary Classification |
| :--- | :--- | :--- | :--- |
| **Inferred Latch** | `Warning` | `Ignored` | Synthesis Warning / Design Flaw |
| **Width Mismatch** | `Error` | `Error` | Compilation / Synthesis Error |
| **Combinational Feedback** | `Error` | `Error` | Synthesis & Simulation Error |
| **Undriven Input** | `Warning` | `Warning` | Simulation Warning / Design Flaw |
| **Incomplete Sensitivity List** | `Ignored` | `Error` | Simulation / Functional Bug |
| **Multi-Driven Issues** | `Error` | `Error` | Synthesis & Simulation Error |
| **Glitch / Combinational Clock** | `Error` | `Error` | Timing / Design Hazard |

---

## Detailed Breakdown

### 1. Inferred Latch
* **Synthesis (`Warning`):** Synthesis tools automatically infer a transparent latch when combinatorial conditions are incomplete (missing `else` branches or default assignments). Modern tools issue a warning because latches cause asynchronous timing challenges.
* **Simulation (`Ignored`):** Standard simulators treat the code as completely legal behavior based on the inferred logic, which frequently results in pre- and post-synthesis functional mismatches.

### 2. Width Mismatch
* **Synthesis (`Error`):** Stricter language standards (like VHDL) treat mismatched vector assignments as hard compilation errors. Some Verilog/SystemVerilog tools may allow implicit casting with warnings, but strict lint flows reject them.
* **Simulation (`Error`):** Compilers and elaborators flag size mismatches (such as assigning a 4-bit value to an 8-bit wire) as errors or strict warnings to prevent unexpected truncation or zero-extension bugs.

### 3. Combinational Feedback
* **Synthesis (`Error`):** Triggers a critical design rule check (DRC) violation or hard error because it creates an illegal combinational loop, violating synchronous design rules.
* **Simulation (`Error`):** Causes zero-delay infinite loops, signal oscillations, or simulation hangs (resulting in simulator timeouts or fatal runtime errors).

### 4. Undriven Input
* **Synthesis (`Warning`):** The tool issues a warning that a net or input port is undriven, often automatically tying it to ground (`0`), VCC (`1`), or leaving it floating depending on attributes.
* **Simulation (`Warning`):** Elaboration and lint tools warn about floating wires, which evaluate to `X` (unknown) in Verilog and corrupt downstream logic.

### 5. Incomplete Sensitivity List
* **Synthesis (`Ignored`):** Modern synthesis tools analyze the logic blocks and automatically infer all read signals for combinatorial logic, completely bypassing the sensitivity list.
* **Simulation (`Error`):** Simulators strictly follow the sensitivity list. Omitting signals means the block won't re-evaluate when those signals change, causing functional simulation failures and mismatches.

### 6. Multi-Driven Issues
* **Synthesis (`Error`):** Multiple active drivers on a standard net are rejected as illegal wiring because physical hardware cannot resolve conflicting drivers on a single wire.
* **Simulation (`Error`):** Results in driver contention where conflicting logic values fight, propagating `X` states or triggering simulator contention errors.

### 7. Glitch / Combinational Clock
* **Synthesis (`Error`):** Driving a clock with combinational logic (ripple clocks / gated clocks) violates synchronous design practices. Synthesis tools reject it with errors/critical warnings due to severe clock skew and timing closure failure.
* **Simulation (`Error`):** Exposes severe timing hazards, race conditions, and glitches during gate-level timing simulations that violate setup/hold requirements and trigger explicit failure flags.
