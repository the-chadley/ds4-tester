# ds4-tester

A controller tester for PS4 pads on Linux. Shows buttons, sticks, triggers and
the touchpad live, tests both rumble motors, and sets the lightbar colour. You
can name each pad, and it remembers a colour for each one and puts it back
when the pad connects.

Built for the clone pads (`Wireless Controller`, ID `054C:09CC`), but it uses
the kernel's normal `playstation` driver, so it should work with real ones too.

Using a clone over Bluetooth that won't reconnect? You probably need
[ds4-clone-bt-fix](https://github.com/the-chadley/ds4-clone-bt-fix) as well.

## Install

It needs Python 3 with evdev, pycairo and Tkinter. Install them for your distro:

```bash
sudo dnf install python3-evdev python3-cairo python3-tkinter   # Fedora
sudo apt install python3-evdev python3-cairo python3-tk        # Debian, Ubuntu, Mint
sudo pacman -S python-evdev python-cairo tk                    # Arch
```

If one goes missing later (a distro upgrade can do that), the app tells you
which one when you open it.

Then:

```bash
git clone https://github.com/the-chadley/ds4-tester.git
cd ds4-tester
./install-user.sh
```

That adds DS4 Tester to the app menu (under System) and sets up the lightbar
colour restore. You can also just run `python3 ds4-tester.py`.

Buttons, sticks and rumble work straight away. For the touchpad, and for
setting the lightbar from the menu, install the udev rules:

```bash
sudo cp 70-ds4-tester.rules /etc/udev/rules.d/
sudo udevadm control --reload
sudo udevadm trigger --action=change --subsystem-match=input --subsystem-match=leds
```

## Command line

`ds4led` sets the colour without the GUI:

```bash
sudo ./ds4led list
sudo ./ds4led all blue
sudo ./ds4led 50:43 ff4500
```

`python3 ds4-tester.py --list` lists connected pads.

## Removing it

```bash
./install-user.sh --uninstall
sudo rm /etc/udev/rules.d/70-ds4-tester.rules
```
