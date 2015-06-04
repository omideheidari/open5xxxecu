Load sensing is how the ECU knows how much power you are asking the engine to produce at the current rpm and form that the amount of fuel that is required.  The load information is used in a very simple way where the Engine Model is the wide open throttle or 100% load condition and is 100% fuel flow, then 50% load is 50% fuel flow, 10% load is 10% fuel flow.  Together % Load and the Engine Model form the foundation of the how the ECU calculates fuel flow requirements.

The ECU supports 3 methods of determining the engines load condition:

  * **MAP** – Manifold Absolute Pressure (speed-density)
  * **TPS** – Throttle Position Sensor (alpha-n)
  * **MAF** – Mass Air Flow

Each method has strengths and weaknesses. When properly configured any of the methods will work on any engine, but generally 1 will work better because its strengths better suit the application.

**MAP** – This is probably the most commonly used because it works well and is probably the easiest to set up.  This method works on the very simply principle that air density is directly proportional to absolute pressure so for a naturally aspirated engine 0kPa = 0% load, 50kPA =50% load, 100kPA = 100% load.

This method gives very good load resolution at low power if the engine has low overlap OEM type cams so there isn’t a lot of noise in the intake track but tends to have somewhat poor high power resolution.  Open5xxxECU allows you to set the crank position where the MAP sample is read which can very effectively eliminate the signal noise caused by big cams and a [multiMAP](http://code.google.com/p/open5xxxecu/downloads/detail?name=multi_map.zip&can=2&q=) can be used to help get a strong signal from an ITB (individual throttle bodies) setup.

**TPS** – This method provides the best high power resolution but worst low power resolution making it the traditional favorite with racers and last choice for street application.  TPS load sensing requires a pressure signal in addition to the TPS signal and if MAP is provided instead of barometric pressure the low power resolution issue can be overcome.

Another traditional issue with this method it that while 100% throttle is 100% load, that is the only point that matches because air flow (and the matching fuel flow) is not linear to throttle position like it is with MAP so the tuning is a bit harder.  To overcome this issue Open5xxxECU include a Throttle position v flow curve that is available when TPS load sensing is selected.  This curve allows you to calibrate you throttle so the ECU knows what load goes with that throttle position.

**MAF** – A mass air flow sensor tells the ecu exactly how much air is entering the engine so calculating the fuel requirement is the simplest and most accurate.  Because this method is so accurate and repeatable car to car it is used on virtually all current production cars.  The biggest down side it that if you have a MAF sensor that is small enough to do a good job at idle it causes a flow restriction at full power high RPM use.  A MAF sensor is also sensitive to noise in the intake system so it works best with low over lap OEM type cams and with the sensor wants to be mounted so distance from the intake manifold.