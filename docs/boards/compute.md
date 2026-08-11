# ORCA Compute Module

[← Hardware Platform](../hardware-platform.md)

## Purpose

ORCA Compute is the replaceable processing module for the ORCA server platform. It should contain the processor, memory and processor-specific power hardware while leaving bulky external I/O to other boards.

## Candidate architecture

```text
System Power
    │
    ▼
┌───────────────┐
│ Local PMIC /  │
│ Regulators    │
└───────┬───────┘
        │
┌───────▼───────────────┐
│         SoC           │
│   CPU / GPU / NPU     │
└───┬──────┬───────┬────┘
    │      │       │
   RAM   eMMC    ORCA connector
                   │
             I/O / Fabric /
             Backplane
```

## On-board ideas

- SoC
- RAM
- PMIC
- CPU/GPU/NPU point-of-load regulators
- DDR regulators
- eMMC or boot flash
- clocks
- decoupling
- temperature sensing
- high-speed board-to-board connector
- debug/test points

## Keep off the module where practical

- RJ45
- full-size USB connectors
- front-panel buttons
- large system power connectors
- most chassis fan connectors

## Compute roadmap

C1 should prioritize successful bring-up and useful Linux compute. Later revisions can increase RAM, CPU/NPU performance and high-speed connectivity.

The goal is that a future Compute revision can replace an older one while the surrounding ORCA power, management, storage and chassis architecture remains useful.

## Related boards

- [ORCA I/O](io.md)
- [ORCA Power](power.md)
- [ORCA Control](control.md)
- [ORCA Backplane](backplane.md)
- [ORCA Network / Fabric](network-fabric.md)
