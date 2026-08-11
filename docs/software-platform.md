# ORCA Software Platform

[← Hardware Platform](hardware-platform.md)

> Concept roadmap for software across the ORCA Servers modular boards. Companion document to [hardware-platform.md](hardware-platform.md). Nothing described here has been implemented yet — this defines the intended architecture and build sequence, to be revised as SoC/MCU choices are made and boards are prototyped.

## Guiding principle

Mirror the hardware modularity: each board owns the software closest to its own hardware, a small set of shared low-level protocols ties boards together, and a single management plane (ORCA Control) is the one place "whole system" state lives. Compute Modules run workloads; they should not need to know about power sequencing, fan curves, or fleet management — that belongs to Control.

## Software layers

```text
┌─────────────────────────────────────────┐
│      ORCA management UI / API           │  user-facing
├─────────────────────────────────────────┤
│      ORCA Control services              │  orchestration / policy
│  (power, reset, fans, sensors, telemetry)│
├─────────────────────────────────────────┤
│   I2C mgmt bus · UART · mgmt Ethernet    │  inter-board protocols
├─────────────────────────────────────────┤
│  Per-board firmware / OS                 │  Compute, Power, Network...
└─────────────────────────────────────────┘
```

## Per-board software

### ORCA Compute Module

- Bootloader: U-Boot
- OS: embedded Linux, built with Buildroot for early bring-up (Yocto only if a later revision needs shared BSP layers across multiple SoCs)
- Device tree covering the SoC plus whatever peripherals sit behind the ORCA connector
- Minimal rootfs with console/SSH access for bring-up
- A small health agent exposing temperature/voltage/status to Control over UART or I2C
- Later: containerized workloads once bring-up and the connector interface are stable

### ORCA Control (management role)

This is the board that has to stay up independently of compute health, so it needs a small, boot-fast, reliable image — not the same build as a workload OS.

- OS: start with a minimal Buildroot Linux image and hand-written services rather than adopting a full BMC stack (e.g. OpenBMC) up front — revisit if the feature set grows enough to justify that complexity
- Core services:
  - power/reset controller — drives `POWER_EN` / `RESET#` per node
  - fan controller — PWM out, tach in, thermal policy
  - sensor poller — I2C temperature/voltage
  - UART console proxy / presence tracker per Compute Module
  - watchdog supervisor
  - RTC
  - telemetry aggregator — folds in Storage and Network/Fabric data
- Local API: a simple HTTP/JSON (or gRPC) endpoint over the management Ethernet exposing all of the above. This becomes the backend for the management UI, and later a candidate for a Redfish-style interface so standard tooling can talk to it

### ORCA Power

No general-purpose OS. If a microcontroller handles current/voltage sensing or eFuse switching, it runs small firmware exposing I2C readback registers — no local policy. All decisions (when to cut a rail, alarms, etc.) live in Control, not on the Power board itself.

### ORCA Backplane

No software — passive interconnect. Presence-detection and alert lines are read by Control.

### ORCA Network / Fabric

- Gen 1: an off-the-shelf managed switch, configured through its own vendor firmware/CLI. The only ORCA-specific piece is a small poller in Control reading per-port stats (SNMP or the switch's management API) into telemetry.
- Gen 2/3: once a custom fabric exists, this becomes real embedded software (link/topology management) — out of scope until that hardware exists.

### ORCA Storage (NVMe)

The current NVMe Link board has no onboard processor. SMART/temperature data is read by whichever board owns the PCIe root complex (a Compute Module) via `libnvme`/`nvme-cli`, and forwarded to Control's telemetry aggregator. A future PCIe-switch revision would need its own switch-config firmware.

### ORCA Debug

Not board firmware — a bring-up tool. Needs OpenOCD (JTAG/SWD) and a serial terminal (minicom/picocom) on the *developer's* machine, not on any ORCA board.

## Cross-board protocols

| Link | Carries |
| --- | --- |
| Management I2C | sensors, presence, low-rate control (Control is bus master) |
| UART | per-node console/debug; a simple text protocol for reset/health before a full API exists |
| Management Ethernet | Control ↔ management UI, Control ↔ switch API, Control ↔ Compute health agent (once networking is up) |
| GPIO discretes | `RESET#`, `POWER_EN`, `ALERT#`, `PRESENT#` — plain level signals, no protocol |

## ORCA management UI

- Talks only to Control's API — never touches hardware directly
- v1: read-only — node health, temperatures, fan speeds, power draw
- v2: actions — reset/power-cycle a node, fan-curve override
- v3: fleet view, once more than one Control board/chassis exists
- Suggested stack: a lightweight backend co-located with Control's services, and a simple frontend (static HTML/JS against the JSON API is enough to start; no framework commitment needed yet)

## Toolchain / dev environment

- Cross compiler: `aarch64-linux-gnu-*` or `arm-linux-gnueabihf-*` depending on eventual SoC choice (not yet selected)
- Build system: Buildroot for bring-up; reassess Yocto only if BSP sharing across boards becomes necessary
- Bootloader: U-Boot, with a board port added per Compute revision
- Debug: OpenOCD plus a JTAG/SWD adapter, minicom/picocom for UART consoles
- Version control: keep software in its own repository once real code exists (e.g. `orca-software`), cross-linked from here, so this hardware repo stays hardware-only

## Sequencing

Mirrors the hardware development order in [hardware-platform.md](hardware-platform.md):

1. **S1 — Bring-up**: U-Boot + minimal Linux boots on Compute C1 over UART; bench power only, no Control board yet
2. **S2 — Control MVP**: Control runs power/reset, fan, and sensor services; reachable over a simple CLI/`curl`-able API; no UI yet
3. **S3 — Management UI v1**: read-only dashboard on top of Control's API
4. **S4 — Storage + telemetry**: NVMe health data flows into Control; UI shows per-drive status
5. **S5 — Actions + fleet**: UI can reset/power-cycle nodes; multi-Control-board fleet view
6. **S6 — Network/Fabric integration**: switch telemetry folded in; Gen 2 fabric software once that hardware exists

## Open decisions

- SoC/MCU selection for Compute and Control (drives cross-compiler target and Buildroot vs Yocto call)
- Whether Control adopts a subset of an existing BMC project (OpenBMC) later instead of the hand-rolled service set above
- API shape for the Control ↔ UI boundary (REST/JSON vs gRPC vs Redfish-compatible)

## Status

Concept-stage plan only — no firmware, OS images, or UI code exist yet. Sequencing and technology choices above will change as SoC selection, Compute/Control prototypes, and the ORCA module connector spec are finalized.

## Related

- [ORCA Modular Hardware Platform](hardware-platform.md)
- [ORCA Control](boards/control.md)
- [ORCA Compute](boards/compute.md)
- [ORCA Storage](boards/storage.md)
- [ORCA Network / Fabric](boards/network-fabric.md)
