# Introduction #

The OSBDM that comes with the TRK-MPC5634M includes an option to be a serial link for programs like Tuner Studio. However this is not enabled by default. To get it you will need to update the OSBDM firmware, and install some drivers on you PC. Below is a wiki about how to do this.

## To upgrade the JM60 OSBDM firmware on a TRK-MPC5634M board: ##

---

  * Note this did not work on a Win7 64 bit more here http://forum.open5xxxecu.org/viewtopic.php?f=63&t=293&start=22

---

1) Downloaded & installed the P&E utility from http://www.pemicro.com/osbdm/index.cfm
  * Top left link, after login you get to download. Note bugmenot.com is likely helpful for the account thing.

2) Check your current firmware to make sure you need an update Note this is an old firmware.
![![](http://wiki.open5xxxecu.googlecode.com/git/OSBDM_pictures/osbdm_VERSION.png)](http://wiki.open5xxxecu.googlecode.com/git/OSBDM_pictures/osbdm_VERSION.png)

3) Install a jumper to J35, up near LIN and barrel connector power supply.

4) Plug in the board using the USB connector only
  * unplug any other P&E Multilinks or Freescale boards
  * Note, no LED's light up, it will look like it did when it was not connected.

5) Run 'P&E Firmware Updater Utility' which is now in the start menu /PEMicro/P&E Firmware Updater

6) Your settings should be like this
![![](http://wiki.open5xxxecu.googlecode.com/git/OSBDM_pictures/PE_firmware_upload_settings.png)](http://wiki.open5xxxecu.googlecode.com/git/OSBDM_pictures/PE_firmware_upload_settings.png)

7) Click update firmware and wait for a minute or two.

7) Now re-check the firmware version as noted in step 2) you should get something like this
![![](http://wiki.open5xxxecu.googlecode.com/git/OSBDM_pictures/osbdm_VERSION_after_updated.png)](http://wiki.open5xxxecu.googlecode.com/git/OSBDM_pictures/osbdm_VERSION_after_updated.png)

At this point you have the updated drivers, so the dev board is ready to work with the PC. Next you'll need to install the PC drivers as noted below.

## PC Drivers ##
### For both windows and Linux, ###
Download and install the driver. Download from the PE page noted above, it's just below the firmware download button in a area titled "install driver". Then do the below based on what platform you are using.

### For windows, ###
Then launch putty setting the serial to 115200 8 data bits, no parity, 1 stop bit and of course with the correct COM port. Once putty is connected type "Q" and you should get a human readable reply. If you do, then you have the drivers installed correctly.

### For Linux, ###
install "screen" then you should be able to fire off the command below in a terminal window.

screen /dev/ttyACM0 115200

Now type Q and you should get a human readable reply. If you do, then you have the drivers installed correctly.