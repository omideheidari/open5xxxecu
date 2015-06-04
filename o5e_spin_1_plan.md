# Introduction #

This is a plan/checklist for when new boards are ordered. Once they have been received, they should be tested and should have parts populated in a specific sequence. This page details the plan for how that should happen.


# Details #

  * PCB resistance
Once we get the board, we should verify several general things with a beep test multi meter. I would like to verify the grounds and power planes are connected as expected. Such that a multi-meter lead in the harness hole, will beep test to the other ground points of each chip. Then the same for 5V and 3.3V planes.

I would also like to run 1 amp through certain ground paths and measure the voltage drop is minimal. This test would tell us how good our ground path is. There are a couple injector circuits that may be a bit weaker than others in that they have less via's allowing for a ground path. One via is probably enough, but I'd like to see how strong they are. If it doesn't go well, we may have to de-rate a pin. For example Q519 has weaker GND via's. I'd like to verify the via's are good enough at 1amp draw. They probably are, but I wasn't able to over beef it like the others.

Required tools. Multi meter, computer access with KICAD schematic.

  * Harness
We should verify the harness connector should install. Via holes and such cut outs align correctly and such. Do not fully install, it will likely be hard to remove. Also verify we have extra length on the harness leads. The harness connector will be jacked up by about .1 inch off the PCB. The leads should still protrude past the PCB.

  * Regulator
Once that's done, I say start populating the power regulator circuits, doing it little by little looking for shorts and other issues. Start with the 7812, then the switcher, ect until the 5v and 3.3v are generating correct voltages. I'm tempted to do a load test at this point, to see how hot it gets under certain loads, but I also don't know a realistic load that this circuit will see. I'd mostly like to see how it reacts to ripple and such from the supply. Those might be a bit hard to test, and they are probably not a problem, so we can probably pass over that for now.

Measure several input voltages, and output voltages including 7812 output, 6V, both 5V's and the 3.3V. Provide up to 20V and down to 10.4V. Might as well see what it does as you go below 10.4. However any voltage below that either has bad wiring or, a dead battery.

Verify that shut down works correctly. Pull this high or low to turn on and off the regulators. See "EN PWR" on 40 pin header.

Required tools. Multi meter, adjustable voltage bench supply current limited to around 100 mA and producing a voltage up to 18V. Soldering iron and solder. Solder paste recommended.

  * Hello world LED
Populate the LED, and install the SOM. It will need some code or something to make the LED blink. This will verify the power supply is allowing the 5554 to boot up. It also verifies that we are handling the enable power thing correctly. Or I think it will verify that. I don't really know what's powered by the constant 3.3v circuit. I think that LED is driven via the 5V supply. We should get a diag firmware written that shows this test.

  * Serial coms.
Can someone fill this in a bit?

  * General input circuits
I say start on the input circuits, populate the parts for one of the general purpose protected pins. With the SOM removed, and with the input supply current limited to say .25A, we should test that these will handle a rail short to 12V. We should see only a couple mA pass, when shorted, and we should also measure that the SOM's input is below 5.5V and we should have a measurement of what ever it is. We may have to tune the 2.2k resistor to keep the voltages in acceptable boundaries. Also test how much - voltage we can sustain before the SOM input is below -.3V. I expect about 2.7V. I suggest we try this on two separate circuits before we consider the selected resistor values as good.

Again with SOM removed, we should put in 1V on the harness input, and measure that the SOM receives 1V, ect, up to 5V input. My primary concern is the diode leakage as you get close to 5V. For example, does 4.9V in, measure at 4.5V at the SOM? These tests will indicate if we need to put in different diodes or not. In PUMA we found the diode data sheet was flat out wrong, and it's leakage was much more than claimed in the data sheet, so we should verify what we expect to see here.

Next do the same for MIAT, ect. We should do similar to the above, shorting to the rails and - voltage checks. We'll need do additional testing to simulate the sensor with resistors or something. We'll want to put in a resistor or perhaps even a real sensor, then measure the voltage at the sensor, and the voltage at the SOM. We should make sure it appears to match.

Required tools. Multi meter, adjustable voltage bench supply current limited to around 100 mA and up to 1A and producing a voltage up to 18V. Soldering iron and solder. Solder paste recommended.

  * VR/hall
Next lets try the VR chip, they are small and can be a pain to install. Might as well do that early on. Perhaps we can have the VR pulse the LED per the input signal. I'd like to see the voltage levels of a fast rev'ed VR. We should see a pulsed input at the SOM. If all looks good, and we see appropriate voltage levels at the SOM, install the SOM and make it blink the LED.

Required tools. Oscope, bench power supply current set at 14.4V. Soldering iron and solder. Solder paste required, VR chip is hard to install with out it.

  * WO2
Then WO2. Connect a WO2, and have it change the LED blink rate base on it's input. A lighter under the sensor can change the O2 levels.

Required tools. Bench power supply current set at 14.4V. Soldering iron and solder. Solder paste recommended. Lighter or open flame, some way to vary the O2 level at the sensor. WO2 sensor.

  * Stepper
Finally start with the outputs, I say start with the stepper, it should be easy. We'll need a firmware that simply rotates CW and CCW, perhaps at various speeds. If we feel crazy, perhaps we can test half step ect as well.

Required tools. Bench power supply current set at 14.4V. Soldering iron and solder. Solder paste recommended. Stepper motor.

  * MAP
Then MAP, that should be easy, I know Mark has MAP experience. Same thing with the LED, blinks faster or slower based on pressure.

Required tools. Bench power supply current set at 14.4V. Soldering iron and solder. Solder paste recommended. Vacuum/pressure gauge.

  * Injectors
Finally, populate injector 01, and connect to an LED/resistor, not an injector. Have a firmware that pulses this on and off. Perhaps with a scope we can capture the CPU pin signal, and LED signal. I'm interested in the signal delay on the rise and falling edges.

Required tools. Oscope, bench power supply current set at 14.4V. Soldering iron and solder. Solder paste recommended. High impedance injector. Low impedance injector. LED test light.

Then do the same with an injector.

  * Misc high side and low side drives.
The the HS drive chip, and LS drive chip. These can be tested with a light or what ever. They aren't precisely timed or anything like that.

  * Knock.
I left Knock for last. I don't know how to test it, and I think the software will lag a bit here.

  * Finish it up
Now perform each of the above steps one at a time on each of the remaining injector, ignition, RPM input circuits, ect until everything is installed and passes the basic function tests. Remember any one of those circuits can have a short, or can be miss wired. So proceed with caution, and try to current limit your bench supply to 1A if you can.

  * Install connector
Do we dare to install the connector permanently now? Once it's installed it will be hard to access knock circuits, and some components of the VR input circuits. The VR's are likely good and well known to work. The knocks may be a bit more risky. It's probably not that big of a deal, as we'll probably develop ion sense before to long. So sure lets add the connector.

  * Notes
Each of these steps should have a BOM associated with ref designation.

I think there might also be steps in here that Jon or Paul want to test. I know there is the reset button, and dip switch thing. I'm not sure when/how those should be tested. When should communications com in to play, and how should we test them? Some of the test might be handy to see with hyper-terminal, putty or what ever.

How is Mark going to program the SOM?

Hmmm, how to connect and test without installing the harness connector.... We should find a way to do it with out solder. Solder in those holes will make it really hard to install the harness connector later on.