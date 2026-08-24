# Headset mute providers

Headset mute synchronization separates observable button events from a known
latched mute state. Silence is never used to infer physical mute.

## Provider order

1. **SteelSeries device state** reads the Arctis Nova Pro Wireless vendor
   status report and supplies a high-confidence latched state.
2. **Standard HID mute button** observes USB HID System Microphone Mute,
   Phone Mute, and Call Mute Toggle usages. These are medium-confidence events,
   not guaranteed switch positions.
3. **Windows hardware mute** uses Core Audio's
   `ENDPOINT_HARDWARE_SUPPORT_MUTE` flag and endpoint mute state.
4. **Unsupported/no observable state** means no trustworthy signal is
   available.

After a standard HID event, the mod briefly waits for Windows or the active
call application to react, then synchronizes only states that did not already
change. This avoids unnecessary duplicate toggles.

## Vendor adapters

Vendor integrations implement `IHeadsetMuteAdapter` in the Windhawk source.
The registry includes extension slots for Logitech (`046D`), Jabra (`0B0E`),
Poly/Plantronics (`047F`), and Corsair (`1B1C`). Those entries document adapter
boundaries; they do not claim support for proprietary protocols that have not
been implemented and tested.

An adapter must match confirmed vendor/product IDs, validate HID report sizes,
report a known state only when the device explicitly supplies it, avoid writes
during discovery, and never derive mute from audio silence.

## Diagnostics and privacy

Set **Export sanitized headset diagnostics** to a full `.txt` path in the mod
settings and apply the settings. The export records:

- USB vendor and product IDs;
- manufacturer and product names exposed by the device;
- top-level usages and report lengths;
- recognized standard mute usages;
- registered vendor adapter status;
- report number, length, and changed byte offsets.

It excludes HID device paths, serial numbers, and raw report values. When
requesting support, reproduce one physical control change at a time before
exporting so contributors can correlate the changed offsets safely.

