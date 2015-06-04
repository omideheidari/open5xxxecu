# Global Injection time #
All fuel flow calculations are done in units of injection time in  usec.  Doing this avoids having to do unit conversions in real time  which can consume quite a bit of processor time.

A global injection time is calculated as follows
## Base\_Injection\_Time ##
The starting point is Base\_Injection\_Time
> (currently Max\_Inj\_Time and is a user input, no calculation)

The assumptions are :
  * Lambda = 1.0
  * VE = 100%
  * Air is at STP (0C and 100kKPa)giving a density of 1.293 kg/m^3

Then,
Air mass flow = Displacement x Air Density

Fuel mass flow required is then
Fuel\_mass\_flow= Air\_mass\_flow  / 14.7

Base\_Injection\_Time = Fuel\_mass\_flow / injector flow rate  / Number\_Cylinders

Injector flow rate is calculated using:
Rated\_injector\_flow\_rate
Rating\_Fuel_Pressure
Actual\_Fuel\_Pressure_

Injector\_Flow\_Rate =  (Actual\_Fuel\_Pressure /  Rating\_Fuel_Pressure) ^ .5   x Rated\_injector\_flow\_rate_

Base\_Injection\_Time would ideally be calculated in the tuner and passed to the FW but TunerStudio is not setup to do pre-processing so this is calculated on ECU started up

Once Base\_Injection\_Time is calculated a series of corrections are applied to it.

## Load correction ##
The load correction reads the % load and directly scales the injection time
```
 Pulse_Width = (Pulse_Width * Load) >> 14;
```

For information on % load calculation see “Load Calculation”

## Fuel Table Correction ##
The main fuel table has axes of % load and  rpm with the lookup term being a %correction that is then applied directly
```
Corr = table_lookup_jz(RPM, Load, Inj_Time_Corr_Table);
        Pulse_Width = (Pulse_Width * Corr) >> 14;
```

## Air Temperature Correction ##
The air temperature correction assumes air follows the ideal gas law and the density is therefore inversely proportional to absolute temperature.

To help simplify this calculation the units of temperature used are “% Standard  Absolute Temperature” where Standard Temperature is:
> 0C and standard absolute temperature is then  273.15K.

And:
IAT = % Standard Temperature  =   IAT\_sensor\_reading  /  273.15

The correction is then applied directly as:
```
Pulse_Width = (Pulse_Width << 14) / IAT;
```
Note that the air density correction is being applied directly to the fuel not an intermediate calculation of air density.


## Engine Temperature Corrections ##
Two different corrections are done based on engine coolant temperature (CLT), which for consistency is again a % standard absolute temperature, 273.15K.

The first correction is a steady state operation term.  This  is a % correction vs temperature lookup that is used to take heat gain and fuel evaporation changes  that can occur in the normal operating temperature range of the engine.  Most users will probably not need this but it can be helpful to deal with effects of a malfunction cooling system.
```
      Corr = table_lookup_jz(CLT, 0, Fuel_Temp_Corr_Table);
        Pulse_Width = (Pulse_Width * Corr) >> 13;
```

The second correction is the transient warmup correction ans is explaind in detail here :

[Prime/Warm-up](https://code.google.com/p/open5xxxecu/wiki/Fuel_Correction_Prime)

Once calculated the correction is applied
```
Pulse_Width = (Pulse_Width + Prime_Corr);
```

Note this is an addition of fuel instead of a multiplication.  This is true because this fuel is being stored in the intake and is not entering the cylinder.

## Acceleration/Deceleration Correction ##
The Acceleration/deceleration correction and is explained in detail here :
> [Accel/Decel](https://code.google.com/p/open5xxxecu/wiki/Fuel_Corection_Accel_Decel)

Once calculated the correction is applied
```
           if (TPS_Dot_Sign > -1) {
                Pulse_Width = (Pulse_Width + ((Pulse_Width * TPS_Dot_Corr) >> 14));
            } else {
                Pulse_Width = (Pulse_Width - ((Pulse_Width * TPS_Dot_Corr) >> 14));
                if (Pulse_Width < 0)
                    Pulse_Width = 0;
```

Note this is an addition/subtraction of fuel instead of a multiplication.  This is true because this fuel is being stored in the intake and is not entering the cylinder.