avr-x1100w is the model number of AV receiver I use, and the code found in this
package orchestrates the receiver, as well as a video projector (Optoma
HD25-LV, attached via a usb-to-serial adapter).

The intention is that I never have to switch the source of my video projector
or AV receiver, and I never have to turn them on or off. Instead, this
orchestration tool looks at all the available media input sources and controls
the devices.

Theoretically, HDMI’s CEC covers a subset of these functions (e.g. it should
automatically switch input source appropriately). In practice it turns out that
my video projector does not support CEC at all and the AVR only supports
switching the input source, not powering on/off.

## Logic

The program takes the following inputs into account:

1. Which app is running on the
   [Chromecast](http://www.google.com/chrome/devices/chromecast/) (e.g.
   Netflix, YouTube).
1. Whether my [Zotac
   ZBOX](http://www.zotac.com/products/mini-pcs/zbox/nvidia/product/nvidia/detail/zbox-id82.html)
   is powered on, implying that one wants to use the
   [OpenELEC](http://openelec.tv/) installation that it runs.
1. Whether midna (my workstation) is unlocked.

Based on these inputs, the following outputs are controlled:

1. AVR power state (on or standby).
1. AVR input source (PC, Chromecast or ZBOX).
1. Video projector power state (on or standby).

For the logic that determines the outputs, see stateMachine() in main.go.

## Cross-compilation

In order to cross-compile this code to run on a Raspberry Pi, use the following
commands on a Debian host system:

```bash
sudo dpkg --add-architecture armel
sudo apt-get update
sudo apt-get install golang-go-linux-arm gcc-arm-linux-gnueabi \
  gcc-4.9-arm-linux-gnueabi libc6-dev:armel linux-libc-dev:armel
CC=arm-linux-gnueabi-gcc CGO_ENABLED=1 GOARCH=arm GOARM=5 go build
```

(The instructions are somewhat elaborate because go-rs232 needs cgo.)