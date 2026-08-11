# ORCA Backplane

[← Hardware Platform](../hardware-platform.md)

## Purpose

ORCA Backplane is the common physical backbone that lets modular ORCA boards operate as one server.

## Early-revision ideas

- system power
- ground
- standby power
- board presence detection
- RESET#
- POWER_EN
- ALERT#
- management I2C
- UART
- low-speed GPIO

## Later-revision ideas

After the connector and signal-integrity requirements are proven, later backplanes could add:

- PCIe
- high-speed Ethernet/fabric links
- additional differential pairs
- redundant management links

## Concept

```text
┌─────────┐ ┌─────────┐ ┌─────────┐
│Compute 1│ │Compute 2│ │ Storage │
└────┬────┘ └────┬────┘ └────┬────┘
     │           │           │
═════╪═══════════╪═══════════╪═════
             ORCA Backplane
═════╪═══════════╪═══════════╪═════
     │           │           │
┌────▼────┐ ┌────▼────┐ ┌────▼────┐
│ Control │ │  Power  │ │ Fabric  │
└─────────┘ └─────────┘ └─────────┘
```

## Important prerequisite

The ORCA module connector specification needs to be defined before a serious backplane design. Pinout, current capability, return paths, impedance, mating cycles, mechanical alignment and any hot-plug behaviour require validation.

## Related boards

- [ORCA Compute](compute.md)
- [ORCA Power](power.md)
- [ORCA Control](control.md)
- [ORCA Network / Fabric](network-fabric.md)
- [ORCA Storage](storage.md)
