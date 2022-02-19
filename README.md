# vnmtwo
 - tested on Raspberry Pi 3 Model B V1.2 
 - Raspbian 10 (buster) x64
# syspwm
Python Libary for Hardware PWM on Raspberry Pi using Linux kernel driver and /sys interface 

This is a python library that allows control of the limited number of PWM pins available on most Raspberry Pi hardware. It does via a Linux kernel PWM driver that comes with recent kernels (at least version 4.9).

Using it to, for example, control servo motors leads to very smooth control, which I wasn't able to get with RPi.GPIO on a Raspberry Pi Zero W.

**Enabling PWM driver:**
1. Edit /boot/config.txt and add the following:  `dtoverlay=pwm-2chan`
1. Reboot
1. Check that driver is loaded: `lsmod | grep pwm_bcm`
    1. Output should be something like `pwm_bcm2835             2711  0`

**Using code:**

Place `syspwm.py` in the same directory as your code, and include it like `from syspwm import SysPWM`

The following code uses syspwm to  smoothly turn a servo through its turning radius. Tweak the duty cycle values of S, E, and M to cause your servo to go to its start, end, and middle positions, respectively.
````python
        from time import sleep
        import atexit
        SLEE=3
        FREQ=20

        #pwm0 is GPIO pin 18 is physical pin 12
        pwm = SysPWM(0, FREQ)
        atexit.register(pwm.disable)
        pwm.enable()

        while True:
                pwm.set_duty_cycle(0.3)
                print 0
                sleep(SLEE)
                pwm.set_duty_cycle(0.5)
                print 90
                sleep(SLEE)
                pwm.set_duty_cycle(0.9)
                print 180
                sleep(SLEE)
````
I learned the /sys interface for hardware PWM from http://www.jumpnowtek.com/rpi/Using-the-Raspberry-Pi-Hardware-PWM-timers.html
