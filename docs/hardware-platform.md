# ORCA Modular Hardware Platform

> Concept roadmap for the ORCA Servers hardware ecosystem. This page documents ideas and intended interfaces. It does not represent production-ready hardware.

ORCA is intended to grow as a modular server platform rather than a single large motherboard. Compute, I/O, power, management, storage, networking and expansion can be developed as separate boards connected by common ORCA interfaces.

## Platform idea

```text
                    ┌────────────────────┐
                    │    ORCA Control    │
                    │ Management / BMC   │
                    └─────────┬──────────┘
                              │
┌──────────────┐    ┌─────────▼──────────┐    ┌──────────────┐
│ ORCA Power   ├────►   ORCA Backplane   ◄────┤ ORCA I/O     │
└──────────────┘    └──────┬──────┬──────┘    └──────────────┘
                           │      │
                    ┌──────▼─┐  ┌─▼────────┐
                    │Compute │  │ Compute  │
                    │Module 1│  │ Module 2 │
                    └────┬───┘  └────┬─────┘
                         │           │
                    ┌────▼───────────▼─────┐
                    │ Network / Fabric     │
                    └──────────┬───────────┘
                               │
                    ┌──────────▼───────────┐
                    │ Storage / Expansion  │
                    └──────────────────────┘
```

## Core boards

### 1. ORCA Compute Module

A compact processing module containing only the hardware that needs to stay close to the processor.

Ideas:

- ARM SoC
- RAM
- PMIC and processor point-of-load regulators
- eMMC or boot flash
- clocks
- local decoupling
- temperature monitoring
- high-speed ORCA board-to-board connector
- optional local NVMe on higher-end revisions

The compute module should avoid bulky external connectors. A future compute generation should be replaceable without redesigning the entire server.

Possible progression:

`Compute C1 → Compute C2 → Compute C3 → AI-focused Compute`

Each generation can increase CPU performance, memory capacity, PCIe bandwidth and accelerator capability while retaining as much platform compatibility as practical.

### 2. ORCA I/O

Carrier-style board providing external connectivity for one or more Compute Modules.

Ideas:

- USB 3
- USB-C
- 2.5 GbE
- GPIO
- UART/debug
- M.2 expansion
- optional display output
- front-panel connection
- optional PCIe expansion

Different I/O boards could target different systems while reusing the same Compute Module.

### 3. ORCA Power

Server-level power distribution and monitoring.

Ideas:

- 12 V or 24 V system input
- reverse-polarity protection
- surge/transient protection
- input fuse/eFuse
- independently switched node outputs
- current and voltage measurement
- standby power
- fan power distribution
- temperature monitoring
- interface to ORCA Control

Sensitive SoC core and memory rails remain local to each Compute Module. ORCA Power distributes the higher-level system supply.

### 4. ORCA Control

Dedicated management board that remains available independently of the main compute nodes.

The current V3s work is a possible basis for this role.

Ideas:

- Linux-capable management processor
- Ethernet
- RTC
- watchdog
- GPIO
- PWM
- fan monitoring
- temperature monitoring
- UART access to Compute Modules
- node reset
- node power enable
- I2C management bus
- status LEDs

Long-term goal: allow software to inspect and control the physical server without relying on the main compute node being healthy.

### 5. ORCA Backplane

The common physical connection between modular boards.

Early revisions could focus on:

- power distribution
- ground
- board presence
- reset
- management I2C
- UART
- GPIO/alerts

Later revisions could add carefully designed high-speed links such as PCIe and faster networking.

The backplane should be developed after the ORCA module connector and electrical interface are defined.

### 6. ORCA Network / Fabric

Internal communication between compute nodes and the outside network.

Possible progression:

- Gen 1: 1/2.5 GbE switching
- Gen 2: 10 GbE uplinks and faster node links
- Gen 3: dedicated high-bandwidth ORCA Fabric

Ideas:

- managed Ethernet switch
- per-port status
- Control-board management
- SFP/SFP+ uplinks on suitable revisions
- node-to-node links
- telemetry

The long-term goal is to make adding another Compute Module increase useful system compute without requiring a completely separate standalone machine.

### 7. ORCA Storage

Dedicated storage modules.

#### NVMe Storage

Ideas:

- multiple M.2 NVMe drives
- PCIe switch on later revisions
- SSD temperature monitoring
- activity/fault indicators
- individual drive power control where practical
- dedicated cooling connection

The existing [ORCA PCIe NVMe Link](../README.md) is an early development step toward ORCA storage hardware.

#### HDD / Bulk Storage

Future ideas:

- multi-drive backplane
- SATA or another appropriate storage interface
- drive power distribution
- drive presence detection
- activity/fault LEDs
- hot-swap support after the required electrical and mechanical design is validated

## Expansion boards

### ORCA PCIe Expansion

For GPUs, NPUs, network cards and other accelerators.

Ideas:

- PCIe slots or high-speed board-to-board links
- clock/reset distribution
- accelerator power monitoring
- high-power connector support where required
- future PCIe switch

### ORCA Accelerator

Future dedicated AI accelerator module separate from general-purpose compute.

Potential technologies include NPUs, GPUs and FPGAs. The exact architecture is intentionally open while the core platform is developed.

### ORCA Cooling

Dedicated cooling-control PCB.

Ideas:

- 6–12 PWM fan outputs
- tachometer inputs
- temperature sensor headers
- fan-failure detection
- I2C management
- emergency fan fallback behaviour

### ORCA Front I/O

Small chassis-facing board.

Ideas:

- power button
- reset/service button
- USB
- system status LEDs
- optional display connector
- buzzer

### ORCA Debug

Development tool for bringing up other ORCA hardware.

Ideas:

- USB-C
- UART
- JTAG/SWD
- I2C
- SPI
- GPIO
- selectable I/O voltage
- test points

### ORCA Sensor

Distributed chassis monitoring.

Ideas:

- temperature
- airflow
- voltage
- fan/chassis sensors
- I2C connection to ORCA Control

## Common ORCA interface

A shared connector specification is one of the most important future pieces of the project.

Candidate signals include:

- 12 V system power
- standby power
- ground
- management I2C
- UART
- RESET#
- POWER_EN
- PRESENT#
- ALERT#
- GPIO
- PCIe lanes on suitable modules
- USB lanes on suitable modules
- Ethernet/fabric links
- reserved differential pairs for future revisions

The final connector, pinout, impedance requirements, current ratings and hot-plug behaviour must be engineered and validated before hardware depends on it.

## Development order

Suggested path:

1. ORCA Control
2. ORCA Power
3. ORCA Compute Module
4. ORCA I/O
5. Define ORCA module connector
6. ORCA Backplane
7. ORCA Network / Fabric
8. ORCA Storage
9. ORCA PCIe Expansion
10. ORCA Accelerator

## Existing hardware

### ORCA PCIe NVMe Link

Status: design/prototype stage.

Purpose: PCIe x4 to M.2 NVMe expansion and practical experience with PCIe routing, M.2, PCB edge connectors and storage hardware.

See the [main project page](../README.md).

## Repository/documentation plan

As ORCA grows, this platform page can act as the index. Individual board pages can live under `docs/boards/` and link back here.

Suggested future pages:

- `docs/boards/compute.md`
- `docs/boards/io.md`
- `docs/boards/power.md`
- `docs/boards/control.md`
- `docs/boards/backplane.md`
- `docs/boards/network-fabric.md`
- `docs/boards/storage.md`
- `docs/interfaces/module-connector.md`
- `docs/interfaces/management-bus.md`
- `docs/roadmap.md`

## Status

Everything on this page except explicitly linked existing hardware should be treated as a concept or development direction. Specifications will change as components are selected, schematics are reviewed and prototypes are tested.
