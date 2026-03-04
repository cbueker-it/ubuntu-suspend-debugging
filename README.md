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
- After a suspension, keyboard input would behave abnormally.
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

Update (Two Weeks Later)

After running the system on the Xorg session for approximately two weeks, my Ubuntu 24.04 system has remained completely stable.

These are the results that I observed:

- No more abnormal keyboard behavior after suspend.
- No workspace switching caused by scrolling.
- No keyboard freezes.
- Audio resumes correctly after suspend.
- Overall suspend/resume behavior is stable and predictable.

Conclusion:

Switching from Wayland to Xorg resolved the suspend/resume input state issue on my daily Linux machine. The issue appears to have been related to the interaction between Wayland input handling, the i8042 keyboard controller, and Bluetooth HID devices during the suspend/resume cycle.

My Ubuntu distro (the OS) has been operating normally since the change!
