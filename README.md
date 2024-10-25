# __MODS:__

## Macro Output Pin Plugin

Provides the following additional Klipper configuration section (printer.cfg)

### [macro_output_pin]
(original plugin: https://github.com/randellhodges/klipper-plugins/tree/master)
Creates a output pin, that execute gcode if value changesme created configuration.

Only the subset of configuration values as noted below are supported. Because gcode is executed on a value
change, the utility of `shutdown_value` is questionable and may get removed (or changed) in the future.

### Examples
```
# Example of a sliding scale element
[macro_output_pin test_scale]
pwm: true
scale: 255
value: 0
shutdown_value: 0
gcode:
  {% set value = printer['output_pin test_scale'].value | float %}
  { action_respond_info("TEST SCALE: %.4f" % (value)) }
```

```
# Example of an on/off element
[macro_output_pin test_bool]
value: 0
shutdown_value: 0
gcode:
  {% set value = printer['output_pin test_bool'].value | float %}
  { action_respond_info("TEST BOOL: %s" % (value)) }
```

## AVR / Timer1 Pwm Frequency changeable

For the Arduino Nano/Uno and identical MCUs, the frequency for the Timmer1 can be adjusted to control the fans with a higher frequency than the standard frequency.
The frequency must be set via 

```
make menuconfig
```
 
# __Installation:__

Install first 'main' image form 't800-sonic_lcd_uart0-1.0.6.43.51.zip'
https://github.com/CalDymos/Creality_Sonic_Pad_Firmware/tree/main/imgs

Then copy Config.ini and t800-sonic_lcd_ab_1.0.6.54.20-Mod.swu from the zip archive 'Creality_Sonic_Pad_V1.0.6.54.20-Mod'
into the root directory of a USB flash drive. 
Insert the USB flash drive into the Creality Sonic Pad, and it will automatically pop up a window to prompt the upgrade.

 
