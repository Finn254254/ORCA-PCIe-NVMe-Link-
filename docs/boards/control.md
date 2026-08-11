# ORCA Control

[← Hardware Platform](../hardware-platform.md)

## Purpose

ORCA Control is the dedicated management board for the physical server. It should remain capable of monitoring and controlling the machine independently of the main compute workload.

The current V3s-based design work is a possible foundation for this role.

## Ideas

- Linux-capable management processor
- Ethernet
- RTC
- watchdog
- GPIO
- PWM fan control
- fan tachometer monitoring
- temperature sensors
- UART to Compute Modules
- node RESET#
- node POWER_EN
- I2C management
- status LEDs

## Concept

```text
             ORCA Control
        ┌────────┼────────┐
        │        │        │
      Power    Compute   Cooling
        │        │        │
      switch    UART     PWM/Tach
      monitor   reset    sensors
```

## Long-term software idea

The ORCA management UI could use this board to show node health, temperatures, fan speeds and power data and to perform actions such as resetting or power-cycling a failed Compute Module.

## Related boards

- [ORCA Power](power.md)
- [ORCA Compute](compute.md)
- [ORCA Backplane](backplane.md)
