# o5e Hardware: #

## General Description ##
  * This is a hardware platform that uses open5xxxecu software. This version is intended to include the typical features desired for engine control, for most applications. It has a fair bit of flexibility in the IO and is fairly rugged against issues that can arise in an experimental environmental. The intent of this spin, is to create a hardware platform such that people can get involved with this project. Allowing them to  develop software or expansion devices that attach to the flexible IO options.
  * This is a MIT open source project, which means you don't have to worry about an arm chair lawyer(s) claiming you violated a licence agreement because you didn't fully read paragraph 9 on page 93 of the GPL licence.
  * This version of o5e hardware is a first generation piece of hardware, and it's in a development cycle at the time this is being written. It uses **C** ommon  **O** ff  **T** he  **S** helf (COTS) devices when possible, to aid modular design. Such that a circuit can be tested now, while allowing future spins to move everything onto one PCB board.
  * The brain is a 5554 from FreeScale found a Phytec 5554 SOM. It includes the 5554 and some other components. Using this device prevent the need to solder the 5554 BGA chip. Devices like this remove the need to solder complicated SMT components. While this does use SMT parts, they are fairly easy to install.
  * There are also two 40 pin IDE style headers that allow access to most IO. This allows other brains to potentially attached to this IO board, in lieu of the 5554 SOM.

##### Status #####
  * The o5e spinout, is the first spin of the o5e hardware. The first set of boards has been ordered, as of 11/13/11.
  * We expect the first engine to be run from this hardware some time in  December of 2011.


##### General Spec #####
  * 5554 processor, that includes two ETPU's, such that there are three processors in one chip.
  * Sequential fuel and spark 12 cyl native.
    * 12 - 4 amp capable Low side injector drivers support with auto-protected FET's. (24 INJ channels total)
    * 12 - 1 amp capable Low side injector drivers support with auto-protected FET's. (24 INJ channels total)
    * 12 Low side ignition drivers, power driver FET's are external. Increasing isolation voltage.
  * 3 MAP sensors 1st MAP is the typical MAP, 2nd allows left and right manifold pressures, the third is ambient pressure. These can be internal to the ECU, or external with electrical wires.
  * 5 general purpose 1 amp low side driven outputs for fuel pump, O2 heaters, indicator light, ect.
  * 3 general purpose 1 amp high side driven outputs for 12V from ECU devices that could include fan's, fuel pump, ect.
  * 2 engine RPM inputs for both VR and hall sensors
  * 4 wheel RPM inputs for both VR and hall sensors, allowing for traction control.
  * Expandable via flexible IO, including 14 general purpose analog input, or digital output, 2 CAN busses, 1x232 port, and an extra pin on the harness for what ever signal you might need.
  * Stepper motor controller included native, with COTS module.
  * Schematic and Layout under MIT licence with KICAD for 6 layer, 2 oz copper PCB, fits in a LE ModICE Cinch enclosure. [PDF schematic](http://hardware.open5xxxecu.googlecode.com/git/docs/Open5xxxECU_schematic.pdf)
  * All digital and analog IO are protect against many abnormal conditions, making this system very rugged. Conditions include most ESD conditions, shorting GPIO/AN to battery/gnd, CPU source loading, and IO are frequency limited to prevent noise issues.


##### General Layout #####
  * A 3D rendering of it from KICAD found [here](http://wiki.open5xxxecu.googlecode.com/git/cinch_start_w-case.png)
![http://wiki.open5xxxecu.googlecode.com/git/cinch_start_w-case.png](http://wiki.open5xxxecu.googlecode.com/git/cinch_start_w-case.png)
  * A 3D rendering of it from Solidworks found [here](http://wiki.open5xxxecu.googlecode.com/git/som_top_view_3.jpg)
![http://wiki.open5xxxecu.googlecode.com/git/som_top_view_3.jpg](http://wiki.open5xxxecu.googlecode.com/git/som_top_view_3.jpg)
  * A pictorial snap shot of the PCB layout found [here](http://wiki.open5xxxecu.googlecode.com/git/layout_top_view_5.PNG)
http://wiki.open5xxxecu.googlecode.com/git/layout_top_view_5.PNG
  * A pictorial snap shot of the PCB layout found [here](http://wiki.open5xxxecu.googlecode.com/git/molex_connectors_iso.PNG)
http://wiki.open5xxxecu.googlecode.com/git/molex_connectors_iso.PNG
http://wiki.open5xxxecu.googlecode.com/git/iso_model.PNG
http://wiki.open5xxxecu.googlecode.com/git/iso_model_enclosure_hidden.PNG

##### Specific Hardware #####
  * CPU Prototype board : Phytec 5554 SOM [PDF](http://www.phytec.com/pdf/manuals/L-484e.pdf)
  * Schematics w/ reference layout: Found here [Git repo download page](http://hardware.open5xxxecu.googlecode.com/git/docs/Open5xxxECU_schematic.pdf)

##### open5xxxecu Main Site #####
  * Go here for more information and more concrete information: [code.google page](http://code.google.com/p/open5xxxecu/) or the [Home page](http://open5xxxecu.org/)