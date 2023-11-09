# klipper-config

CHANGE THIS BEFORE EDITING THE REAL THING IN MAILSAIL

current: (take new photo if necessary)

![image](https://cdn.discordapp.com/attachments/1109794418808070181/1171392630295363624/IMG_20231107_181505.jpg?ex=655c8366&is=654a0e66&hm=5d8acb28575f04c8952ebb86eece0acd9427b323c684acbcea9d9ae9390e58d8&)

## note on tuning stallguard: 
Stallguard: 
 1) Disable Stealthchopper (set thershold 999999)
 2) Homing Speed >= 100
 3) Dont Do Current near to 3.000, too close to the limit causes breakdown due to current sometimes
Rotation Distance:
  1) Pitch * Gear Count
  2) Usually Pitch is 2 (2mm for most belts)

## canbus procedure:
1. turn on printer
2. type `sudo ip link set can0 txqueuelen 1000 up type can bitrate 1000000` in terminal
3. uuid should show up in `~/klippy-env/bin/python ~/klipper/scripts/canbus_query.py can0`
4. click connect / restart in mainsail

## make menuconfig

for klipper (main board):
![image](https://cdn.discordapp.com/attachments/1170003460054319154/1172046971515707413/IMG_20231109_133602.jpg?ex=655ee4cd&is=654c6fcd&hm=3754f8f2463071f9816b71cf0c5720f1116d3726748299be8114391203fc03eb&)

for canboot/katapult (ebb board):
![image](https://cdn.discordapp.com/attachments/1170003460054319154/1172046933104279632/IMG_20231109_133445.jpg?ex=655ee4c4&is=654c6fc4&hm=ca1977ed8ba0b0e6f2ab465ed146c76e661a01ff6a94741b2d4eff991f563170&)

and then flash klipper onto ebb board thru canboot, go to ~/klipper and make menuconfig same as the first image for main board except communication interface canbus PB0 PB1
## auto config can:
/etc/network/interfaces.d/can0 is outdated; neet to use systemd-netword instead

https://www.freedesktop.org/software/systemd/man/latest/systemd.network.html#%5BCAN%5D%20Section%20Options

instructions on systemd-neteworkd: 
https://maz0r.github.io/klipper_canbus/extras/systemd-networkd.html

## make ebb working:
flash canboot onto ebb: https://github.com/maz0r/klipper_canbus/blob/main/toolhead/ebb36-42_v1.1.md

use this config? https://github.com/maz0r/klipper_canbus/blob/main/toolhead/example_configs/toolhead_btt_ebbcan_G0B1_v1.1.cfg

