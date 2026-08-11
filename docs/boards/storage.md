# ORCA Storage

[← Hardware Platform](../hardware-platform.md)

## Purpose

ORCA Storage provides modular fast and bulk storage for the server platform.

## NVMe module ideas

- multiple M.2 M-key sockets
- PCIe connectivity
- PCIe switch on higher-end revisions
- SSD temperature sensing
- activity/fault indicators
- individual drive power control where practical
- dedicated airflow/fan connection

The existing [ORCA PCIe NVMe Link](../../README.md) is an early storage-development board and provides experience with PCIe x4, M.2 and PCB edge connections.

## Bulk-storage ideas

A later HDD/SSD backplane could explore:

- multiple drives
- SATA or another appropriate storage interface
- drive power distribution
- presence detection
- activity/fault indicators
- service-friendly connectors
- validated hot-swap support on a suitable revision

## Concept

```text
Compute / PCIe
      │
      ▼
 ORCA Storage
 ├─ NVMe 1
 ├─ NVMe 2
 ├─ NVMe 3
 └─ NVMe 4
      │
      └── temperature / status → ORCA Control
```

## Related boards

- [ORCA Compute](compute.md)
- [ORCA I/O](io.md)
- [ORCA Backplane](backplane.md)
- [ORCA Network / Fabric](network-fabric.md)
