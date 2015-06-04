# Limits function #

## Under construction ##

The purpose of this function is to decide if, when, and which cylinders should be shut down.  To do this fuel, spark, or both can be cut and this is configured in the limiter setup.

These settings are global and apply to any function that needs to cut cylinders.  All cylinder cut requests are read and the highest number requested is acted on.

Fuel is 1 or 2 (for staged injection) eTPU channels per cylinder, spark is 1 eTPU channel per 1 or 2 cylinders.  Because of this when fuel and spark cut is selected the cut pattern must follow the number of coils on the number of cylinders.

The cylinder cut starts at 1 and selects the required number to cut in order.  The start point increments by 1 each cycle and the previous start point is retained until the ECU is powered off.

The firing order is also considered in the cut pattern generation such that the second cylinder cut is the one closest to 360 degrees away for the first cut.

For example on a 4 cylinder engine with a firing order of 1, 3, 4, 2 the cut pattern would be
Cycle 1 = 1, 4, 2, 3
Cycle 2 = 2, 3, 4, 1
Cycle 3 = 3, 2, 4, 1
Cycle 4 = 4, 1, 2, 3
Cycle 5 = 1, 4, 2, 3
And so on

Also available is an ignition retard feature which works in conjunction with the cylinder cuts.  When enabled the limit routine will apply ignition retard first to attempt to limit engine output.



## Rev Limiter ##
The purpose of this function is to prevent engine damage from over rev.  The limiter is a “soft” limit type and works by cutting a number of cylinders starting at the soft limit set-point and cutting proportionally more cylinders as the engine approaches the hard limit set-point where all cylinders are cut.

## Traction Control ##
The purpose of this function is to control power on wheel slip.  This function will attempt to limit wheel slip by retarding ignition timing first and if that is not adequate will cut cylinders.  The correction begins when wheel slip exceeds  the slip threshold and the amount of correction is proportional between the slip threshold and max slip settings.