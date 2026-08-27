# Olga
# Template
**10" long-range quadcopter — Cube Orange, companion computer, DroneBridge link**

Olga is a 10-inch class multirotor built around an X500 v2 Frame, CubePilot Cube Orange with an
onboard companion computer for offboard control and a DroneBridge Wi-Fi link for
telemetry.

---

## Hardware

| Subsystem | Component | Notes |
|---|---|---|
| Frame / props | 10 inch | |
| Motors | 3115 | |
| ESC | 4-in-1 | |
| Flight controller | Orange Cube (CubePilot) | Standard carrier board |
| Telemetry link | DroneBridge | Wi-Fi, MAVLink |
| Companion computer | Offboard computer | Serial MAVLink to FC |
| Payload | Gimbal | Serial control |
| Rangefinder | Distance sensor | Serial |
| Positioning | GNSS | DroneCAN |

---

## Port Map

Everything hanging off the Cube, in one place.

| Device | Cube port | ArduPilot serial | Protocol |
|---|---|---|---|
| Offboard computer | `TELEM1` | SERIAL1 | MAVLink 2 |
| Gimbal control | `TELEM2` | SERIAL2 | MAVLink 2 / gimbal-specific |
| DroneBridge | `GPS1` | SERIAL3 | MAVLink 2 |
| Distance sensor | `GPS2` | SERIAL4 | Serial rangefinder |
| GNSS | `CAN1` | — | DroneCAN |

> **Note:** `GPS1` and `GPS2` are repurposed as general-purpose UARTs here — the
> GNSS comes in over CAN instead, so neither port is doing GPS duty.

### Wiring diagram

```
                      ┌──────────────────────┐
  Offboard Computer ──┤ TELEM1               │
     Gimbal Control ──┤ TELEM2               │
        DroneBridge ──┤ GPS1    Orange Cube  │
    Distance Sensor ──┤ GPS2                 │
               GNSS ──┤ CAN1                 │
                      └───────────┬──────────┘
                                  │
                              4-in-1 ESC
                                  │
                          4 × 3115 / 10"
```

---

## Configuration

Suggested starting points — **verify against your actual hardware before flying.**

| Parameter | Value | Purpose |
|---|---|---|
| `SERIAL1_PROTOCOL` / `_BAUD` | 2 / 921 | MAVLink 2 to companion, high rate |
| `SERIAL2_PROTOCOL` / `_BAUD` | 2 / 115 | Gimbal |
| `SERIAL3_PROTOCOL` / `_BAUD` | 2 / 115 | DroneBridge (default 115200) |
| `SERIAL4_PROTOCOL` | 9 | Rangefinder |
| `CAN_P1_DRIVER` / `CAN_D1_PROTOCOL` | 1 / 1 | DroneCAN GNSS |
| `GPS1_TYPE` | 9 | DroneCAN GPS |

If Olga is running PX4 rather than ArduPilot, the equivalent settings live under
`MAV_*`, `SENS_*` and `UAVCAN_ENABLE` — the physical port map above is unchanged.

---

## TODO / Fill in

- [ ] Firmware and version (ArduPilot vs PX4)
- [ ] Battery: cell count, capacity, expected flight time
- [ ] Exact motor / ESC / prop part numbers and KV
- [ ] Gimbal model and control protocol
- [ ] Distance sensor model and mounting orientation
- [ ] GNSS module (Here3/Here4/other)
- [ ] Companion computer model + what it runs
- [ ] AUW and thrust-to-weight
- [ ] Link to saved parameter file

---

## Preflight

1. Props off for any bench work with the battery connected.
2. Confirm GNSS lock and HDOP before arming.
3. Check rangefinder reads sane values on the ground.
4. Verify DroneBridge link and RSSI at the launch point.
5. Confirm companion computer heartbeat is visible to the FC.
