# Introduction #

The o5e firmware (software loading into the 5xxx processor flash memory) is designed to run on a FreeScale  MPC5xxx processor (5554/5634 preferred) and control an internal combustion engine.

The FreeScale MPC5xxx processors are specifically designed for combustion engine control and contain multiple mechanism in one chip to help unload the main processor. This includes a 2 core processor with a sub processor known as an eTPU (enhanced Time Processing Unit), the eDMA (enhanced Dynamic Memory Allocation), eMIOS (enhanced Modular Input/Output Subsystem) as well as Analog/digital converters, internal flash and ram, serial and CAN transceivers.

O5e firmware uses CocoOS, a non-preemptive micro OS. So you may need to spend a little bit of time getting the basic idea from http://www.cocoos.net/ (although we don't use many of the features).  One of the primary developers was more comfortable with it after they wrote two little tasks/threads that had printf statements which produced debugging information in Codewarrior on their PC and saw it work.  Reading "main()" in file "main.c" in the o5e firmware source repo, is also a helpful starting point. That file helps give an overview of the system.

This operating system combined with several physical parts, can control a variety of combustion engines. Below is a basic layout of how the engine signals flow around the engine it controls, which is then followed by the firmware program flow which show how the code allows the signals to flow.




# Signal Flow #
In the below picture it notes the basic signal flow with a 5634 demo board connected to a PC (or PLX gauge), and an IO board that allows interfacing with a real engine.
![![](http://wiki.open5xxxecu.googlecode.com/git/SIGNAL_FLOW_DIAGRAMS/5634_signal_flow_diagram.png)](http://wiki.open5xxxecu.googlecode.com/git/SIGNAL_FLOW_DIAGRAMS/5634_signal_flow_diagram.png)
The dia document that generated the above can be found here. http://wiki.open5xxxecu.googlecode.com/git/SIGNAL_FLOW_DIAGRAMS/5634_signal_flow_diagram.dia

## Programming signal flow path ##
To program this, you'll need a windows PC with Codewarrior and a USB cable. You need to check out the project, then open it with codewarriror, then click the compile button, then the debug button. When you click debug, it will download the firmware via USB and the OSBDM chip that's included with the demo board. Once the firmware is flashed, you can step through code, or allow full speed execution of code.

## Communications signal flow path ##
Communications are done either with TunerStudio (TS), or with a serial streaming device like the PLX guages / Putty / Hypertermal. Using TS and a serial cable, you can upload tuning tables for fuel and/or spark, as well as read information from the 5634's internal memory including RPM, fuel pulse widths, temperatures ect. TS can simulate sensor inputs by activating certain diagnostics features. If the diagnostics information is activated and you specify RPM, it will cause the 5634 demo board to generate a simulated CRANK and CAM signal, which can be physically jumper connected to the RPM input pins. By changing other parameters like TPS or MAP you can simulate an engine's operation, and verify specific fuel and spark calculations. All with out having any additional hardware.

## Analog signal flow path ##
Signals like MAP, TPS, temperatures ect, are typically sent to memory via DMA. So the analog readings do not cause processing stress on the PPC or ETPU. There is some direct usage of the ADC, mostly for diag and initialization however typical operation is when the DMA simply updates the appropriate bits of internal memory. These bits of analog memory are then used as part of the PPC maintenance schedule, to update other system variables, or as information sent via serial stream.

## PPC signal flow path ##
The PPC includes a real time Operating System (OS) that performs engine variable maintenance and system communications. Periodically the OS reads a variety of system variables from the internal memory, it reads information from the tuning table stored in memory, and proceeds to update other system variables. For example, the OS reads system variables like MAP, RPM, ect, then it looks up in the tuning tables what fuel pulse should be used. Once it knows the fuel pulse, it then writes the pulse length to the fuel channel system variables. After this maintenance has been performed, it allows the OS to use the additional CPU cycles for what ever the OS tasks deem necessary. It repeatedly does this maintenance again and again in less than 1mS.

While the system is idle in between the maintenance intervals, the OS passes information from the memory tables to communications devices like the UART which typically results in a display via TS.

## ETPU signal signal flow path ##
The ETPU code is provided by FreeScale and used as an pre-compiled object file. At the time of writing this, the compiler for ETPU code is not available for no $, so in the spirit of free as in beer, we are keeping the costs down by using the pre-compiled code that is available for no $. Because of this the exact signal flow is kind of unknown. The source code can be obtained under NDA, but can't be published or copied. What we know, is that in the ETPU processor, receives pulses on a couple pins, and updates the internal memory with specific information like RPM and CRANK ANGLE. FreeScale also provided a function for generating CRANK and CAM signals that operates in this same ETPU. For that code, you specify RPM and such information, then it generates the proper pulses on a pin. You can verify your RPM information by installing a jumper between the tooth gen pin, and the tooth decode pin. If you specify 1kRPM and read 1kRPM, things are working fairly well.

## RPM decoder / crank angle signal flow path ##
There is a piece of memory that is shared with the PPC and ETPU. The ETPU is configured to look for a skipped tooth wheel pattern on a specific set of pins. Once the ETPU syncs on this set of pins, it will update the memory with the crank angle, RPM, as well as diagnostics information like a sync signal. This information can then be read by the PPC maintenance routines, or sent to TS/PLX for gauge displays.

## Spark and Fuel signal generation ##
The ETPU also directly controls the fuel and spark IO. It reads the crank angle and determines if a fuel or spark event should start. It also checks rapidly if a fuel or spark pulse is has come to an end. It updates the IO pins accordingly.




# Firmware Program Flow #
The general program flow involves several initialization routines, that setup an OS which executes a list of tasks. These tasks have interwoven executions that from a signal flow perspective, makes them appear to function at the same time. The operation of these tasks will update and maintain the sub processor (ETPU) such that the sub processor executes the time critical stuff like spark and fuel signals.

## PPC program flow including initialization ##
This part is executed from power on reset and is primarily for the PPC part of the processor chip. It sets pins to be inputs or outputs, it sets clocks, uploads the ETPU firmware, and all the stuff that's needed for normal operation. Below is the program flow diagram that helps graphically represent how the code is executing in the PPC processor.

![![](http://wiki.open5xxxecu.googlecode.com/git/SIGNAL_FLOW_DIAGRAMS/o53_program_flow_INIT.png)](http://wiki.open5xxxecu.googlecode.com/git/SIGNAL_FLOW_DIAGRAMS/o53_program_flow_INIT.png)The DIA document that generated that can be found here http://wiki.open5xxxecu.googlecode.com/git/SIGNAL_FLOW_DIAGRAMS/o53_program_flow_INIT.dia

## PPC program flow including OS tasks and normal operation ##
Once the above initialization has taken place, the PPC processing is  handed over to the OS. The OS operates several tasks, with various levels of priority. The tasks are prioritized such that the task on top, is highest priority. However each task has wait or equivalent calls that hands execution back to the OS and allows other tasks to execute. The result is that from the signal perspective, it appears that several signals flow in and out seamlessly.

Below is a graphic that shows the various tasks and there flow as it was done in a specific branch and commit found in the firmware repository. Note that in the bottom right-ish there are some notes that will likely be of interest to someone who's interpreting the graphic for the first time.

![![](http://wiki.open5xxxecu.googlecode.com/git/SIGNAL_FLOW_DIAGRAMS/o53_program_tasks.png)](http://wiki.open5xxxecu.googlecode.com/git/SIGNAL_FLOW_DIAGRAMS/o53_program_tasks.png)
The DIA document that generated that can be found here.
http://wiki.open5xxxecu.googlecode.com/git/SIGNAL_FLOW_DIAGRAMS/o53_program_tasks.dia

## ETPU program flow ##
Exact program flow for the code that's located in the ETPU is unknown, as it's provided by FreeScale as a compiled file. For signal flow and such information look at FreeScales app notes AN3768 - eTPU Automotive Function Set, AN3769 - crank angle, AN3770 - Fuel function, AN3771 - Spark function.




## Final notes ##
That should do for a basic overview. Hopefully it will help quickly indicate how signals flow in and out of the system, as well as how the code executes such that it allows those signals to flow. Feel free to develop more features that we can added to the above graphs, and feel free to chat about it in the forums found here. http://forum.open5xxxecu.org/index.php