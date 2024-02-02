# klipper-config

CHANGE THIS BEFORE EDITING THE REAL THING IN MAILSAIL

current: (take new photo if necessary)

![image](https://cdn.discordapp.com/attachments/1109794418808070181/1171392630295363624/IMG_20231107_181505.jpg?ex=655c8366&is=654a0e66&hm=5d8acb28575f04c8952ebb86eece0acd9427b323c684acbcea9d9ae9390e58d8&)

## PINS

https://github.com/bigtreetech/Octopus-Max-EZ#ocotpus_max_ez-pinout-table

https://github.com/bigtreetech/EBB/tree/master#pinout-2

## note on tuning stallguard: 
Stallguard: 
 1) Disable Stealthchopper (set thershold 999999)
 2) Homing Speed >= 100
 3) Dont Do Current near to 3.000, too close to the limit causes breakdown due to current sometimes
Rotation Distance:
  1) Pitch * Gear Count
  2) Usually Pitch is 2 (2mm for most belts)

## canbus procedure:
1. turn on printer (u can skip step 3 and 4 as it is auto configured by systemd and udev now)
2. if after boot of lattepanda, type `sudo service systemd-networkd start`
3. type `sudo ip link set can0 txqueuelen 1000 up type can bitrate 1000000` in terminal
4. uuid should show up in `~/klippy-env/bin/python ~/klipper/scripts/canbus_query.py can0`
5. click connect / restart in mainsail

## make menuconfig

for klipper (main board): copy klipper.bin to sd card and rename to firmware.bin, red light should flicker rapidly for 1 second, wait for about 1 minute
![image](https://cdn.discordapp.com/attachments/1170003460054319154/1172046971515707413/IMG_20231109_133602.jpg?ex=655ee4cd&is=654c6fcd&hm=3754f8f2463071f9816b71cf0c5720f1116d3726748299be8114391203fc03eb&)

for canboot/katapult (ebb board): put into dfu mode, put jumper on usb 5v, connect to usb, then type `sudo dfu-util -a 0 -D ~/CanBoot/out/canboot.bin --dfuse-address 0x08000000:force:mass-erase:leave -d 0483:df11` (error message means success)
![image](https://cdn.discordapp.com/attachments/1170003460054319154/1172046933104279632/IMG_20231109_133445.jpg?ex=655ee4c4&is=654c6fc4&hm=ca1977ed8ba0b0e6f2ab465ed146c76e661a01ff6a94741b2d4eff991f563170&)

for klipper (ebb board): connect can cable, put jumper in 120R, type `~/klippy-env/bin/python ~/klipper/scripts/canbus_query.py can0` to find the uuid, then type `python3 ~/CanBoot/scripts/flash_can.py -i can0 -f ~/klipper/out/klipper.bin -u MYUUID`
![image](https://cdn.discordapp.com/attachments/1170003460054319154/1172066185307770900/IMG_20231109_145214.jpg?ex=655ef6b2&is=654c81b2&hm=d5a715c4805ab866c3de8b67f01634db3e9106311834f0c8b5be9d2ae8208b16&)
reference: https://github.com/maz0r/klipper_canbus/blob/main/toolhead/ebb36-42_v1.1.md

if frick up klipper, dont need to flash canboot again, just press reset twice and it will enter bootloader (flashing red light means its in canboot)
## auto config can:
/etc/network/interfaces.d/can0 is outdated; neet to use systemd-netword instead

https://www.freedesktop.org/software/systemd/man/latest/systemd.network.html#%5BCAN%5D%20Section%20Options

instructions on systemd-neteworkd: 
https://maz0r.github.io/klipper_canbus/extras/systemd-networkd.html

## make ebb working:
flash canboot onto ebb: https://github.com/maz0r/klipper_canbus/blob/main/toolhead/ebb36-42_v1.1.md

use this config? https://github.com/maz0r/klipper_canbus/blob/main/toolhead/example_configs/toolhead_btt_ebbcan_G0B1_v1.1.cfg

