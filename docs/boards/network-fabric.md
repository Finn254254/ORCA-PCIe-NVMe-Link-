# ORCA Network / Fabric

[← Hardware Platform](../hardware-platform.md)

## Purpose

ORCA Network / Fabric connects Compute Modules to each other and to external networks.

## Progression idea

### Gen 1

- 1/2.5 GbE
- simple managed switching
- per-port status
- management connection

### Gen 2

- 10 GbE uplink
- faster node links
- SFP+ where appropriate
- improved telemetry

### Gen 3

- high-bandwidth ORCA Fabric focused on distributed compute
- topology designed around multiple Compute and Accelerator modules

## Concept

```text
Compute 1 ─┐
Compute 2 ─┼── ORCA Fabric ── External Network
Compute 3 ─┤        │
Storage  ──┘        └── Accelerator
```

## Development goals

- make node-to-node communication predictable
- allow Compute Modules to be added without each becoming an isolated machine
- provide management telemetry to ORCA Control
- increase bandwidth as compute capability increases

## Related boards

- [ORCA Compute](compute.md)
- [ORCA Backplane](backplane.md)
- [ORCA Storage](storage.md)
