The easiest way is to start with a generic project file so:

Open TunerStudio, select “file”, “project”, “open project” and select “Open5xxxECU example”
Now select “file”, “save tune as”, and give your project a name.

The first thing to do is get your basic engine information entered.  To do this select:
“Engine Set-up” and you will see a list of the basic engine set-up parameters.  Generally you want to start at the top and work down the list.

**Cylinder Count** – select the number of cylinders your engine has

**Cylinder Position** – This is where you tell the ECU where each cylinder is.  There are a number of ways this can be configured but for most applications the easiest is say injector 1 is in cylinder 1, injector 2 in cylinder 2, etc and this is the method that will be covered in the manual.

First set cylinder 1 to “0”.  All other entries are relative to cylinder 1 and are the cylinder offset in crankshaft degrees from 0-719.  For example if you have 4 cylinders and your firing order is 1-3-4-2 then you would enter:

  1. – 0
  1. – 540
  1. – 180
  1. - 360

**Crank Position Setup** – There are 3 variables on this menu
  * The actual number of teeth on your crank wheel  (a 36-1 wheel has 35 teeth on it)
  * The number of missing teeth ( a 36-1 wheel has 1 missing tooth)
  * The cam lobe position relative to the missing tooth – this the number of crank degrees past the missing tooth where the cam signal should be expected.

**Engine Model** – This is probably something you haven’t seen in an ECU before and it’s important to how the Open5xxxECU works so it it’s worth explaining not only what it is but why it’s here and how it makes your life a lot easier.  [The Engine Model](http://code.google.com/p/open5xxxecu/wiki/EngineModel)

**Load Sensing Method** - [Load Sensing Method options](http://code.google.com/p/open5xxxecu/wiki/LoadSensingMethod)

**Max Expected Injection Time** -  [Max Expected Injection Time](http://code.google.com/p/open5xxxecu/wiki/MaxInjectionTime)

**Injector Dead Time Set** - enter the injector dead time at 13.7V - 1ms is a close guess most of the time.

**Injector Dead Time v Voltage Curve** - If you don't have data for your injectors, a guess is:

|Voltage(V)|Time Corr (%)|
|:---------|:------------|
|   16       |   -17       |
|   14       |     0       |
|   13       |    12       |
|   11       |    45       |
|   10       |    70       |
|    9       |   105       |
|    8       |   150       |

**Dwell Time Set** - Set the Coil dwell time at 13.7V. 3-4ms is a good guess but you really should measure  it.  [understanding Dwell Time](http://www.dtec.net.au/Ignition%20Coil%20Dwell%20Calibration.htm)

**Dwell Time Curve** - This is the change to Dwell time with Voltage and again should be measured.  A guess is to use the injector v voltage curve values.

**Enable Accel/Decel Corrections** - This should be disabled until you have the engine running properly at steady state conditions

**Rev Limiter Setup** - This is where you enable and setup the rev limiter
  * Rev limiter Type - The hard stop means the ECU shuts off the engine. A soft stop means the ECU begins to limit the engine's output to slow the rpm climb rate so the engine doesn't just shut down/turn on abruptly.

  * Rev Limit - this is the hard stop point where the will shut the engine off.

  * Soft Rev Limit is where the ECU begins to limit the engines output.  The power reduction is proportional from the soft limit to the hard limit so at the hard limit the engine is off.

**Staged Injection Setup** - Enable and configure staged injection if you have it.
  * Enable Staged injection - on/off
  * Load Stage point - % load to stage injectors
  * Load dead band - how far below on point to turn off, this eliminates cycling at the set point.
  * RPM stage Point - RPM to stage injectors
  * RPM dead band - how far below on point to turn off, this eliminates cycling at the set point.

**Idle Air Control** - Enable able and configure Idle Air control.
  * Configure/Enable IAC - off or IAC type. It's best to disable this until the engine is running properly at steady state conditions.
  * Warm Idle - set the desired normal idle RPM
  * Cold Idle RPM - set the desired cold idle RPM.  This is normally about 100-200 rpm above the warm idle rpm.
  * Warm Start IAC position - IAC position during warm cranking
  * Cold Start IAC position - IAC position during cold cranking
  * Warm Park IAC Position - IAC position when the warm engine is above idle load.  This prevents stalling on rapid throttle off.
  * Cold Park IAC Position - IAC position when the cold engine is above idle load.  This prevents stalling on rapid throttle off.


**Enable User Map Control** - this only used for ECU development work and should be  disabled normally.