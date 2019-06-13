# Traditional helicopter setup

## Disclaimer
We ported INAV on traditional helicopter for ourselves. Use it on your own risk! RC helicopter may be very dangerous. We suppose you know what you do. We mean that you are able to build, to configure and to fly RC helicopter without INAV or any other simillar products. According to laws of many countries RC model with autopilot may be considered as UAV. You are responsible of following your local laws.

## Links
If you are new to INAV, please read [Getting started](Getting%20Started.md).

Please read [safety guide](Safety.md) before turning on RC model.

Also we recomend you to read about [the board you own](Boards.md).
Please make sure that your board supports as much [motors and servos](ESC%20and%20servo%20outputs.md) as you've got on your helicopter.

## Flashing INAV
**WARNING!** Disconect all ESC's and servos from board or disconect separate power if possible before flashing!

Traditional helicopter supported by INAV started [firmware](https://github.com/iNavFlight/inav/releases) version 2.3.0.
1. Download [INAV configurator](https://github.com/iNavFlight/inav-configurator) for your OS.
2. Switch your board to DFU mode (board usualy has got a button for it or if there is already INAV, CleanFlight or BetaFlight on your board make CLI command "dfu")

![CLI DFU](assets/images/heli_setup/cli_dfu.png)

3. Run INAV configurator (under Linux you may need root previlege). You should see in dropdown list "DFU".

![DFU](assets/images/heli_setup/dfu.png)

4. Open "Firmware Flasher" tab.

![Firmware Flasher](assets/images/heli_setup/firmware_flasher.png)

5. Chose your board and firmware version.

![Board and firmware version](assets/images/heli_setup/board_firmware.png)

6. Push "Load Firmware [Online]" button. After download is complete check "Full chip erase" and push "Flash Firmware" button.

![Checkboxes](assets/images/heli_setup/erase_chip.png)

![Firmware buttons](assets/images/heli_setup/buttons_firmware.png)

## Basic mixer setup
**WARNING!** Disconect all ESC's and servos from board or disconect separate power if possible before setup!

1. Chose correct serial port and push "Connect" button.

![COM port](assets/images/heli_setup/com.png)

2. Go to "Mixer" tab and in "Platform configuration" dropdown list choose "Helicopter".

![Mixer helicopter](assets/images/heli_setup/mixer_heli.png)

3. At the time of writing this document there is one configuration of CCPM suported (CCPM 120). Choose corresponding CCPM configuration. If pitch servo on your helicopter is behind swashplate then choose "CCPM 120" mixer preset. If pitch servo on your helicopter is in fornt of swashplate then choose "CCPM 120 Inv" mixer preset (see pictures below). Push "Load mixer" button.

![CCPM 120](assets/images/heli_setup/ccpm120.png)
![CCPM 120 Inv](assets/images/heli_setup/ccpm120_inv.png)

4. Select CCPM Pitch/Roll weight (50% is good start value, you will select more accurate value later). This value represents the relation between servo move for cyclic pitch and servo move for collective pitch (If you want to know more about this value read [this pull request](https://github.com/iNavFlight/inav/pull/4701)). If your helicopter has got a tail motor instead of tail rotor blades pitch control then check corresponding checkbox. Push "Create mixer" button.

![Additional settings](assets/images/heli_setup/additional_settings.png)

5. Push "Save and Reboot" button.

![Save and reboot](assets/images/heli_setup/save_reboot.png)

6. Use PWM generator to determine min and max servo positions. For CCPM servos positions determined by swashplate ability to move along the shaft.

6.1. Using PWM generator set swashplate to the lowest horizontal position. Note resulted PWM values for each servo.

![CCPM low](assets/images/heli_setup/ccpm_min.png)
![CCPM low PWM](assets/images/heli_setup/ccpm_mini_pwm.png)

6.2. Using PWM generator set swashplate to the highest horizontal position. Note resulted PWM values for each servo.

![CCPM high](assets/images/heli_setup/ccpm_max.png)
![CCPM high PWM](assets/images/heli_setup/ccpm_max_pwm.png)

6.3. If your helicopter has got tail servo then using PWM generator set min tail rotor pitch, max tail rotor pitch and middle position (tail rotor pitch at which helicopter stabily hangs and doesn't rotate). Note PWM value for each case.

![Tail min](assets/images/heli_setup/tail_min.png)
![Tail min pwm](assets/images/heli_setup/tail_min_pwm.png)

![Tail max](assets/images/heli_setup/tail_max.png)
![Tail max pwm](assets/images/heli_setup/tail_max_pwm.png)

![Tail mid](assets/images/heli_setup/tail_mid.png)
![Tail mid pwm](assets/images/heli_setup/tail_mid_pwm.png)

For helicopter on the pictures:
- CCPM back servo bottom position 1930, top position 1300.
- CCPM left servo bottom position 1140, top position 1770.
- CCPM right servo bottom position 1900, top position 1250.
- tail servo left yaw position 1820, right yaw position 1260, normal flight position 1430.

7. Open "Servos" tab in configurator. Servo 3, servo 4 and servo 5 are CCPM servos as shown on picture on "Mixer" tab.

![CCPM 120 scheme](assets/images/heli_setup/ccpm120_scheme.svg)

Fill corresponding fields with PWM values, where "MIN" column should be filled with numerically smaller values and "MAX" column - with numerically larger. If PWM servo value for bottom position is higher than PWM servo value for top position check "Reverse" checkbox for such servo.

CCPM servos **MUST** move linear in all range from min to max. So we must calculate middle point values for CCPM servos using next formula

*MID = (MIN + MAX) / 2*

For tail servo middle write middle position PWM value in "MID" column, numericaly smaller PWM value - in "MIN" column, numericaly larger - in "MAX" column. If numericaly larger value corresponds to right yaw then check "REVERSE" checkbox.

Push "Save" button.

![Servos PWM](assets/images/heli_setup/servos_pwm.png)

## Common setup
Sensors setup is similar to any other RC aircraft. Check [this guide](INAV_Fixed_Wing_Setup_Guide.pdf) for instructions.

From now we suppose that all sensors, GPS(if you have one), OSD(if you have one) and RC receiver of your helicopter are setted up and calibrated.

## Motor setup
We descript brushless elecriс motor setup because it is common type of motor on RC helicopters.

First of all you need to setup transmitter rates (if your transmitter supports it). Rates of your transmitter shoud be in range 1100-1900 us. You can found current transmitter rates on "Receiver" tab (transmitter should be turned on).

![Transmitter rates](assets/images/heli_setup/transmitter_rates.png)

If you can't setup rates on your transmitter by some reason then go to "CLI" tab and input next commands:
- set min_check = 1100
- set max_check = 1900

1100 represents min rate, 1900 - max rate. Choose these values according to rate chart on "Receiver" tab.

You need to know min and max PWM values for motor. If these values are unknown then remove motor from helicopter, connect to ESC motor and PWM generator. Find out minimum PWM value at which your motor rotates and subtract 30 us from it (this reserve guarantees motor to stop). Max value take 2000 us.

We suggest you to use ESC compatible with [BlHeli](https://github.com/bitdump/BLHeli) because it is cool. If you using BlHeli or any other configurable ESC then take these values 1050 and 2000. Configure your ESC to use 1080-2000. In our case we are using BlHeli so we took 1050-2000.

Go to "Configuration" tab. Input min and max values inside "Minimum Throttle" and "Maximum Throttle" respectively. "Minimum Command" take equal "Minimum Throttle" minus 50.

![Min max throttle](assets/images/heli_setup/min_max_throttle.png)

## Checking correctness of setup
To this moment your sensors, servos and motors should be configured and ready for tests. Now you may connect motors and servos to helicopter.

**WARNING! Remove blades before tests! (Tail blades too!)**

Now you should determine max refresh rates your servos and ESC support. BlHeli supports almost all of ESC protocols. Choose correct one for your ESC (if not sure then choose "STANDARD"). Choose max ESC refresh rate and servo refresh rate your ESC and servos support. Check "Enable motor and servo output" checkbox.

![ESC/Motor Features](assets/images/heli_setup/esc_motor_features.png)

Push "Save and Reboot" button.

### Servo checking
**WARNING! Disconnect motor from ESC!**

Now you may correct CCPM Pitch/Roll weight. Slowly tilt cyclic pitch stick diagonaly and make sure that swashplate doesn't stuck. Check it for all 4 diagonals. If swashplate stucks then you must reduce CCPM Pitch/Roll weight.

Example of stucked swashplate:

![Stucked swashplate](assets/images/heli_setup/stucked_swashplate.png)

For testing servos you need to tilt cyclic pitch stick and check position of swashplate. Swashplate should be tilted corresponding the stick.

Left view:

![Stick up](assets/images/heli_setup/stick_up.png)
![Pitch up](assets/images/heli_setup/pitch_up.png)

![Stick down](assets/images/heli_setup/stick_down.png)
![Pitch down](assets/images/heli_setup/pitch_down.png)

Front view:

![Stick left](assets/images/heli_setup/stick_left.png)
![Roll left](assets/images/heli_setup/roll_left.png)

![Stick right](assets/images/heli_setup/stick_right.png)
![Roll right](assets/images/heli_setup/roll_right.png)

If your helicopter has got tail rotor blades pitch control servo then tilt yaw stick and check tail rotor blades pitch changes respectively.
