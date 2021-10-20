# Crkbd with Pimoroni Trackball
Portable keyboard and mouse companion.

![alt fullview][fullview]

This was inspired by [foureight84](https://github.com/foureight84/sofle-keyboard-pimoroni)'s sofle and [vlukash](https://github.com/vlukash/corne-trackpad)


## Materials
- [crkbd classic](https://www.littlekeyboards.com/collections/corne-pcb-kits/products/crkbd-classic-essentials-kit)
- [corne analyst case](https://www.littlekeyboards.com/collections/corne-cases/products/corne-analyst-case)
- [mbk choc low profile keycaps](https://www.littlekeyboards.com/collections/keycaps/products/mbk-choc-low-profile-keycaps)
- [kailh choc low profile switches (brown)](https://www.littlekeyboards.com/collections/keyboard-switches/products/kailh-choc-low-profile-switches)
- [mill max ultra lowprofile sockets](https://www.littlekeyboards.com/collections/miscellaneous/products/mill-max-ultra-low-profile-sockets)
- [sparkfun pro micro usb-c](https://www.sparkfun.com/products/15795)
 - If you are a beginner, please use the standard pro micro controllers. [Pro Micro](https://www.littlekeyboards.com/collections/mcus/products/pro-micro-black)
- [pimoroni trackball](https://shop.pimoroni.com/products/trackball-breakout)


## Build
I won't go over the details of the crkbd base build there is a lot of youtube clips and the orginal [foostan](https://github.com/foostan/crkbd) build is already detailed.

Instead of using the standard pro micro, I used the sparkfun pro micro usb-c controller and qwiic port to connect the pimoroni trackball as shown below.


### Qwiic Connection
Pimoroni Trackball with qwiic connection:
![alt trackball-qwiic][trackball-qwiic]

Trackball connected to the controller:
![alt controller-with-trackball-qwiic][controller-with-trackball-qwiic]

Controller connected to the board:
![alt connection][connection]

Right Hand Master:
![alt right-hand-master][right-hand-master]


## QMK firmware
I'm in no means a pro C/C++ developer and I'm sure once you see the code you could probably find improvements, but most of the development was originally from [foureight84](https://github.com/foureight84/qmk_firmware/tree/sofle_foureight84/keyboards/sofle/keymaps/foureight84), [drashna](https://github.com/drashna) and [sevanteri](https://github.com/sevanteri/qmk_firmware/tree/master/users/sevanteri).

Currently the common crkbd doesn't support i2c for the split keyboard communication, so I had to build the firmware similar to [vlukash](https://github.com/qmk/qmk_firmware/tree/master/keyboards/crkbd/keymaps) trackpad approach by having a firmware for the right and left controllers. Basically the master hand, the one that has the usb connected, will have the working code for the pimoroni trackball. So you have to explicitly say which is the master and enable the pimoroni trackball for that side only.

The firmware does not include the OLED driver mainly because I haven't tested it yet. The LED in the trackball will change indicating which layer you are currently at.

Clone my firmware here:
[https://github.com/greyhatmiddleman/qmk_firmware.git](https://github.com/greyhatmiddleman/qmk_firmware.git)

```
git clone --branch greyhatmiddleman https://github.com/greyhatmiddleman/qmk_firmware.git
```

~~The firmware is located in ```keyboards/crkbd/rev1/common/keymaps/greyhatmiddleman_trackball_[left|right]```.~~

The firmware is located in ```keyboards/crkbd/keymaps/greyhatmiddleman_trackball_[left|right]```.


### Firmware Customization
For this example the trackball will be connected to the right hand which will be the master side.

Here you want to make sure the following:

- __Important__: Make sure that both the ```config.h``` and ```keymap.c``` are identical under the right and left folders
- In the ```config.h``` file make sure to define the master:
```
...
#define MASTER_RIGHT
...
```
- Change your keymaps accordingly and update any layer state switch cases accordingly.
- Set the ```rules.mk``` based on the which hand is master and that has the trackball connected.
 - For the __right hand__ ```rules.mk``` set the following:
 ```
 ...
 PIMORONI_TRACKBALL_ENABLE = yes
 ...
 ```
 - For the __left hand__ ```rules.mk``` set the following:
 ```
 ...
 PIMORONI_TRACKBALL_ENABLE = no
 ...
 ```
- Verify all your changes and confirm that the ```config.h``` and ```keymap.c``` are the same in the right and left holders.
- Now you ready to flash the firmware.

### Flashing Firmware
Connect the usb to right hand controller and run the following:
```
qmk flash -kb crkbd -km greyhatmiddleman_trackball_right
```

Next connect the usb to the left hand controller and run the following:
```
qmk flash -kb crkbd -km greyhatmiddleman_trackball_left
```

Connect the usb to the right hand controller then to your computer and you should be all set!


## Using the Sparkfun pro micro USB-C controllers
When using the sparkfun pro micro usb-c controllers you will need to break the VCC bridge on the back of slave/left hand controller. As shown in the image below.
![alt left and right hand controllers][left-right-controllers]

When you need to update the firmware on the slave/left hand controller, you will need to jump the 5v and RAW pins prior to connecting the usb cable.

Remember to load the firmware on these sparkfun controllers, you need to press the reset button twice. Refer to sparkfuns documentation for more details.

I found this article when troubleshooting the sparkfun controllers:
[Only one half of my keyboard works at a time, but not when they are both connected](https://docs.splitkb.com/hc/en-us/articles/360010588860-Only-one-half-of-my-keyboard-works-at-a-time-but-not-when-they-are-both-connected)

## Features
- Pimoroni Trackball reference core source code located in the drivers/sensor path
- OLED enabled by default, must have OLEDs on both sides.
- Added keycode BALL_LC: Mouse left click
- Added keycode BALL_SCR: While held down mouse will act as a vertical scroll wheel

## Troubleshooting
- You might want to use the standard pro micro controllers rather than the sparkfun pro micro usb-c ones. There are some differences with the controllers and it will not follow some of the standard documentations.
- Confirm that the ```config.h``` and ```keymap.c``` are the same under the right and left folders.
- Ensure that you connected the trackball to the master hand, which would be defined under the ```config.h```
- The ```rules.mk``` should be different between the right and left folders. It should dictate which has the Pimoroni enabled which the other side should have it disabled.

## Future Improvements
- Convert the controllers using the Nice!Nano ones for wireless connectivity
- Add an OLED on the opposite side of the trackball
- Try to get EE_HAND working so that we can have a single firmware

<!-- -->
<div>
<blank>
<a href="https://www.buymeacoffee.com/whmiddleman" target="_blank"><img src="https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png" alt="Buy Me A Coffee" style="height: 41px !important;width: 174px !important;box-shadow: 0px 3px 2px 0px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 3px 2px 0px rgba(190, 190, 190, 0.5) !important;" ></a>
</blank>
</div>
<-- -->


[connection]: https://raw.githubusercontent.com/greyhatmiddleman/crkbd-pimoroni-trackball/main/images/connection.jpg
[controller-with-trackball-qwiic]: https://raw.githubusercontent.com/greyhatmiddleman/crkbd-pimoroni-trackball/main/images/controller-with-trackball-qwiic.jpg
[fullview]: https://raw.githubusercontent.com/greyhatmiddleman/crkbd-pimoroni-trackball/main/images/fullview.jpg
[right-hand-master]: https://raw.githubusercontent.com/greyhatmiddleman/crkbd-pimoroni-trackball/main/images/right-hand-master.jpg
[trackball-qwiic]: https://raw.githubusercontent.com/greyhatmiddleman/crkbd-pimoroni-trackball/main/images/trackball-qwiic.jpg
[left-right-controllers]: https://raw.githubusercontent.com/greyhatmiddleman/crkbd-pimoroni-trackball/main/images/left-right-controllers.jpg
