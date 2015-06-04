# Accel/Decel Function #


The accel/decel function works on the principle that not all the fuel that the injectors delivers makes it into the cylinder right away and the fuel that doesn’t make it to the cylinder immediately is stored in the intake manifold and port.   This means under some conditions  extra fuel needs to be delivered to ensure enough does get to into the cylinder and under other conditions the fuel delivered needs to be reduced to ensure too much fuel is not going to the cylinder  as stored or puddle fuel is pulled into the flow stream.

The primary indicator of when conditions are changing fast enough to require a correction is throttle position rate of change or TPS\_Dot.  When the throttle is opening the pressure in the manifold is increasing which cosese fuel to condense in the intake on on the runners so that fuel does not reach the cylinder and extra fuel is required to maintain the correct mixture.  The opposite is true for a  closing throttle, the pressure in the intake drops causing fuel to be evaporated and the injection pulse must be decreased to prevent too much fuel from entering the cylinder.


There are 6 lookups used for this correction, 3 for acceleration, 3 for deceleration.  This functional description will cover only acceleration but deceleration works the same way other than the correction is subtracted instead of added.

The tables are:
# Acceleration limit  – max %correction allowed vs rpm
# Acceleration Throttle sensitivity  - %correction vs TPS\_Dot
# Acceleration Decay rate curve - % to decay the correction/per vs rpm

First a TPS\_Dot is calculated
```
            //get a TPS change         
            TPS_Dot_Temp = (TPS_Last - TPS);
            TPS_Last = (3 * TPS_Last + TPS) >> 2;
            //get           
            TPS_Dot = TPS_Dot_Temp << 3;
            TPS_Dot_Degree = (Degree_Clock - Degree_Clock_Last);
```

A check is done to see if a new acceleration correction should be calculated.   The test is to see is the TSP\_Dot value is larger than a Dead band value (used to fitler) and also bigger than the previous TPS\_Dot (the user is opening the throttle faster than the last time the test was done so the correction is based on the largest TPS\_Dot see)

….perhaps this should be looking at this should be looking for a max correction not the max TPS\_Dot.

…also this works on the idea that once a user stomps the throttle open the only way to go is closed which will generate a negative TPS\_Dot and resets everything for acceleration.  If however you stopm the pedal fast to partial throttle, wait, then stomp it the rest of the way slower an accel correction will not be applied…..this probably needs to be fixed.

```
            // check if acceleration enrich required
            if (TPS_Dot >= TPS_Dot_Dead && TPS_Dot > TPS_Dot_Last) {
```

If true then all the table lookups are done

```

                TPS_Dot_Limit = table_lookup_jz(RPM, 0, Accel_Limit_Table);
                TPS_Dot_Corr = table_lookup_jz(RPM, 0, Accel_Sensativity_Table);
                TPS_Dot_Decay_Rate = table_lookup_jz(RPM, 0, Accel_Decay_Table);
```

a correction factor is calculated by multiplying the sensitivity value by TPS\_Dot

```
                TPS_Dot_Corr = (TPS_Dot_Corr * (TPS_Dot - TPS_Dot_Dead)) >> 14;
```

And the various flags and clocks reset that are related to the max TPS\_Dot
```
                // update the last clock
                Degree_Clock_Last = Degree_Clock;
                TPS_Dot_Degree = 0;
                TPS_Dot_Decay_Last = 1 << 14;
                TPS_Dot_Sign = 1;
            }
```

The current TPS\_Dot is saved
```
            TPS_Dot_Last = TPS_Dot;
```

A test is done to see if a decay is required.  A decay is applied every engine cycle after a max TPS\_Dot is found.  If a decay is required the TPS\_Dot correction is multiplied by the decay rate from the earlier table lookup.

Note that the used enters a decay rate of say -10%, meaning decay 10% er engine cycle, but the value pasted to the ecu is 0.9 so the TPS\_Dot correction can be directly multiplied by  the decay rate.
```
            // calculate the required decay
            if (TPS_Dot_Degree >= 720) {
                Degree_Clock_Last = Degree_Clock_Last + 720;
                TPS_Dot_Decay = (TPS_Dot_Decay_Last * TPS_Dot_Decay_Rate) >> 14;
                TPS_Dot_Decay_Last = TPS_Dot_Decay;
                TPS_Dot_Corr = (TPS_Dot_Corr * TPS_Dot_Decay) >> 14;
            }
```
A Test is done to ensure the correction calculated done not exceed the correction limit in the limit table
```
            if (TPS_Dot_Corr > TPS_Dot_Limit)
                TPS_Dot_Corr = TPS_Dot_Limit;
```

A test is done to determine if Accel or decal is being applied

```
            if (TPS_Dot_Sign > -1) {
                Pulse_Width = (Pulse_Width + ((Pulse_Width * TPS_Dot_Corr) >> 14));
            } else {
                Pulse_Width = (Pulse_Width - ((Pulse_Width * TPS_Dot_Corr) >> 14));
                if (Pulse_Width < 0)
                    Pulse_Width = 0;
            }

        }
```
Note this is an addition/subtraction of fuel instead of a multiplication.  This is true because this fuel is being stored in the intake and is not entering the cylinder.