# Apertar-Alpha

Apertar-Alpha is a prebuilt Raspberry Pi 5 image.

It is intended as a ready-to-use alpha release for supported hardware, with Apertar-UI and ApertarCore already configured so you can boot directly into the camera system without a full manual setup.

## Included

- Apertar-UI
- ApertarCore
    - Raspberry Pi 5 camera workflow
    - Touch-first camera interface
    - cDNG recording
    - DNG still capture
    - Playback and media browser
    - Sensor auto-selection for supported camera setups

## Supported Camera Sensors

- Sony IMX585
- Sony IMX477

## Intended Use

This image is for early testing, evaluation, and development. It is an alpha release and should be treated as experimental software.

## Notes

- Designed for Raspberry Pi 5

- By default its configured for the Waveshare 4Inch Square DSI Display. If you want to use the HyperPixel 4Inch Square DPI display instead you can uncomment the HyperPixel dtoverlay and comment the Waveshare ones out like so:

#Hyperpixel 4Inch Square DPI Display
dtoverlay=vc4-kms-dpi-hyperpixel4sq

#Waveshare 4Inch Square DPI Display
#dtoverlay=vc4-kms-DPI-4inch
#dtoverlay=waveshare-4dpi
#dtoverlay=waveshare-touch-4dpi

If you use a CFEHat and the i2c channel of it is different from whats in the automount.py at /home/pi/ go ahead and edit this file with the correct i2c channel.
