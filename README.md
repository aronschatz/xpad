# Updated Xpad Linux Kernel Driver
Driver for the Xbox/ Xbox 360/ Xbox 360 Wireless/ Xbox One Controllers

## Changes compared to Linux Staging (4.2)

* controller LEDs and Force Feedaback now works even after suspend/ resume
* LED and Force Feedback packets are now re-submitted instead of being dropped
  * also fixes kernel warnings due to submitting active URB requests
* only expose actually connected Xbox 360 Wireless Controllers ([patch by  Pierre-Loup A. Griffais](http://www.spinics.net/lists/linux-input/msg29450.html))
* updated Xbox One controller force feedback ([patch by  Pierre-Loup A. Griffais](https://github.com/ValveSoftware/steamos_kernel/commit/f5f73eb889cac32cbabfc40362fe5635a2255836))
* Xbox 360 Wireless button mappings are now compatible with Xbox 360 (non-wireless) mappings
* added support for Xbox One Covert Forces Edition (Patch by Erik Lundgren)
* fixed support for Razer Atrox Arcade Stick (Patch by Dario Scarpa)

# Installation
```
sudo git clone https://github.com/paroj/xpad.git /usr/src/xpad-0.4
sudo dkms install -m xpad -v 0.4
```
# Updating
```
cd /usr/src/xpad-0.4
sudo git fetch
sudo git checkout origin/master
sudo dkms remove -m xpad -v 0.4 --all
sudo dkms install -m xpad -v 0.4
```
# Usage
This driver creates three devices for each attached gamepad

1. /dev/input/js**N**
    * example `jstest /dev/input/js0`
2. /sys/class/leds/xpad**N**/brightness
    * example `echo COMMAND > /sys/class/leds/xpad0/brightness` where COMMAND is one of
        *  0: off
        *  1: all blink, then previous setting
        *  2: 1/top-left blink, then on
        *  3: 2/top-right blink, then on
        *  4: 3/bottom-left blink, then on
        *  5: 4/bottom-right blink, then on
        *  6: 1/top-left on
        *  7: 2/top-right on
        *  8: 3/bottom-left on
        *  9: 4/bottom-right on
        * 10: rotate
        * 11: blink, based on previous setting
        * 12: slow blink, based on previous setting
        * 13: rotate with two lights
        * 14: persistent slow all blink
        * 15: blink once, then previous setting
3. the generic event device
    * example `fftest /dev/input/by-id/usb-*360*event*`

# Sending Upstream

1. `git format-patch --cover-letter upstream..master`
2. `git send-email --to xxx *.patch`
