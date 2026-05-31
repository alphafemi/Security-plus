# Power Management & Backup Power

## Overview

Power is the single most fundamental dependency in any IT infrastructure. Without electricity, nothing runs — servers, networking equipment, storage, cooling, and security systems all fail simultaneously. Designing for power resilience means planning for outages at multiple timescales: seconds, minutes, hours, and days.

---

## Power Disruption Types

| Event | Description |
|-------|-------------|
| **Blackout** | Complete loss of power from the grid |
| **Brownout** | Voltage drop below normal operating levels — can damage equipment or cause instability |
| **Surge** | Excessive voltage spike — can damage or destroy equipment |
| **Sag** | Brief, momentary voltage drop |

A power protection strategy must address all of these scenarios, not just complete outages.

---

## Uninterruptible Power Supply (UPS)

A **UPS** contains an internal battery (or other energy storage) that provides power when the main supply fails. It bridges the gap between a power event and either restoration of grid power or generator startup.

### UPS Types

| Type | How It Works | Best For |
|------|-------------|---------|
| **Offline / Standby** | Runs on main power; internal switch activates battery when power fails | Basic protection; budget-conscious deployments |
| **Line-interactive** | Adjusts output voltage during brownouts without switching to battery; switches to battery during blackouts | Areas prone to brownouts and voltage fluctuation |
| **Online / Double-conversion** | Always runs from battery — battery is constantly charged from main power; complete isolation from power events | Critical systems requiring zero transfer time and full power conditioning |

### UPS Capabilities and Features

| Feature | Description |
|---------|-------------|
| **Battery capacity** | Determines how long the UPS can power connected equipment |
| **Graceful shutdown signaling** | As battery depletes, UPS signals systems to shut down cleanly before power is lost entirely |
| **Outlet count** | Number of devices that can be powered from the UPS |
| **Surge suppression** | Protects against voltage spikes on power lines |
| **Line conditioning** | Filters power quality issues before they reach connected equipment |
| **Network / phone port suppression** | Surge protection for ethernet and telephone connections |

### UPS Sizing

The UPS must have enough battery capacity to power connected equipment for the intended coverage period. For most data centers, the UPS is sized to cover the **generator startup window** (typically ~1 minute) plus a safety margin.

---

## Generators

A **generator** provides sustained, long-term backup power as long as it has fuel. Unlike a UPS (which is limited by battery capacity), a generator can run for hours, days, or longer.

### Generator Characteristics

| Property | Description |
|----------|-------------|
| **Startup time** | Generators require approximately 60 seconds to start up and reach stable output |
| **Fuel source** | Diesel, natural gas, or propane — must be stocked and maintained |
| **Coverage** | May power the entire building or designated "generator circuits" only |
| **Capacity** | Ranges from small portable units to large installations powering entire data centers |

### Generator-Powered Outlets

In buildings with partial generator coverage, outlets on generator circuits are typically marked — allowing operators to connect critical equipment to guaranteed backup power.

---

## UPS + Generator: Combined Strategy

Most organizations use **both** systems together because they address different phases of a power event:

```
[Grid power fails]
          |
          ▼
[UPS activates instantly] ← covers the first ~60 seconds
  - Powers connected equipment from battery
  - Generator startup begins simultaneously
          |
          ▼ (~60 seconds later)
[Generator reaches stable output]
          |
          ▼
[Power transfers from UPS to generator]
  - UPS battery begins recharging
  - Systems continue running on generator power
          |
          ▼ (grid power restored)
[Power transfers back to grid]
  - Generator shuts down
  - UPS remains charged and ready
```

**Without the UPS:** The 60-second generator startup gap would cause an unprotected outage — potentially crashing systems and corrupting data.

**Without the generator:** The UPS battery would eventually deplete, and systems would shut down during extended outages.

---

## Power Planning Considerations

| Consideration | Detail |
|--------------|--------|
| **Load calculation** | Total power draw of all connected equipment — UPS and generator must exceed this |
| **Runtime requirement** | How long does backup power need to last? Minutes (UPS only) or hours/days (generator)? |
| **Fuel storage** | Generators need adequate fuel supply — plan for extended outages |
| **Testing** | UPS batteries degrade over time; generators must be run periodically to verify they start reliably |
| **Automatic transfer switch (ATS)** | Automatically transfers power between grid, UPS, and generator — no manual intervention required |
| **Cooling** | Generators produce heat; data centers need cooling — ensure cooling systems are also on backup power |
| **Licensed electrician** | Power infrastructure should be designed and certified by qualified electrical professionals |

---

## Security Implications of Power Systems

Power systems are themselves a security concern and an attack vector:

| Risk | Description |
|------|-------------|
| **Physical attack on power** | An attacker who can cut external power or access external panels can cause a DoS without entering the building |
| **Environmental attack** | Disabling generator fuel supply prevents recovery from a power event |
| **UPS tampering** | A tampered or bypassed UPS leaves systems unprotected during power events |
| **Generator access** | Generators and fuel tanks should be physically secured |

> See also: [Physical Attacks](physical-attacks.md) — power disruption is documented as an environmental attack vector.

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Blackout** | Complete power loss — requires both UPS and generator for extended coverage |
| **Brownout** | Voltage drop — line-interactive or online UPS provides protection |
| **Surge** | Excessive voltage — UPS surge suppression protects connected equipment |
| **Standby UPS** | Switches to battery when power fails — brief transfer gap |
| **Line-interactive UPS** | Adjusts voltage during brownouts without switching to battery |
| **Online / double-conversion UPS** | Always runs from battery — zero transfer time, full power conditioning |
| **Generator** | Long-term power (~60 seconds to start) — sustained coverage as long as fuel is available |
| **UPS + generator** | UPS covers generator startup gap; generator covers extended outage — used together for full resilience |
