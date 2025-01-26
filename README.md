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

## UxPlay

UxPlay is an AirPlay 2 receiver for Linux. Newer distros ship it with their package manager. For older distros, I've compiled it for ARM64.

```sh
wget https://github.com/OscillateLabsLLC/binaries/raw/refs/heads/main/uxplay-binaries/aarch64/uxplay /usr/local/bin/spotifyd
```

And run with systemd, with user PulseAudio config:

```sh
[Unit]
Description=UxPlay AirPlay Mirror Server
After=network.target pulseaudio.service
Wants=avahi-daemon.service

[Service]
Type=simple
ExecStart=/usr/local/bin/uxplay
User=ovos
Environment=PULSE_RUNTIME_PATH=/run/user/1000/pulse
Restart=always
RestartSec=3

[Install]
WantedBy=default.target
```

You probably need the following dependencies as well:

```sh
sudo apt install \
  gstreamer1.0-plugins-base \
  gstreamer1.0-libav \
  gstreamer1.0-plugins-good \
  gstreamer1.0-plugins-bad
```
