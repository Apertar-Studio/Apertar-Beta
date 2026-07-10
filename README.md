# Apertar-Beta

Apertar-Beta is a prebuilt Raspberry Pi 5/4 and Compute Module 5/4 image based on Raspberry Pi OS Lite (Trixie).

It is intended as a ready-to-use beta release for supported hardware, with Apertar-UI and ApertarCore already installed and configured so the system boots directly into the camera environment without requiring a full manual setup.

Download: https://github.com/Apertar-Studio/Apertar-Beta/releases/tag/1.3.0

## Included

- Apertar-UI
- ApertarCore
- Raspberry Pi 5/4 Compute Module 4/5 camera workflow
- Touch-first camera interface
- Built-in WebUI for local network camera control
- CinemaDNG (cDNG) recording
- DNG still capture
- Playback and clip browser
- Favorites and Presets workflow
- Sensor auto-selection for supported camera setups
- Improved monitoring tools including Focus Peaking, Zebra, False Color, Greyscale, Guides, Center Marker, and SMPTE Color Bars
- External monitor support
- Improved audio recording and monitoring workflow
- Improved timecode and date/time behaviour
- Performance and responsiveness optimizations across recording, playback, preview, and UI


## Supported Camera Sensors

- Sony IMX585
- Sony IMX477

## Intended Use

Apertar has moved beyond the alpha stage and is now in beta. This image is intended for testing, evaluation, development, and broader real-world use on supported hardware while the platform continues to evolve.

## Notes

- Designed for Raspberry Pi 5/4

- To access the WebUI use this address: apertar.local:8000 or yourpiIP:8000

- For ssh access use:
    ssh pi@apertar.local
    password: 1

- By default its configured for the Waveshare 4Inch Square DSI Display. If you want to use the HyperPixel 4Inch Square DPI display instead you can uncomment the HyperPixel dtoverlay and comment the Waveshare ones out like so in the config.txt (/boot/firmware/config.txt):

#Hyperpixel 4Inch Square DPI Display
dtoverlay=vc4-kms-dpi-hyperpixel4sq

#Waveshare 4Inch Square DPI Display
#dtoverlay=vc4-kms-DPI-4inch
#dtoverlay=waveshare-4dpi
#dtoverlay=waveshare-touch-4dpi


- To configure the bus/address for the INA219 Current shunt and power monitor sensor or CFE-Hat modify the ~/apertar-hardware.conf config file (home/pi/apertar-hardware.conf):
  By default the config is:
[ina219]
bus=4
address=0x40

[cfexpress_hat]
bus=10
address=0x34

[storage]
mount_path=/media/RAW
pcie_controller=1000110000.pcie


## Credit
Inspiration has been taken from Schoolpost's project Cinepi-SDK: https://github.com/cinepi/cinepi-sdk
