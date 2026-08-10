# ORCA PCIe NVMe Link

ORCA PCIe NVMe Link is a custom PCIe expansion board designed by Finn and branded ORCA Servers. It is intended to add M.2 NVMe storage through a PCIe x4 interface as part of the modular ORCA hardware platform.

![Front render of the ORCA PCIe NVMe Link V3.0](images/pcb-render-front.png)

## Features

- PCIe x4 host interface
- M.2 NVMe storage support
- Compact, modular storage expansion for the ORCA hardware platform
- Custom hardware design intended for further development and testing

## Project status

This project is in development. The design has not yet been represented as production-ready, and specifications, layout, and documentation may change.

## Repository contents

- `README.md` — project overview, current status, manufacturing warning, and rights notice
- `images/` — PCB renders, layout views, and a schematic overview
- `hardware/` — KiCad schematic and PCB source files, plus the BOM CSV

Additional design, fabrication, and supporting files may be added as the project develops.

## Design gallery

### PCB renders

| Front | Back |
| --- | --- |
| ![Front PCB render](images/pcb-render-front.png) | ![Back PCB render](images/pcb-render-back.png) |

### PCB layout

| Top | Bottom |
| --- | --- |
| ![Top PCB layout in red](images/pcb-layout-copper.png) | ![Bottom PCB layout in blue](images/pcb-layout-routed.png) |

### Schematic overview

![PCIe x4 to M.2 NVMe schematic overview](images/schematic-overview.png)

## Bill of Materials

The full machine-readable BOM is available at [`hardware/BOM.csv`](hardware/BOM.csv).

| Reference | Qty | Value / part | Footprint |
| --- | ---: | --- | --- |
| C1 | 1 | 10uF | 0805 SMD capacitor |
| C2 | 1 | 100nF | 0402 SMD capacitor |
| C3 | 1 | 22uF | 0805 SMD capacitor |
| D1 | 1 | LED | 0603 SMD LED |
| F1 | 1 | 3A fuse | 1206 SMD fuse |
| J1 | 1 | PCIe x4 edge connector | PCIe x4 PCB edge |
| J2 | 1 | M.2 M-key socket | TE 1-2199119-5 |
| R1 | 1 | 1K | 0603 SMD resistor |

## Hardware source files

- [`ORCA PCIe NVMe Link V3.0.kicad_sch`](hardware/ORCA%20PCIe%20NVMe%20Link%20V3.0.kicad_sch) — KiCad schematic source
- [`ORCA PCIe NVMe Link V3.0.kicad_pcb`](hardware/ORCA%20PCIe%20NVMe%20Link%20V3.0.kicad_pcb) — KiCad PCB layout source

## Manufacturing warning

Do not manufacture, assemble, sell, or rely on this hardware without an independent engineering review and appropriate electrical, signal-integrity, thermal, safety, and compatibility testing. Prototype hardware may contain errors and could damage connected equipment or storage devices. Manufacturing is also restricted by the rights notice below.

## Rights

This design was personally created by Finn and is branded ORCA Servers.

Copyright © 2026 Finn, trading as ORCA Servers. All rights reserved. No permission is granted to copy, modify, redistribute, manufacture, sell, or create derivative works from this design without prior written permission.
