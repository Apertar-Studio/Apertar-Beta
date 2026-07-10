# Apertar-Beta

Apertar-Beta is a prebuilt Raspberry Pi 5/4 image.

It is intended as a ready-to-use beta release for supported hardware, with Apertar-UI and ApertarCore already configured so you can boot directly into the camera system without a full manual setup.

Download: https://drive.google.com/file/d/1JYzEb4-S1-vu2WE1iy9QUHZNJfmhYllt/view?usp=sharing

## Included

- Apertar-UI
- ApertarCore
    - Raspberry Pi 5/4 camera workflow
    - Touch-first camera interface
    - cDNG recording
    - DNG still capture
    - Playback and media browser
    - Sensor auto-selection for supported camera setups

## Supported Camera Sensors

- Sony IMX585
- Sony IMX477

## Intended Use

We have passed the alpha stage and moving on to beta stage. This image is for testing, evaluation, and development.

## Notes

- Designed for Raspberry Pi 5/4


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
