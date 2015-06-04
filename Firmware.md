# Introduction #

The firmware is designed in such a way that will allow it to take advantage of the special features the 5xxx processor family, which makes that series of chip well suited to engine control. This code is also intended to be easy to read such that it can be understood by anyone with basic C programming knowledge.

The firmware is in active development and makes use of subroutines written by FreeScale to support use of the ETPU co-processor(s) for high speed input and output functions. This means many functions do not have have source code provided, as they are provided by FreeScale as binary files. These parts of code do include data sheets and app notes that show how to interface with the via the PPC processor.

### Development Tools (only needed if you plan to help with the firmware development) ###
Here are some basic tools that are needed to work with this project.
  * [CodeWarrior](http://www.freescale.com/webapp/sps/site/overview.jsp?code=CW_SUITES&tid=CWH/) - The CodeWarrior demo package is the projects default firmware tool set.  This is a FREE download with a 128K binary size limit.

  * [P&E multilink universal](http://www.pemicro.com/products/product_viewDetails.cfm?product_id=15320137&CFID=6301609&CFTOKEN=d1546a00aca22d0c-A4995BD1-DAE6-FD1D-1014305795E749E2) -  This is a USB to jtag interface that wil let you connect, load firmware, and debug.  It costs $129.

  * [TRK=MPC5634M](http://www.freescale.com/webapp/sps/site/prod_summary.jsp?code=TRK-MPC5634M) An CPU demo board can be used known as the TRK-MPC5634M. It can be obtained for $99 and it provides everything you need for the processor - cpu board, compiler, debugger, flash burner, etc to get started with development. All you need is an IO board to interface it with injector, ignition, ect. Such an IO board is being developed here as well.

# Basic Details about this project #

Software for this project is typically licensed with the MIT license.
  * [MIT License](http://www.opensource.org/licenses/mit-license.php/)
Code is available at:
  * [Code](http://code.google.com/p/open5xxxecu/source/browse?repo=firmware&name=master#git%2Fo5e%2Fsrc)

### How to get started with TRK-MPC5634 dev board ###
  1. obtain $99 dev board
  1. obtain copy of code
  1. obtain and install free (as in beer) code warrior compiler
  1. attach via USB cable the dev board, USB will power it.

Now that you have all the core pieces, here are the basic steps that need to happen to get the code on the dev board.

  1. Open the o5e.mcp file, this should launch with Code Warrior.
  1. Click the green arrow bug button. If all goes well it will compile, generate a binary file to download. ![http://wiki.open5xxxecu.googlecode.com/git/programming_5634_dev_board/debug_graphic_marked.png](http://wiki.open5xxxecu.googlecode.com/git/programming_5634_dev_board/debug_graphic_marked.png)
  1. It will open the software for OSBDM, choose the MPC5634 rev 2 chip, and click connect. ![http://wiki.open5xxxecu.googlecode.com/git/programming_5634_dev_board/PEMicro_connection.png](http://wiki.open5xxxecu.googlecode.com/git/programming_5634_dev_board/PEMicro_connection.png)
  1. It will then download the compiled program.
  1. Once it's done, from the pull downs choose "execute" then "go" ![http://wiki.open5xxxecu.googlecode.com/git/programming_5634_dev_board/debugger_OK-GO.png](http://wiki.open5xxxecu.googlecode.com/git/programming_5634_dev_board/debugger_OK-GO.png)

If you see the LED1,2,3,4 blink, you have successfully installed the program and it is running on the dev board. Now develop something great. If you have any questions, comment or concerns, feel free to chat it up in the forums http://forum.open5xxxecu.org/index.php

### Normal opertion with TRK-MPC5634 dev board ###
If something goes wrong, one thing you'll have to keep an eye on is if the board is acting normally. Here are some things that are normal operation. I'll assume normal is with USB powering the board.
  1. JP9 connector has several voltages for reference. Referenced to GND, VDDA, 5V and VDDREG should all read about 5V. VRC33 should be about 3.3V (mind is currently 3.40V) and VDD should be 1.3V.
  1. The LED's are trying to tell you some basic things. Starting from left to right,
  * D1 dark orange, shows the 5634 is being provided power, if J1 is set to USB\_5V, the USB will provide 5V power to the 5634. Normally this LED is bright.
  * D2 orange this verifies that power is actually making it to chips on the board. This should be on when ever D1 is on, however if a trace burns up or some similar issue, D1 would be on, and D2 would be off. Normally this LED is bright.
  * D3 green, shows USB can power the board, this looks a bit dim, don't worry, as long as it's green, you have USB 5V.
  * D4 red, indicates that barrel connector can power the board. This LED does not mean that power is routed to anything on the board. See D1 and D2 to ensure the board is in fact powered. This is normally dim, when only powered by the USB connector.
  * D5 and D6 are normally dim. I don't currently know what they do.

  * LED1, 2, 3, 4, 5 and 6 are driven by the 5634. LED's 1-4 are used for diagnostics purposes and can means many things. At the time of writing this a one second pulse means that a certain function is operating well. LED2 is typically tells you that the crank angle code is synced and working correctly. Fast blinking means it lost sync and solid on or off means it's not operating at all. LED5 and LED6 are often connected to outputs like an injector. Such that you can see it get bright or dimmer as the injector is held on more or less.

  * D7, D8, D9 are used by reset signals. If you press the reset button, you will see all these LED's go bright. Also while programming, you'll see them blink from time to time. This indicates the programming device has reset the processor for one reason or another. All these lights should be dim for normal operation.

### What needs to be done and who's doing it ###
If things are going well and you want to get involved, take a look at this page. It lists some ideas of stuff that needs to be done, and who's doing what. Such that you can know how to get involved. http://code.google.com/p/open5xxxecu/wiki/Firmware_Tasks_List/

### Tables ###
If you want to learn a bit more, here's a page about how the tables are used. It can be valuable information. http://code.google.com/p/open5xxxecu/wiki/Tables/

### Block diagrams high level information ###
If you want to learn more, here's an attempt to break the code down into high level documentation, such that you can get a feel for how the hip bone and leg bone are connected.
http://code.google.com/p/open5xxxecu/wiki/Firmware_Overview