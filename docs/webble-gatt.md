# WebBLE and GATT reference

This document describes the interface exposed by the sensor. All multi-byte fields are little-endian. Temperatures and humidities are integer hundredths unless noted.

## Advertisement

The main advertisement carries the BTHome service data and the shortened local name.

| Bytes | Field |
| --- | --- |
| `02 01 06` | Flags: LE General Discoverable, BR/EDR not supported |
| `0C 16 D2 FC` | Service Data, 16-bit UUID `0xFCD2` |
| `40` | BTHome device info: version 2, unencrypted, non-triggered |
| `01 <u8>` | Battery percent |
| `02 <s16>` | Temperature in 0.01 °C |
| `03 <u16>` | Moisture in 0.01% |
| `08 "SoilPulse"` | Shortened local name |

The scan response adds the 128-bit primary service UUID and the complete local name. Pairing is not required and BTHome payloads are not encrypted.

## GATT services

Primary service: `749b53b7-2662-4658-9fb4-e47241816727`. Standard services are GAP (`0x1800`), GATT (`0x1801`) and Device Information (`0x180A`). Device Information reports the model and firmware version.

| UUID | Properties | Payload |
| --- | --- | --- |
| `0xFFE1` | Read, Notify | Recent trend samples |
| `0xFFE2` | Write | 6 bytes: `u32` Unix seconds + `s16` timezone offset in minutes |
| `0xFFE3` | Read | Daily samples |
| `0xFFE5` | Write | `CLR1` or `RST1`, 4 ASCII bytes |
| `0xFFE6` | Write | 1 byte: `0x00` dry, `0x01` wet |
| `0xFFE7` | Write | `0x01` forces one new measurement |
| `0xFFE8` | Read, Write | `s16`, 0.01 °C, range ±1000 |
| `0xFFE9` | Read | 2 bytes: calibration flags + result code |
| `0xFFEA` | Read, Write | Device display name, up to 20 bytes |
| `0xFFEB` | Read, Notify | Latest reading |
| `0xFFEC` | Read, Write | 24-hour history window |
| `0xFFED` | Read | 30-day daily summary |
| `0xFFEE` | Read | 30-day temperature extremes |

Renaming changes only the GATT display name. The BTHome broadcast name stays `SoilPulse`, so Home Assistant keeps its pairing. Firmware updates are available through the dashboard.

History characteristics return records oldest first. The dashboard decodes them; third-party tools can read the values using the units listed above.

## Control writes

| Characteristic | Value | Action |
| --- | --- | --- |
| `0xFFE2` | `u32` epoch + `s16` timezone | Sync time. Timezone uses the JavaScript `getTimezoneOffset()` sign. |
| `0xFFE5` | `CLR1` | Clear history, keep calibration |
| `0xFFE5` | `RST1` | Clear calibration and history, then reboot |
| `0xFFE6` | `0x00` / `0x01` | Store dry / wet moisture anchor |
| `0xFFE7` | `0x01` | Force one measurement, update `0xFFEB` only |
| `0xFFE8` | `s16` | Temperature offset in 0.01 °C steps |
| `0xFFEA` | ASCII bytes | Display name, 20-byte limit |

Calibration flags at `0xFFE9` use bit 0 for dry, bit 1 for wet and bit 2 for temperature offset. The second byte is the result code documented in [calibration.md](calibration.md).
