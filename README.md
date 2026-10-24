# Apertar-Beta

Turn a supported Raspberry Pi and camera sensor into an Apertar camera.

Apertar-Beta is the prebuilt system image: Raspberry Pi OS Lite (Trixie), Apertar-UI and ApertarCore are already installed and configured. Flash the image, configure it for your hardware, and boot into the camera interface without building the software yourself.

The current interface runs directly on the display through EGLFS/DRM/KMS. A desktop, Wayland/labwc or X11 session is not required.

**[Download an image and read the release notes](https://github.com/Apertar-Studio/Apertar-Beta/releases)**

This repository is the home for image releases and setup information. For application source code, visit [Apertar-UI](https://github.com/Apertar-Studio/Apertar-UI) and [Apertar-Core](https://github.com/Apertar-Studio/Apertar-Core).

## What Is Included?

- A touchscreen camera interface with quick controls, Favorites and Presets.
- CinemaDNG, MLV, H.264 and ProRes recording, depending on the board.
- RAW and processed stills, bursts, timelapse, long exposures and exposure bracketing.
- Auto ISO, Auto Shutter, Auto White Balance, exposure compensation and metering controls.
- Focus peaking, zebras, false colour, framing guides and preview LUTs.
- An IMU-based horizontal level showing roll and pitch.
- External HDMI monitoring and display orientation controls.
- Clip/still browsing and playback.
- Audio recording, monitoring and supported gyro sidecars.
- Manual display calibration and a power-saving standby mode.
- Wi-Fi, fallback hotspot, SSH and a built-in WebUI.
- Configurable physical buttons, rotary controls and recording LEDs.

Hardware-dependent features need the appropriate sensor, panel, accessory and configuration. Check the notes for the image you download; older releases may not include everything described here.

## Supported Raspberry Pi Boards

| Board Family | Clip Recording Options |
| --- | --- |
| **Pi 5 / Compute Module 5** | CinemaDNG, MLV, H.264 and ProRes. Includes cDNG lossless/lossy compression. |
| **Pi 4 / Compute Module 4** | Uncompressed CinemaDNG, MLV and H.264. |
| **Pi 3 B / B+, Pi 3 A+, Zero 2 W, Compute Module 3 / 3+** | H.264. RAW clip recording is disabled on these USB 2.0-only boards. |

Still photography is separate from those video restrictions. Older supported boards can still take full-resolution RAW photos when the sensor and driver support them.

On supported Pi 3/4-class boards, H.264 uses the hardware encoder for output up to 1080p30. The camera's actual sensor mode can impose a lower limit. Pi 5 uses software H.264 encoding instead.

Compute Modules also need a suitable carrier board. Display, camera, storage and accessory connections must match that carrier's configuration.

**Enabled features are not performance guarantees.** Sensor readout, bit depth, cooling, RAM, storage speed and other workloads determine what a particular setup can sustain.

## Camera Sensors

The current software includes handling for:

- Sony IMX477
- Sony IMX577
- Sony IMX585
- Sony IMX585 Mono
- Sony IMX294

Available resolutions, crops, frame rates and bit depths depend on the installed driver and selected recording format. The camera menus show the options available for the detected setup.

Use the release's supported sensor configuration and matching tuning files. A sensor with the same model name is not necessarily interchangeable across every module, carrier and driver version.

## Before You Start

You will need:

- A supported Raspberry Pi or Compute Module setup.
- A compatible camera module, lens and camera cable.
- A microSD card large enough for the image's uncompressed size.
- A suitable power supply and cooling.
- Recording storage appropriate for the chosen format.
- A supported display, or a network connection for headless WebUI control.

Back up anything important on the microSD card before flashing. Flashing replaces its contents.

For RAW and high-bitrate recording, storage must sustain the write rate throughout a clip. USB 2.0 storage and slow cards can be unsuitable even if they have plenty of free space.

## Quick Start

1. Open the [Releases page](https://github.com/Apertar-Studio/Apertar-Beta/releases) and choose the appropriate disk image. Download the image asset or linked image download, not GitHub's source-code ZIP.
2. Read that release's hardware, flashing and known-issue notes.
3. Flash the image to your microSD card using an image-writing tool. Follow its verification step, then safely eject the card.
4. With the Pi powered off, connect the camera, display and any required storage. Use the correct display/camera overlays for your hardware.
5. Insert the card and power on. Let startup finish before disconnecting power.
6. In Apertar, check the detected sensor, date/time, recording destination and capture settings. Configure Wi-Fi if needed.
7. Make a short test recording and play it back before relying on the camera for a shoot.

The supplied image may be configured for a particular display or sensor. It cannot automatically identify every third-party panel and accessory.

## Connecting Without a Screen

ApertarCore serves the WebUI independently of the touchscreen interface. A configured camera can therefore be used headlessly.

On a device connected to the same network, open:

```text
http://apertar.local:8000
```

If your network/device does not resolve `.local` names, use the camera's IP address instead:

```text
http://<camera-ip>:8000
```

The WebUI includes live preview, camera settings, clip/still capture, presets and media downloads.

### Fallback Hotspot

If the camera does not connect to Wi-Fi, its fallback hotspot can start after a configurable delay.

Default hotspot details, unless changed in the image or your saved settings:

| Setting | Default |
| --- | --- |
| Network name | `Apertar` |
| Password | `apertarcam` |
| Wi-Fi fallback delay | 60 seconds |

You can change the hotspot name and password in Wi-Fi settings. Change the fallback delay in `/home/pi/apertar-hardware.conf`:

```ini
[network]
hotspot_delay_seconds=0
```

`0` removes the configured wait; another value sets the delay in seconds. The hotspot still needs the adapter to be ready. It does not replace an established Wi-Fi connection or override a disabled Wi-Fi radio.

Restart the network helper or reboot normally after changing the delay. Be aware that changing network settings can disconnect the device you are using for remote control.

### SSH

For images using the documented default login:

```bash
ssh pi@apertar.local
```

| Setting | Default |
| --- | --- |
| Username | `pi` |
| Password | `1` |

The downloaded release's instructions take precedence if its login details differ.

**Change the login and hotspot passwords before normal use.** After logging in over SSH, use `passwd` to change the account password.

The WebUI is not an authenticated internet-facing service. Keep it on a trusted network or camera hotspot, and do not expose port 8000 directly to the internet.

## Display Setup

The image's display overlays must match your actual screen. Waveshare panels, HyperPixel displays and other DPI/DSI screens are not configured identically.

The boot configuration is normally:

```text
/boot/firmware/config.txt
```

When the card is attached to a PC, this is the `config.txt` file on the visible boot partition.

Keep a backup before editing. Follow the instructions for your exact display and board; do not enable conflicting panel overlays or copy GPIO assignments from a different model.

External HDMI monitoring can be enabled under Monitoring settings. Its orientation, assisted view and information overlays can be configured independently.

**Display Calibration**, under System settings, adjusts the built-in screen and HDMI separately. It affects the UI and preview, not the recorded image.

## Recording and Still Photography

### Video

- **CinemaDNG:** RAW DNG sequences, with Uncompressed, Lossless JPEG, Lossy 3:1 and Lossy 5:1 options on supported boards.
- **MLV:** RAW video in an MLV file.
- **H.264:** Compressed MP4 video for smaller files and convenient playback.
- **ProRes:** 10-bit 4:2:2 MOV, with HQ, 422, LT and Proxy profiles. This is not ProRes RAW.

Lossy ratio names are targets, not guaranteed file sizes. Scene noise and detail affect compression.

ProRes offers selectable colour-space/gamma profiles, including log options. CinemaDNG has an independent preview profile that leaves the recorded RAW data unchanged. Read the [Core guide](https://github.com/Apertar-Studio/Apertar-Core) for colour workflow and codec limitations.

### Stills and Timelapse

Long-press the on-screen record/shutter button to choose Clip, Still or Timelapse.

Still options include DNG, JPEG, PNG, TIFF and supported RAW + processed pairs, plus a self-timer, 3/5/10-shot bursts and three-shot exposure bracketing with an adjustable EV offset.

Long Exposure offers shutter times up to 30 seconds where supported. Preview remains responsive while composing; the actual shot uses a blackout and countdown.

Timelapse supports selectable and custom intervals, including fractional seconds, with a shot counter. Exposure choices must fit the interval, and readout/writing still take time. Captures are not stacked into an overlapping queue.

## Storage and Your Files

The standard recording destination is:

```text
/media/RAW
```

Use **Settings > Media** to inspect and mount the intended drive. Automatic mounting depends on the storage type, hardware configuration and image release; do not assume every connected drive is already mounted.

Still photos go in the `Photos` folder. Clip folders contain the picture files and any audio, gyro or recovery sidecars. Keep those associated files together when copying a clip.

The remaining-time display estimates recording minutes for the current setup. It is not a guaranteed duration, particularly with scene-dependent RAW compression.

Formatting media erases its contents. Back up your files first.

**Wait for WRITING to finish before ejecting storage or shutting down.** Use the camera's eject control before physically removing recording media.

## Customising the Camera

| Item | Location |
| --- | --- |
| Hardware configuration | `/home/pi/apertar-hardware.conf` |
| Colour calibration overrides | `/home/pi/apertar-color-profiles.conf` |
| Custom tuning files | `/home/pi/Tuning-Files/` or recording media's `Tuning-Files/` |
| Preview LUTs | `/home/pi/LUTs/` or recording media's `LUTs/` |
| Pi 5 default tuning | `/home/pi/Tuning-Files/defaults-pisp/` |
| Pi 3/4 default tuning | `/home/pi/Tuning-Files/defaults-vc4/` |

Use the tuning/LUT pickers' Refresh controls to discover newly added files. Selecting a different tuning file restarts the camera pipeline without rebooting the Pi.

Hardware configuration controls supported buttons, encoders, recording LEDs, IMUs and other accessories. Enable only devices you have connected, and use their actual bus, address and wiring.

GPIO numbering is not physical header numbering, and I2C buses can differ between boards. Power off before changing wiring. Use 3.3V-compatible GPIO signals and a suitable resistor for LEDs.

Hardware configuration belongs in `/home/pi/`, not on the boot partition. Restart the relevant service or reboot normally after changing hardware mappings.

## Standby and Safe Power-Off

Use **System > Power Actions > Standby** to pause the camera between shots, or configure an idle timeout. On supported hardware, double-pressing the Pi power button also enters standby.

Wake with a physical camera button, a single power-button press, or the WebUI's Wake Camera control. Touchscreen input does not wake the camera, and the first physical wake press does not also trigger its normal action.

Standby pauses capture and monitoring and turns off supported display outputs/backlights. It is not a full shutdown. Optional USB power-off on Pi 5 uses safeguards for mounted storage and critical USB devices.

To disconnect power, stop recording, wait for WRITING to finish, select **Shutdown**, and wait for shutdown to complete. Standby is not a substitute for shutdown.

Sudden power removal can corrupt recordings or the SD card. Keep backups of important clips, configuration and working images.

## Updating and Getting Help

A new disk image replaces the card's existing system. Back up personal settings, presets, LUTs, tuning files and any recordings on that card before reflashing.

Follow the update instructions for the specific release. Application updates should keep Apertar-UI and ApertarCore in step; arbitrary OS/kernel or camera-stack upgrades can affect custom drivers and graphics support.

This is beta software. Test the modes you need before an important shoot, especially after changing hardware, drivers or tuning.

If something goes wrong, open an [issue](https://github.com/Apertar-Studio/Apertar-Beta/issues) with:

- The image version and Raspberry Pi model.
- Sensor, display and recording-storage details.
- Format, resolution, crop, bit depth and frame rate.
- Clear steps to reproduce the problem.
- Relevant logs and, for image problems, a small sample file.

Do not include passwords, private SSH keys or other personal credentials in reports.

## Credits

Apertar builds on the work of libcamera, Raspberry Pi, Qt and many other open-source projects. Inspiration comes from [Schoolpost's CinePi SDK](https://github.com/cinepi/cinepi-sdk).

Thanks to everyone testing Apertar, sharing camera builds and helping improve the project. Components included in the image retain their respective licences.
