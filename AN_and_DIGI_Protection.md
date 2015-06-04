# Introduction #

This ECU will often be installed by an experimenter or a race mechanic, which means it's not possible to maintain controls that are often found in the assembly of electrical devices. To prevent issues, the design has to be done such that several potential conditions are acceptable on a field installation.

The processor can handle certain voltage levels and current in each pin.
In this case, because end users will likely make their own harnesses, that means protecting harness pins against being directly connected to the +12V battery rail, as well as the GND rail. The AN/DIGI signals can be ANalog inputs or DIGItal inputs/outputs, so they need to be protected against currents sourced by the processor. They can not drive to much current, or sink to much current. It's also likely that a person will touch the wires in the harness for one reason or another, so it's important to ensure good human body model ESD protection. Those are the conditions talked about in the below.

# Basic requirements #

In datasheet MPC5554.pdf table 2, it notes the 5554 is rated for up to 5.5V and down to -.3V. Maximum DC pin input of +/-2mA, and max AN pin input of +/-3mA. Also table 9 notes analog input voltage needs to be VDDA + 0.3, and we plan for VDDA to be 5.0V. So the planned voltage limits are -.3 and up to 5.3V, I'll assume the input impedance is 1 Mohm to gnd, such that a 5.3V signal won't exceed the 2mA input current.

In datasheet MPC5554.pdf table 5, it notes the 5554 is rated for a 2000V human body model. The human body model can go up to 8000V, so this rating is a bit on the light side. This also notes it was only pulsed once. This makes sense as assembly of a board using this chip could get that pulse during assembly. Once installed in an OEM vehicle, this ESD rating is no longer a concern. However this application that could have multiple ESD, so this rating might be on the weak side for for many of the applications we expect this will be used on. So additional ESD protection is probably a good thing. Also there will be components between the processor and harness pins, and those parts can benefit from ESD protection at the harness, such that voltage spikes and pulse currents aren't absorbed by the components in the middle.

The human body model ranges from 2000V up to 8000V with a 100pF cap dumped via 1.5k resistor into one pin of your DUT while all other pins are grounded. In reality, all other pins aren't grounded, so the MFG test is often not very good for a real world application.

# General Design guidelines #

Snubbing an ESD spike at the source is the best way to prevent damage. ESD contains very high voltages, high currents, and has a large spectrum of energy. By first snubbing it at the source, you dampen or limit the energy that moves through the circuit. Such that ESD protection in a processor chip, or a secondary chip will then have to absorb less energy. Also note, that an ESD surge will pass several components as it propagates to the processor. If ceramic or tant caps are in that path, they will typically short and absorb much of that energy before the surge makes it to the sensitive components. Shorting via ceramic or tant cap will often cause a small crack. These small cracks can grown given time for thermal stressing, or repeated exposure to more ESD surges. These cracks are bad as they change properties like ESR and on occasion capacitance, which warps analog readings at the processor, while your sensors are correct. So it's a good idea to limit that kind of stress, to prevent incorrectly warped signals from being read by the processor.

Diodes start to conduct long before saturation. A .7v saturated diode when given a limited current, can be limited to .3 of voltage drop. This is done by knowing the voltage you expect to have, and limiting with a resistor.

# Front side harness ESD protection #

Littlefuse makes some interesting ESD chips. The SP720, shunts current when 1to2V above or below the rails, and when not conducting it's leakage current in around 1nA with a 1pF capacitance. So this chip will shunt ESD current, while not modifying the original signal much at all.

# Rail connected protection #

The ESD protection chip won't prevent accidental shorts to the rails, as it only conducts when outside the rails. Schottky diodes are a good idea for this, as they are rugged and easily available. By using a series resistor, you can also limit the current such that they do not allow an excess in voltage to pass to the processor. For the BAT54 diode, a current of about 1mA will drop about .3V.

# How it all works out, in simulation #
![![](http://wiki.open5xxxecu.googlecode.com/git/12V_short.png)](http://wiki.open5xxxecu.googlecode.com/git/12V_short.png)

This circuit simulates a low pass filter and short circuit protection, to a simulated processor pin. It notes a reverse sustained voltage of about 2.7V, and a top rail voltage of about 5.3 when shorted to the +12V rail.

Note the lower graph shows incorrect voltages. The diodes are set for a .3V junction, but are still conducting at .7V. It's probably a learning curve issue on my part. With a .3V drop across the diode, it would go up to 5.3 not 5.7 as shown.

![![](http://wiki.open5xxxecu.googlecode.com/git/AN_protect_AC-sim.png)](http://wiki.open5xxxecu.googlecode.com/git/AN_protect_AC-sim.png)

This is an frequency simulation, and indicates the amplitude is 99.9% accurate  while the signal to be looked at is under about 1kHz. It also notes the signal is shifted by about 10 degrees when under 1kHz. As seen in the DC simulation, the response is on the order of .1mS, so we don't have to be concerned about the shift, as signals like a temperature sensor's are significantly slower than that. However, if we tried to use this circuit for a knock input, we'd be buggered by shifted signals. That's one reason why you would want to use secondary knock sensor chip, or a PGA instated of the processor ADC. This circuit is fine for general purpose signals.