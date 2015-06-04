# Introduction #

This covers some extreme basics of how to use o5e. The below should result in a basic understanding of how o5e connects and works. It covers the basic items needed to get the pieces all together and doing something.

# Details #

The Open5xxxECU system consists of 3 main parts:

  * Hardware - the ECU itself and its sensors
  * Firmware - the software that is stored in the ECU
  * Tuner - the software the runs on your PC to allow monitoring and adjusting the ECU

Below is a basic overview of how all three of these components interact and an overview of the extreme basics of using them.

# So I got the TRK-MPC5634 dev board, now what? #

  * Download and install CW version 2.10 from this link (TODO add links)
  * Downlaod and install TortiseGIT (TODO add links)
  * Check out o5e http://code.google.com/p/open5xxxecu/wiki/Git_for_Windows (TODO add pictures and more notes)
  * Switch to mark\_5634 branch, or develope\_branch. (TODO add notes about what branch and why)
  * Download and install JRE 6 (not 7 or newer) (TODO add links and notes)
  * Download and installed TunerStudio 2.08 (TODO add link)
  * Open the folder where ever you checked it out to and go src/o5e/tuner and copy"o5e example" (if it's not there then you are in the wrong branch!) and paste the file into my Documents/TunerStudio Projects.
  * Downloaded the updated firmware on TKR board via P&E Micro's update utility more found at this wiki http://code.google.com/p/open5xxxecu/wiki/OSBDM_com_port

Finally you should have all the critical dev pieces. This should get much better with future hardware, for now the above is what's required for this dev board. Next check basic communications.

  * Find the com port in device manager I'll call it COMX. (TODO add pictures and additional instructions)
  * If you prefer putty, open Putty on COMX The communications link is 115200 8-n-1. You might want to set the session to local echo, such that when you type a Q you see that it actually was typed, and was in fact capital. You should get this "QMShift 2.111          "
  * Open TS, click pull down "communications" then "settings" now make sure it specifies the 115200 and COMX. Then click "Test Port". This will cause TS to send the Q command, and look for the reply noted above in the putty command.
  * Once this is working you can click "Accept" to get into normal TS, it takes a couple seconds some times to do it's thing, so you might have to count to 10 before it start doing some stuff. It should communicate with board a bunch, and eventually show a bunch of gauges and such. At this point you can start playing with TS.
  * Now install a jumper wire to connect the crank gen and cam gen (TODO add pictures)
  * Now click "develop" button then "test RPM setup" under here you can change most parameters of TS like RPM TPS, MAP and all the major variables of the engine controller. You should see them update on your gauges.

At this point you now have the basis up and running. Feel free go dive into things and chat it up in the forums. Enjoy :)

# Troubles? Looky here. #

If you still have troubles, visit the forums found here http://open5xxxecu.org/index.php People there will likely help you either in the forum, or via PM / e-mail. If you post for help in the forum, please include the below info in your post, e-mail, or PM.

  * What PC operating system do you have?
  * What is the OSBDM firmware version noted by FreeScale utilities?
  * What TunerStudio version do you have?
  * What JRE version do you have?
  * What Com port is the OSBDM on noted in Device manager?
  * What version of o5e and branch do you have?
  * Have you successfully gotten the o5e firmware to install?
  * Did you type go in the debugger, or power cycle the dev board?
  * What is the status of the LED's on the dev board?
  * What is the physical layout, items like USB connected vs TTL, or what jumpers/jumper wires are installed.