### STM32L Discovery Board firmware template

This is a template for quickly getting up and running with bare metal, pure open source, command line, firmware development for the [STM32L-DISCOVERY and 32L152CDISCOVERY](http://www.st.com/web/catalog/tools/FM116/SC959/SS1532/PF250990?sc=internet/evalboard/product/250990.jsp) boards on Linux. After installing a small number of packages, a simple `git clone` and `make` should produce working `.elf` and `.bin` files that can be flashed to the board.

The template supports and demonstrates use of all the main features of the board:
 
* LCD
* Capacitive sensors as 4 touch buttons or a touch slider 
* Green and blue LEDs
* User pushbutton
* A one second (by default) interrupt driven timebase based on the onboard accurate external clock crystal

#### Overview

This template is based on the free [STM32Cube library / framework from ST](http://www.st.com/web/catalog/tools/FM147/CL1794/SC961/SS1743/LN1897?icmp=ln1897_pron_pr_feb2015&sc=stm32cube-pr11). The initial project was generated with [STM32CubeMX](http://www.st.com/web/catalog/tools/FM147/CL1794/SC961/SS1743/PF259242?icmp=stm32cubemx_pron_prcube_feb2014&sc=stm32cube-pr) and the Makefile was generated with [CubeMX2Makefile](https://github.com/baoshi/CubeMX2Makefile). The STM32 TouchSensing library and Board Support Packages (BSP) were then manually pulled in and modified as necessary to cleanly expose the LCD, capacitive touch buttons, User button and LEDs.
 
The User button can be set up to trigger an interrupt or to be polled. By default it's set to trigger an interrupt. Polling is required for the capacitive sensors. A handy use for these boards is for various specialized timers, so the template includes an interrupt driven timebase, configured to trigger an interrupt once per second by default.

#### Versions

Library                | Version
---------------------- | ------
CMSIS / STM32Cube FW   | 1.4.0  
STM32 TouchSensing     | 2.1.1  
BSP / GLASS LCD        | 1.0.2  
STM32CubeMX            | 4.12.0 

#### Usage

* Install gcc-arm-none-eabi by following the instructions [on Terry Guo's Launchpad](https://launchpad.net/~terry.guo/+archive/ubuntu/gcc-arm-embedded). Note the special instructions for Ubuntu. 

* Then:

```
$ sudo apt-get install openocd git 
$ git clone <copy the URL from the top of this page>
$ cd stm32l-discovery-timer-template
$ make
$ openocd -f board/stm32ldiscovery.cfg -c "program build/stm32l1-disco-template.elf reset"
```

Tested on Linux Mint 17.3 64-bit.

**Note:** For the User button to work correctly, the `SB1` solder bridge on the board must be opened and the JP1 jumper must be set to the OFF position. This also disables the onboard power usage measurement circuitry, which removes the purpose for `SB2` and `SB14`. Those can then be removed as well, which opens up the PA4 and PC13 pins for normal use. 

That's it! With the included example, touching the capacitive sensor will cause a value corresponding to the location to be displayed on the LCD. The blue LED will toggle once per second and the green LED will toggle when the User button is pressed.

Prebuilt binaries are also included.

Check the [OpenOCD](http://openocd.org/) documentation for how to perform on-chip debugging from the command line.
