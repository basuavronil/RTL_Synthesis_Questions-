# RTL Issues: Synthesis vs Simulation Behavior

| Issue | Synthesis | Simulation |
|---|---|---|
| Inferred latch | Warning | Ignored |
| Width mismatch | Warning | Warning |
| Combinational feedback loop | Warning | Ignored |
| Undriven input | Warning | Ignored |
| Incomplete sensitivity list | Warning | Ignored |
| Multi-driven | Error | Ignored |
| Glitch (combinational clock) | Warning | Ignored |

> **Note:** Behavior varies by tool and settings (Vivado, Design Compiler, Questa, VCS, etc.), so treat this as typical behavior.
