Ubuntu 22.04 Suspend/Resume Debugging

System Info
- Ubuntu 22.04 LTS
- GNOME Desktop
- Dell Latitude
- Logitech Bluetooth Keyboard

Initial State
Session type:

- `echo $XDG_SESSION_TYPE` > wayland

Observed Issue
- After suspend, keyboard input would behave abnormally.
- Scrolling switched workspaces.
- Occasional keyboard freeze.
- Audio resume anomalies previously observed.
- Fresh reboot resolved the issue temporarily.

Investigation Steps

1. Verified session type:
   - `echo $XDG_SESSION_TYPE`

2. Checked warning logs:
   - `sudo journalctl -b -p warning`

Notable warning:
   i8042: Warning: Keylock active

3. Verified available sessions:
   - `ls -1 /usr/share/xsessions`
   
   - `ls -1 /usr/share/wayland-sessions`

Confirmed Xorg session installed.

Mitigation

I switched it from Wayland to Xorg via the GDM session selector.

Verified:
   - `echo $XDG_SESSION_TYPE` > x11

Hypothesis

The suspend/resume state corruption impacts:
- i8042 keyboard controller
- Wayland input handling
- Bluetooth HID interaction

Status

I am observing OS behavior and testing the Xorg session for system stability.
