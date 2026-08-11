# ORCA Power

[← Hardware Platform](../hardware-platform.md)

## Purpose

ORCA Power handles server-level input protection, distribution, switching and monitoring. Processor-specific low-voltage rails stay on the Compute Module.

## Concept

```text
12/24 V Input
     │
 Protection
     │
 Monitoring
     │
 ┌───┴──────────────┐
 │                  │
Compute 1         Compute 2
 │                  │
Local PMIC        Local PMIC
 │                  │
SoC/RAM           SoC/RAM
```

## Ideas

- 12 V or 24 V input
- fuse/eFuse
- reverse-polarity protection
- transient protection
- current measurement
- voltage measurement
- independently switched node outputs
- standby supply
- fan power
- temperature sensing
- ORCA Control interface

## Design principle

Do not use the Power board as a remote PMIC for sensitive CPU or DDR rails. Each Compute Module should generate the processor-specific rails close to its SoC and memory.

## Related boards

- [ORCA Compute](compute.md)
- [ORCA Control](control.md)
- [ORCA Backplane](backplane.md)
