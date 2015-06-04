# Introduction #

The CPU used is the very powerful MPC5554 from FreeScale. It uses the PowerPC instruction set and runs at 132Mhz. Some support will also be provided for the lower powered MPC5634 and the industry's fastest - the 5 core, 180 Mhz MPC5676R MCU announced by FreeScale.

  * [FreeScale 5554](http://www.freescale.com/webapp/sps/site/prod_summary.jsp?code=MPC5554/)

The CPU board is the phyCORE®-MPC5554. Some support may also be provided for other boards.

  * [phytec SOM](http://www.phytec.com/products/som/PowerPC/phyCORE-MPC5554.html/)

The I/O board with injector drivers, ignition coil drivers, power regulation, etc, etc will be available soon. Current schematic is available at:

  * [The current PCB](http://code.google.com/p/open5xxxecu/downloads/detail?name=open5xxx%20hardware.pdf&can=2&q=/)

The SOM/processor/harness pin-outs

  * [Pin out](http://code.google.com/p/open5xxxecu/downloads/detail?name=2010_05_18_open_ecu_phycore_signals-ME_r14.xls&can=2&q=/)


# Details #

  * Who's working on what

| Action  | Name | Status | estimated completion |
|:--------|:-----|:-------|:---------------------|
| Layout sub circuits | Jared | in process | 10/15/11 ?           |


# Design concepts and theories #
Here are a couple links that detail some of the reasons why certain components where selected, and what we expect for their characteristics as a system.
  * [Injector drive theory](http://code.google.com/p/open5xxxecu/wiki/Injector_driver_theory) OVP MOSFET's are good, other are less optimal, this attempts to explain why.
  * [Analog / GPIO protection theory](http://code.google.com/p/open5xxxecu/wiki/AN_and_DIGI_Protection) This predicts our protections against direct rail shorts, and low pass filtering.