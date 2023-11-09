# klipper-config

CHANGE THIS BEFORE EDITING THE REAL THING IN MAILSAIL

current: (take new photo if necessary)

![image](https://cdn.discordapp.com/attachments/1109794418808070181/1171392630295363624/IMG_20231107_181505.jpg?ex=655c8366&is=654a0e66&hm=5d8acb28575f04c8952ebb86eece0acd9427b323c684acbcea9d9ae9390e58d8&)

note: 
Stallguard: 
 1) Disable Stealthchopper (set thershold 999999)
 2) Homing Speed >= 100
 3) Dont Do Current near to 3.000, too close to the limit causes breakdown due to current sometimes
Rotation Distance:
  1) Pitch * Gear Count
  2) Usually Pitch is 2 (2mm for most belts)

canbus procedure:
1. turn on printer
2. type `sudo ip link set can0 txqueuelen 1000 up type can bitrate 1000000` in terminal
3. uuid should show up in `~/klippy-env/bin/python ~/klipper/scripts/canbus_query.py can0`
4. click connect / restart in mainsail

auto config can:
/etc/network/interfaces.d/can0 is outdated; neet to use systemd-netword instead

https://www.freedesktop.org/software/systemd/man/latest/systemd.network.html#%5BCAN%5D%20Section%20Options

instructions on systemd-neteworkd: 
https://maz0r.github.io/klipper_canbus/extras/systemd-networkd.html
