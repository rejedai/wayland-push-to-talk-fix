# wayland-push-to-talk-fix
This fixes the inability to use push to talk in Discord when running Wayland


**NOTE: by default the left Meta (Windows) key is used for push to talk. In order to use a different key, set `EV_KEY_CODE` and `XKEY_EVENT` environment variables.**

## Requirements

Nothing special.
- C++ compiler & Make
- libevdev
- libxdo (Debian/Ubuntu: `libxdo-dev`, Fedora/Centos: `libxdo-devel`)

## Approach

Read specific key events via evdev (needs sudo) and then pass them to libxdo to inject key presses to X apps.

# Installation

## Manual run

```
make
sudo ./push-to-talk /dev/input/by-id/<device-id> &
```

## Autostart

First edit the `push-to-talk.desktop` file and replace `/dev/input/by-id/<device-id>` with your device path. Then:
```
make
sudo make install

# to allow you access `/dev/input` devices without root
sudo usermod -aG input <your username>
```
Then just log out and log in. A process named `push-to-talk` should be running (visible in any process monitor).

## Configure

Supported options:

| Environment variable | Description                                                                                                                                                                     |
|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `EV_KEY_CODE`        | Сode of the button that program expects to be pressed.<br/><br/>Full list:<br/>https://github.com/torvalds/linux/blob/master/include/uapi/linux/input-event-codes.h             |
| `XKEY_EVENT`         | Event to be sent to X11 apps.<br/><br/>Full list (Ignore leading **XKB_KEY_**):<br/>https://github.com/xkbcommon/libxkbcommon/blob/master/include/xkbcommon/xkbcommon-keysyms.h |
| `SKIP_EVENT_CHECK`   | Skipping the evdev event existence check. Useful for cases when the mouse is used keyboard events.                                                                              |




# License

MIT
