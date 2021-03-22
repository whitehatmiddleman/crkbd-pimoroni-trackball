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
- [pimoroni trackball](https://shop.pimoroni.com/products/trackball-breakout)


## Build
I won't go over the details of the crkbd base build there is a lot of youtube clips and the orginal [foostan](https://github.com/foostan/crkbd) build is detailed.

The only thing special is I used the sparkfun pro micro usb-c controller and qwiic port to connect the pimoroni trackball, shown below.

Pimoroni Trackball with qwiic connection:
![alt trackball-qwiic][trackball-qwiic]

Trackball connected to the controller:
![alt controller-with-trackball-qwiic][controller-with-trackball-qwiic]

Controller connected to the board:
![alt connection][connection]

Right Hand Master:
![alt right-hand-master][right-hand-master]


## QMK firmware
I'm in no means a pro C/C++ developer and I'm sure once you see the code you could probably find improvements, but most of the development was originally from [foureight84](https://github.com/foureight84/qmk_firmware/tree/sofle_foureight84/keyboards/sofle/keymaps/foureight84) and [sevanteri](https://github.com/sevanteri/qmk_firmware/tree/master/users/sevanteri).

Currently the common crkbd doesn't support i2c for the slip keyboard communication, so I had to build the firmware similar to [vlukash](https://github.com/qmk/qmk_firmware/tree/master/keyboards/crkbd/keymaps) trackpad by having a firmware for the right and left controllers.



[connection]: https://raw.githubusercontent.com/greyhatmiddleman/crkbd-pimoroni-trackball/main/images/connection.jpg
[controller-with-trackball-qwiic]: https://raw.githubusercontent.com/greyhatmiddleman/crkbd-pimoroni-trackball/main/images/controller-with-trackball-qwiic.jpg
[fullview]: https://raw.githubusercontent.com/greyhatmiddleman/crkbd-pimoroni-trackball/main/images/fullview.jpg
[right-hand-master]: https://raw.githubusercontent.com/greyhatmiddleman/crkbd-pimoroni-trackball/main/images/right-hand-master.jpg
[trackball-qwiic]: https://raw.githubusercontent.com/greyhatmiddleman/crkbd-pimoroni-trackball/main/images/trackball-qwiic.jpg
