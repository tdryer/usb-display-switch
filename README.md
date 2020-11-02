# usb-display-switch

This is a script to enable or disable display standby when a USB device is
added or removed.

If you have multiple computers a USB switch device (a hub with multiple inputs
and a button to toggle between them), you can use this script to also switch
your display inputs automatically. When you press the button, the switch device
is removed from one system and added to another. If the removed system turns
the display off, and the added system turns the display on, the display will
switch to the active input.

This solution is inspired by Haim Gelfenbeyn's [display-switch] software and
[article], which rely on [DDC/CI] to change display inputs. This didn't work
for me because my display doesn't accept DDC/CI requests from inactive inputs.
Instead, this script uses [DPMS] to control display standby, and relies on the
display to automatically switch to the only input that is not on standby.

[display-switch]: https://github.com/haimgel/display-switch
[article]: https://haim.dev/posts/2020-07-28-dual-monitor-kvm/
[DDC/CI]: https://en.wikipedia.org/wiki/Display_Data_Channel
[DPMS]: https://en.wikipedia.org/wiki/VESA_Display_Power_Management_Signaling

## Requirements

You'll need two X11-based Linux systems, with input peripherals connected to a
USB switch device, and video outputs connected to different inputs on your
displays.

On each of these systems, you'll need Python 3 and [pyudev]. On Debian
derivatives, you can install the `python3-pyudev` package:

```
sudo apt install python3-pyudev
```

[pyudev]: https://pyudev.readthedocs.io/

## Setup

1. Modify the `usb-display-switch` script to contain the vendor ID and product
   ID of your switch device.

   For example, when I plug in my switch device, I get the following message in
   `dmesg`:

   ```
   usb 3-9: New USB device found, idVendor=0bda, idProduct=5411, bcdDevice= 1.44
   ```

   So I configure the script like this:

   ```
   VENDOR_ID = 0x0BDA
   PRODUCT_ID = 0x5411
   ```

2. On each system you want to switch between, ensure that `usb-display-switch`
   is always running.
