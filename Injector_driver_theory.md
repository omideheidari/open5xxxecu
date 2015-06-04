# Introduction #

I'm sure many folks have seen the Snubbed or "Freewheel" diodes used to control injector voltage spikes. These diodes work for basic situations, but aren't optimal, and require extra work to install and maintain. For about the same price as any other MOSFET, one can get OVP MOSFET's like VNS14NV04 (SMT) or VNP28N04 (thru hole), which are much better than a snubber-ed MOSFET and are recommended for most purposes. This page will talk about and explain many aspects of using OVP devices as inductive driver(s). The below simulations can be found at this link https://github.com/jharvey/Cinch_enclosure_template/tree/master/simulation, they are done with QUCS, a free simulation tool.

# Diode snubber topology #

Here's a screen shot of a QUCS simulation that shows details of the Snubber diode topology.

[http://wiki.open5xxxecu.googlecode.com/git/Snubbed\_HighZ\_injector\_simulation.PNG](http://wiki.open5xxxecu.googlecode.com/git/Snubbed_HighZ_injector_simulation.PNG)

Some key items to note, the low side injector voltage stays at or under 16V, which is kind of nice, and the collapsing magnetic field (which generates the injector current after it's turned off) is completely dissipated by the resistance in the coil of the injector. Adding to the pre-heat of the fuel, which pushes you closer to detonation limits. An energy calc for the OVP MOSFET approach (noted below) indicates the injectors average stored energy at 9kRPM is on the order of 3 watts per injector. Now go touch your 3 watt night light, and get a gut feel for how much it's pre-heating your fuel. Also note the time from when the MOSFET turns off the injector, to the time when the injector stops delivering fuel is about 6mS. The pintle falls at approx 1/3 amps draw for a high Z injector. Notice the decay rate at this point, is on the lower sided of the exponential decay, which means small variations in the decay caused by slight variations in inductance due to heating, corrosion in a wire harness, ect will cause a larger tolerance on control of when fuel stops flowing. In this case I don't think that +/-.5mS is unreasonable to expect and +/- .25mS is likely. With a 6mS pulse, and .5mS variation the fuel delivery error is about 4%.

This 6mS decay time, and 6mS ramp up time, means that simply turning on and off the injector will take about 12mS. If delivering a 6mS long fuel charge once per revolution, then your min full length duty cycle, would be 18mS. Which equates to about a 3,300 max RPM for accurate delivery ((1rev/.018S)x60S/min). If you start to rev faster than this, the injector will either loose the 6mS window for accurately delivering fuel, causing a lean condition. Or you'll start to overlap the off and on ramps, loosing precision of your delivered charge, which can also result in a lean condition. Typically algorithms simply dump in more fuel to play it safe, as a lean condition under full load and RPM makes for a short time before you melt engine parts. Lean fuel is bad because it won't take the cylinder heat away quick enough, and causes chamber failures. One solution is to simply stop delivering fuel on a per revolution basis,  and try to inject based on an average fuel consumption, not a per rev consumption. If running a 4 stroke engine, you can inject during that other cycle, which allows for a much longer fuel pulse. However you are no longer know precisely what charge you have in the cylinder and you are making some assumptions about what actually made it into the chamber. Also note, that if you are delivering on a per rev basis like in a 2 stroke, and with 12mS to turn off and on the injector, at 9kRPM you could have just dry fired two rev's, or lean fired 2 revs and dry fired 1 rev. This 12mS window is not a good thing. So lets look at how OVP MOSFET help with that problem.

# General info desired from an injector #

So how much energy is there in an injector? First we'll need to ball park how much inductance there is in a typical injector. Take a look at this spead sheet to see list that's been compiled from several different injectors. https://github.com/jharvey/Cinch_enclosure_template/blob/master/docs/injector_mH_averages.xls?raw=true

When this page was created this spread sheet noted a max for a high impedance injector at 32.8mH 15.9ohms, A low of 7.9mH 11.8ohms. It appears it will be common for 12mH and 12 ohms.

The more mH, the more energy you will store in a magnetic field. The ohms is also important, as it controls the charging curve.

Now lets take a moment to remember the time it takes for one rotation at various RPM's.

12kRPM = .00500 seconds/rotation

9kRPM = .00666 seconds/rotation

6kRPM = .01000 seconds/rotation

3kRPM = .02000 seconds/rotation

So when sizing your injectors, you probably want something around a 9kRPM max rev, for a full load flow pulse of up to around 6.6mS. That's an aprox figure, for talking purposes. It's not for sizing your injectors, as you have to consider many items like 2 or 4 stroke, sequential, or not, valve opened time, ect. For talking purposes I'll assume the 6.6mS pulse is in the ball park, however on a non sequential 4 stroke, that max pulse delivery could be 0.00333S. So that pulse could vary by quite a bit.

# A 70V OVP MOSFET #

So now that we've gone over some flaws of the snubber diode approach, and we have a basic set of values establishing a range of pulse times, and injector electrical properties, how can we control them better? Here's one approach.

Here's a picture of a QUCS simulating the 70V OVP MOSFET driving a the same injector modeled above. This simulation used a worse case injector, that's a bit worse off than the largest mH found in the above spread sheet, so it should dissipate more heat, and take longer than most injector to open/close. It's used as a baseline to show the variations from one design to another.

![![](http://wiki.open5xxxecu.googlecode.com/git/70v-ovp_highz_injector_simulation_4.2-watts.png)](http://wiki.open5xxxecu.googlecode.com/git/70v-ovp_highz_injector_simulation_4.2-watts.png)

Note the injector decay is linear, and takes about .0005S to collapse. If you multiple the voltage and current, you'll discover the MOSFET is dissipating an average 35 watts for .0005S each time the injector is pulsed. The OVP region of the pulse isn't determined by the injector inductance or resistance, so it's accuracy is increased from 4% likely to under .1%. That should be true for about any OVP MOSFET. One down side is that 70V may exceed the injectors spec, or it may cause internal stresses, as these are designed as 12V devices. So it's overall performance is much better, but the high voltage may be a problem, and it technically violates typical electrical code. When below aprox 50V most electrical codes like NFPA70 consider it a safe voltage. So a lower voltage would be nice to reduce injector stress, and increase safety a bit.

# The 40V OVP MOSFET #

Introduce the 40V OVP MOSFET. Here's a picture of the same QUCS simulating the **40V** OVP MOSFET driving an simulated injector.

![![](http://wiki.open5xxxecu.googlecode.com/git/40v-ovp_highz_injector_simulation_4-watts.png)](http://wiki.open5xxxecu.googlecode.com/git/40v-ovp_highz_injector_simulation_4-watts.png)

Note this injector decay is linear, and takes about .001S to collapse. If you multiple the voltage and current, you'll discover the MOSFET is dissipating an average 20 watts for .001S each time the injector is pulsed. It's constant at 40V, and the current starts at 1A and in a linear fashion drops to 0A. So an average of .5A. Therefore 40V x .5A=20W of OVP heat. Again the accuracy is in the range of .1%.

Lets look at this another way to double check that the decay seems about right. From here http://en.wikipedia.org/wiki/Inductor#Stored_energy we get .5(.035H)(1A^2)= .0175j. Then from here http://en.wikipedia.org/wiki/Watt#Definition and assuming we dissipate at a constant 20 watts, the decay time would be .0175j / 20W = .0008S. Which is about what I expected and see in the simulation.

At 9kRPM that would mean if the injectors are injecting on a per rev basis, it would rotate/pulse 150 times in 1 second. So it would be in OVP decay for 150cycle/second x .001seconds/cycle=.15 seconds per one second of rotation. So 15% of the OVP heat is dissipated on average. 15% of 20W is about 3 watts total heat caused by the OVP MOSFET decay per injector channel. Per Fred's notes in the DIYEFI forums, it's not unreasonable to see 257 injections per second, which turns into 25.7% on time, which is 5.14 watts per injection channel.

The 40V OVP MOSFET noted above, moves that 6mS down to under 1mS, and the 70V OVP moves the off time to .5mS, where the Snubber diode was 6mS. This changes your full length duty cycle from 18mS to under 13mS, and changes your max accurate delivery RPM to about 4,600RPM (the 70V OVP is at 4800RPM). This increases the precision of your delivered fuel charge. This increased accuracy means you don't waste as much fuel, you don't risk system failures causing hidden lean conditions, and you don't have to guess as much when under full load and full RPM.

The trade off, can your injector handle 40 or 70V potentials? I haven't heard of one yet that can't. So sure it can handle it with out breaking immediately. However with my lack of long term reliability data, I'd recommend a 40V instead of a 70V to increase the long term reliability. A 70V is probably just fine, but I don't have empirical data to back it up with. If you get the accuracy out of a 40V OVP, that's what really matters, and the extra 200RPM of accurate control is kind of minor, when many successful builds can do it with snubber diodes.

The OVP MOSFET circuit dissipates the clasping field current in the ECU, not the injector, which reduces the fuel pre-heating issues caused with the snubber approach.

So why does the datasheet note uS and nS for the MOSFET's on and off times, when we measure mS at the actual device? That's because when the injector pushes the voltage up to 40V (Or what ever you **O** ver **V** oltage **P** rotection (OVP) is set for) The OVP will leak some voltage to the gate, turning it on slightly, causing the MOSFET to look like a resistor, not an open or short circuit. This temporary resistance will vary, causing the current to decay in a linear fashion instead of an exponential decay. The OVP limit will establish the rate of decay, not the switching time of the MOSFET. A fast switching time decreases the signal delays and such. The MOSFET's Ton and Toff times are still important, as it determines the system delays from when the injector is commanded to when it reacts. So faster devices are preferable.

So why not use an IGBT, after all they have a higher isolation voltage? I haven't seen an IGBT with low resistances like a MOSFET. This increase resistance decreases the amount of energy the IGBT can dissipate, and slows the off times, approaching the snubber diode in terms of off delay times. It seems common place for that IGBT's to drop about 1V at 5 amps. Which turns into a steady state resistance heat load of 5 watts, vs the above noted MOSFET's at .035 watts. While the OVP IGBT's will have similar tolerances in the decay, the internal resistances will slow the rise time, stretching out that 6mS rise, and will limit the decay, such that the 1mS MOSFET would be more like 1.1mS in an IGBT.

So in general OVP MOSFET's are faster as an overall system, they remove heat from the injector, and into the ECU, and they offer lower resistant heating characteristics, decreasing thermal management concerns.

JZ Comments

While I agree with OVP MOSFETs for injectors, I'll attempt some counter points.

1) After doing the calculations, it looks to me like the number of btus related to "fuel heating" is completely inconsequential as compared to the heat sources already present (like combustion).

2) A normal diode is a poor choice for quick closing, but a bidirection TVS provides the same speed as an OVP MOSFET by allowing the voltage to rise.

3) Different injectors yield very different performance.  For example, a < 2 msec rise time (probably < 1 msec opening time) with an injector I tested.

4) Low inductance injectors and to a smaller extent, higher voltages could be used for even faster times.