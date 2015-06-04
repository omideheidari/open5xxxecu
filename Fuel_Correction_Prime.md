# Prime/warm-up function #

The prime/warup function works on the principle that not all the fuel that the injectors delivers makes it into the cylinder right away so extra fuel needs to be delivered to ensure enough does get to into the cylinder.  The Prime/warm-up function supplies the extra fuel until the engine is at operating temperature.

There are 2 lookups used for this correction
The first is %extra fuel vs temperature.  This values is used to scale  current injection pulse width create a base correction value
```
Prime_Corr = table_lookup_jz(CLT, 0, Dummy_Corr_Table);
                // scale the correction to the pusle width
                Prime_Corr = (((Pulse_Width * Prime_Corr) >> 13) - Pulse_Width);
```

The second step is to set a decay rate.  The theory here is that at any temperature the engine will reach steady state where :

fuel stored = the fuel released

so less extra fuel is required each cycle because more is being released each cycle.

The decay term is %correction decreaseper cycle  v rpm and is applied :
```
                    Prime_Post_Start_Last = Post_Start_Cycles;
                    // Get the decay rate for current conditions
                    Prime_Decay = table_lookup_jz(RPM, 0, Prime_Decay_Table);
                    // decrease decay by the new value
                    Prime_Decay = (Prime_Decay_Last * Prime_Decay) >> 14;
                    // reset last
                    Prime_Decay_Last = Prime_Decay;
```

Then the correction is applied to the pulse width

```
Pulse_Width = (Pulse_Width + Prime_Corr);
```


Note - this is an addition/subtraction of fuel instead of a multiplication.  This is true because this fuel is being stored in the intake and is not entering the cylinder.