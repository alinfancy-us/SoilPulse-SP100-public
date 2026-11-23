# Troubleshooting

The sensor spends most of its time in deep sleep to save power. Most support issues come from that duty cycle rather than a failed radio.

## The sensor does not appear in a Bluetooth scan

- Touch the pad first. A touch opens a longer advertising window.
- A normal update happens about every 10 minutes. Scanning for less than a minute can miss the window entirely.
- Keep the phone or host within a few metres while pairing.
- Search for the name `SoilPulse` or the service UUID `749b53b7-2662-4658-9fb4-e47241816727`.
- No pairing code is needed.

## Home Assistant says unavailable

That is expected between updates. Packets arrive roughly every 10 minutes. If the state never returns, touch the pad and wait for one full cycle.

Range figures assume open space. Keep the host close to the sensor.

## Touching the pad does not change the value

A touch opens the advertising window; it does not force a measurement. The sensor rebroadcasts the last successful sample. To measure now, connect over GATT and write `0x01` to `0xFFE7`, then read `0xFFEB`. The Home Assistant entity updates on the next BTHome packet.

## Web Bluetooth is unavailable

| Situation | Fix |
| --- | --- |
| iOS Safari or iOS Chrome | Install the Beacio extension and enable it for the site; on older iOS devices, use Bluefy |
| Desktop Safari or Firefox | Use Chrome or Edge over HTTPS |
| Page loads but Connect fails | Check Bluetooth permission, then touch the sensor pad |

The connection can take a few seconds to set up while the sensor advertises. Give it a moment before retrying.

## Moisture looks wrong

- Moisture is relative. Calibrate dry and wet anchors as described in [calibration.md](calibration.md).
- The probe needs good contact with the soil around it. Insert to the `OPTIMAL DEPTH` mark (about 2.4 in), avoid the pot edge and press the soil gently around the probe.
- The moisture channel has no temperature compensation.
- Check the battery level. A nearly empty battery can stop new measurements, in which case the last good reading is repeated.

## Calibration is rejected

Read `0xFFE9`. Code 3 means the dry sample was at or above 20%; code 4 means the wet sample was at or below 80%; code 5 means the two anchors were too close together; code 7 means the reading direction was reversed. Dry air must produce a higher probe reading than water.

## History changed after a battery replacement

Some recent history is held in memory and resets when the sensor loses power. Longer-term history is kept. After the dashboard writes the time, today's chart is rebuilt from the stored records. Data taken before the first time sync has no timestamp and is not shown in the daily chart.

## Firmware update fails

- Make sure the battery is in good condition before starting.
- Keep the phone or computer close and the screen unlocked during the transfer.
- If the link drops, reconnect and retry from the dashboard.
- If the sensor keeps rebooting after an update, leave it powered for a few minutes and let the recovery finish.

## Reset commands

| Write to `0xFFE5` | Effect |
| --- | --- |
| `43 4C 52 31` (`CLR1`) | Clear history, keep calibration |
| `52 53 54 31` (`RST1`) | Clear calibration and history, then reboot |

A factory reset also clears the device display name stored at `0xFFEA`.
