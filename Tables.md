# Introduction #
The ECU uses table driven math.  The tables are created in a way that allows the user to put in as much information as they have to get the base line tuning as close as possible before any actual tuning is done.

For example is you have an engine simulation program like DynoSim or Dynomation5 to predict the engine's output, you can enter the predicted torque curve in the "Engine Model" and the base fuel and timing will be adjusted accordingly.




# Details #

## The tables used in this project are a fixed axis increment, binary math type ##

### Does that mean we don't know how to do user defined axis or floating point math? ###
### No! ###

What it means is that we had to make a choice between giving you a few table with variable axises or a lot of tables with fixed axises and we chose the latter.

### But why?  Everybody knows you need variable axis to get a good tune right? ###
### No! ###

What you need to get a good tune is points that are close enough together to let the straight lines that connect the points give a good approximation on the torque curve.

One way to do this is to use variable point spacing to let the points you have do the best possible job.  This is method requires the fewest number of points but it consumes a lot of process time.  Jon did some testing, and "for me, tables that use variable axis spacing take
~6x longer (although still quite fast)".

We have LOT of memory available so using the fewest number of points in not of any real importance so we decided to just put lots of fixed points close enough together to give you  the same resolution you could get with variable points.  By doing this we can not only give you a fuel table, but in the same amount of processor run time we can also give you  trim tables for each and every injector.

Having trim tables (not just over-all trims) means that you can correct each cylinder for air flow differences, exhaust differences, injector performance differences, temperature differences, and just about anything else that comes along.  The best example might be that you have big injectors and big ports and big exhaust and big cams in you high output engine to make the hp and as a result have a hard time getting uniform cylinder mixtures at idle/low power.  You don't want to use an over-all trim to fix idle because that will mess up your high power mixture but a trim table allows you to make corrections exactly where you want the corrections and get your race engine behaving nicely in your stree car.


### But doesn't having all those points in the table make tuning harder? ###
### NO! ###

Yes there are more points but there is no reason in the world to try to tune ever one of them!  Tune the ones that are at a point you care about and use the interpolate feature to fill in the rest, interpolation between the point you care about is EXACTLY what happens in a user defined point table.  You simply lock the cell you've tuner then hit interpolate. Easy.

Your other option is to use the "User Defined" feature in the tuner.  This feature only displays the points you ask to see and automatically fills in the rest with the interpolate feature for you so you see EXACTLY what you you would see on a user define point system without paying the time penalty in the ECU itself.