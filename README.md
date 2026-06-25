### Logitech K380 Bluetooth Keyboard F-n keys mode switcher for MacOS

F-n keys are working in non-standard mode by default on this keyboard.
I do not want to mess with "Logi Options" just for switching so I made this little app.

#### How to use
$ sudo ./k380 -f on|off

#### Raycast Script Command

If the keyboard returns to the default media-key behavior after sleep or a
Bluetooth reconnect, you can run the same command manually from Raycast.

1. Make sure the command works in Terminal:

```sh
sudo "$HOME/Code/k380-macos/k380" -f on
```

2. Create a Raycast script command at:

```text
~/Code/k380-macos/RaycastScripts/k380-fn-on.sh
```

Example script:

```sh
#!/bin/bash

# @raycast.schemaVersion 1
# @raycast.title K380 Fn On
# @raycast.mode compact
# @raycast.packageName Keyboard
# @raycast.icon ⌨️

sudo "$HOME/Code/k380-macos/k380" -f on
```

3. Make the script executable:

```sh
chmod +x "$HOME/Code/k380-macos/RaycastScripts/k380-fn-on.sh"
```

4. Add the script directory in Raycast:

```text
~/Code/k380-macos/RaycastScripts
```

Open Raycast, go to `Extensions` -> `Script Commands`, then add this directory.
After that, search for `K380 Fn On` in Raycast and run it when the keyboard
needs the F-key mode applied again.

If Raycast cannot run the command because `sudo` asks for a password, allow only
this binary to run without a password:

```sh
sudo visudo
```

Add:

```text
<your-username> ALL=(root) NOPASSWD: /Users/<your-username>/Code/k380-macos/k380
```

### Additional info to build from sources

The app depends on https://github.com/libusb/hidapi to communicate with HID subsystem
But there's a known problem [https://github.com/libusb/hidapi/issues/127] with hidapi library and device paths enumeration on MacOS so patch is required:
https://github.com/flirc/hidapi/commit/8d251c3854c3b1877509ab07a623dafc8e803db5
( or `0001-Mac-use-IORegistryEntryGetRegistryEntryID-to-resolve.patch` which matches with hidapi sources version 0.10.1 )
