# Binaries

Various binaries and/or wheels I've compiled for specific use cases.

## spotifyd

`spotifyd` does not have an ARM64 release, so I've compiled it for my Raspberry Pi 4.

```sh
wget https://github.com/OscillateLabsLLC/binaries/raw/refs/heads/main/spotifyd-binaries/aarch64/spotifyd /usr/bin/spotifyd
```

Configure with `~/.config/spotifyd/spotifyd.conf`:

```toml
[global]
use_mpris = true
dbus_type = "session"

device_name = "YourNameHereNoSpaces"
```

And run with user systemd:

~/.config/systemd/user/spotifyd.service

```sh
[Unit]
Description=A spotify playing daemon
Documentation=https://github.com/Spotifyd/spotifyd
Wants=sound.target
After=sound.target
Wants=network-online.target
After=network-online.target

[Service]
ExecStart=/usr/bin/spotifyd --no-daemon
Restart=always
RestartSec=12

[Install]
WantedBy=default.target
```
