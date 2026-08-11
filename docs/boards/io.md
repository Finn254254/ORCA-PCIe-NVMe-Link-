# ORCA I/O

[← Hardware Platform](../hardware-platform.md)

## Purpose

ORCA I/O provides the external connectors and expansion interfaces that do not need to live on the Compute Module.

## Ideas

- USB 3
- USB-C
- 2.5 GbE
- GPIO
- UART/debug
- M.2 expansion
- front-panel connection
- optional display output
- optional PCIe expansion

## Why separate I/O?

Keeping I/O separate lets ORCA reuse a Compute Module in different systems. A compact server, storage node and development system could use different I/O boards around the same compute architecture.

## Concept

```text
ORCA Compute
     │
     ▼
 ORCA I/O
 ├─ USB
 ├─ Ethernet
 ├─ M.2
 ├─ GPIO
 ├─ Debug
 └─ Front Panel
```

## Related boards

- [ORCA Compute](compute.md)
- [ORCA Backplane](backplane.md)
- [ORCA Network / Fabric](network-fabric.md)
- [ORCA Storage](storage.md)
