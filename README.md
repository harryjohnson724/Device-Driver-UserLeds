# Device-Driver-UserLeds
This device driver will create a sysfs entry to control some onboard gpio user leds on Beaglebone Black.
Device tree method is used to enumerate the leds

Dependenicies
 1. Board used is Beaglebone blakc Rev C model.
 2. The driver is meant to be used with kernel 5.4 
 3. Distro is debian. version 10.3-IoT (download link is given below)

Tools required
1. kernel 5.4 souce here : https://github.com/beagleboard/linux/tree/5.4
2. download debian images for beagle bone black from here : https://beagleboard.org/latest-images
3. Cross compiler for ARM. I'm using a tool chain called gcc-armv8l-linux-gnueabihf from linaro
   get it here : https://www.linaro.org/downloads/
3. an application like gparted to flash the linux image on to the board

How to Run
1. Use the make file provided to get the .ko file. (Give path to the Kernel source in KERN_DIR variable before doing make)
2. I have directly given the dtb(devic tree binary) which you can copy to the boot partition of the board
3. I have also given the original dtsi file
	1. you can copy this to <kernel_source_directory>/arch/arm/boot/dts/
	2. execute this from kernel source directory : make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- am335x-boneblack.dtb
	   This will create the dtb file which you can then copy to the board. Use either step 2 or step 3
4. Copy the .ko file to any location in the board.
	1. You can use the makefile itself to copy the file. use 'make copy-drv' (copy-dtb for dtb file)
5. copy the .dtb file to the boot partition of the board. 
	1. You might have to mount the boot partition before copying.
	2. Reboot after copying. Kernel should automatically load the dtb during boot time
	3. check for the devices in /sys/devices/platform/  - you should see bone_gpio_devs
6. Now you can load the driver using insmod
	1. driver should print the gpio devices detected when insmod is executed
	2. if the driver is loaded successfully, you can see the devices here : /sys/class/bone_gpios/
	3. we can control upto 3 user leds in the board with this driver(user-led 0,1 and 2)
	4. Example: go inside an led: ex: /sys/class/bone_gpios/usrled1/
	   you can see the current configruation of the led (direction and value)
	5. use echo 1 > value to turn on the led and 0 to off.



	
