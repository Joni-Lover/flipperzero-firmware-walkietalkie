# walkie_talkie

## Overview
This fork adds ability to listen to 22-channel walkie-talkies using a Flipper Zero.


To install on your Flipper Zero:
1. Backup any important data on your Flipper Zero before proceeding!
2. Close qFlipper, lab.flipper.net and serial windows
3. Connect Flipper Zero to your computer
4. Open a new command window (Windows+R, CMD, Enter) 
5. Run the following commands to update your Flipper Zero firmware:

```cmd
mkdir \flipper
cd \flipper
git clone --recursive https://github.com/jamisonderek/flipperzero-firmware-walkietalkie
cd flipperzero-firmware-walkietalkie
git checkout jamisonderek/walkietalkie
git pull
fbt COMPACT=1 DEBUG=0 FORCE=1 flash_usb_full

```

Features:
- [customized setting_user file](https://github.com/flipperdevices/flipperzero-firmware/commit/49e95313e9730ed4d607489e9b87a4b0771ae2eb)
- [frequency analyzer support for 22-channel walkie-talkies](https://github.com/flipperdevices/flipperzero-firmware/commit/311da512114311bb543fe0ae1e94ec2c903fe355)
- [tune frequency during recording](https://github.com/flipperdevices/flipperzero-firmware/commit/a8de08f9e0f953a109b2ec1f320334231ea32e20)
- [use an external speaker/headphone on pin A6 and GND](https://github.com/flipperdevices/flipperzero-firmware/commit/d05e95b992b33564f4f0df2d38a3f0fb045ed3f9)
- [display the full frequency value](https://github.com/flipperdevices/flipperzero-firmware/commit/eb243bdf38377ca2641ab40a0ccc9f07da5c8f23)

## Frequency Anaylzer
1. Update your firmware to the above version or apply a [patch](https://github.com/flipperdevices/flipperzero-firmware/commit/311da512114311bb543fe0ae1e94ec2c903fe355).
2. Open "Sub-GHz" app.
3. Select "Frequency Analyzer".
4. Press **Talk** on the walkie-talkie.
5. The Flipper Zero should show the frequency of the walkie-talkie!  (Note: only channels 1-7 and 15-22 are supported by the Flipper Zero)

## Listening to 22-channel walkie-talkies
1. Update your firmware to the above version or update your [setting_user](https://github.com/flipperdevices/flipperzero-firmware/commit/49e95313e9730ed4d607489e9b87a4b0771ae2eb) file (in "sd card/subghz/assets"). Some firmware versions may require the file to be named setting_user.txt.
2. Open "Sub-GHz" app.
3. Select "Read RAW".
4. Select "Config".
5. Select the frequency of the walkie-talkie you want to listen to. This should be at the end of the frequency list (channels 1-7 or 15-21). Channel 22 can be heard on the last frequency.
6. Select the modulation (either "FM238" or "TLK1")
7. Select "Sound" to "ON".
8. If you are going to use the app for a long time, set "RSSI Threshold" to -75 (so static doesn't take up memory).
9. Press "Back".
10. Press "OK" (REC) to start listening.
11. If you "See the signal" but hear silence, try pressing LEFT or RIGHT to adjust the frequency slightly. (Requires firmware update or [patch](https://github.com/flipperdevices/flipperzero-firmware/commit/a8de08f9e0f953a109b2ec1f320334231ea32e20).  Be sure you are talking into the walkie-talkie loudly and clearly.

## Using an external speaker or headphone
1. Follow the above steps for listening to 22-channel walkie-talkies; except choose "EXT.A6" for "Sound" instead of "ON".
2. Connect an external speaker or headphone to pin A6 and GND. Be careful not to touch the pins together!

## Broadcasting
1. The frequencies for the 22-channel walkie-talkies are not supported in any region; DO NOT TRANSMIT.  See https://docs.flipper.net/sub-ghz/frequencies
2. If you save the .SUB file you can edit the frequency to a supported frequency and transmit.  Be sure to follow all laws and regulations for your region.
3. Open the .SUB file in a text editor.  Replace the top portion with the following...
```
Filetype: Flipper SubGhz RAW File
Version: 1
Frequency: 433920000
Preset: FuriHalSubGhzPresetCustom
Custom_preset_module: CC1101
Custom_preset_data: 02 0D 0B 06 08 32 07 04 14 00 13 02 12 14 11 83 10 A7 15 06 18 18 19 1F 1D 00 1C 00 1B 07 20 FB 22 10 21 56 00 00 C0 00 00 00 00 00 00 00
Protocol: RAW
```
  - NOTE: Replace the `Frequency:` above with a supported frequency for your region.
  - NOTE: This uses "12 14" `gfsk modulation`, instead of "12 04" `2-fsk modulation` to improve the likelyhood of compatibility with other devices.
4. Make sure there are NO blank lines between `Protocol:` and the `RAW_Data:` line.
5. Copy the file back to your SD card and load it in the "Sub-GHz" app.

