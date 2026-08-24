# MuteAlert for Windhawk

MuteAlert adds a live microphone meter and call-mute controls directly to the
Windows 11 taskbar. This repository contains the Windhawk edition; it runs
inside Explorer and does not require the standalone MuteAlert application.

## Features

- Live bottom-to-top microphone activity meter using Windows Core Audio.
- Left-click Windows input mute/unmute and scroll-over volume control.
- Optional volume lock which restores a chosen input level; scrolling updates
  and persists the locked target.
- Active Slack, Microsoft Teams, and Zoom call icon with app logo, mute state,
  left-click focus, and right-click mute/unmute.
- Speaking-while-call-muted warning with an optional Windows audio cue.
- Zoom meeting persistence through `CptHost.exe` and an `Alt+A` fallback.
- Headset mute synchronization through Windows hardware-mute reporting,
  standard USB HID microphone/call-mute controls, and vendor adapters.
- SteelSeries Arctis Nova Pro Wireless high-confidence vendor integration.
- Detection method and confidence in the taskbar tooltip.
- Sanitized diagnostic export with HID descriptors and changed byte offsets,
  excluding device paths, serial numbers, and raw report values.

## Install

1. Install [Windhawk](https://windhawk.net/).
2. Open **Create a new mod** in Windhawk.
3. Replace the template with
   [`mutealert.wh.cpp`](mutealert.wh.cpp).
4. Compile the mod, then enable it for `explorer.exe`.

The catalogue ID is `mutealert`. If you previously installed a local build
using the old `microphone-activity-taskbar-widget` ID, remove that mod before
installing the catalogue version to avoid duplicate taskbar icons.

## Headset limitations

Headset hardware is not universal. The mod reports one of four methods:
`Windows hardware mute`, `Standard HID mute button`, `SteelSeries device
state`, or `Unsupported/no observable state`.

A standard HID button event does not necessarily reveal the position of a
latched physical switch. Purely mechanical microphone disconnect switches are
not observable in software. MuteAlert never treats a zero audio level as proof
of physical mute.

See [HEADSET_PROVIDERS.md](HEADSET_PROVIDERS.md) for adapter extension points
and diagnostic privacy details.

## License

MIT. See [LICENSE](LICENSE).
