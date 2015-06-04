# Fuel control #
The  flow is controlled by controlling the time the fuel injectors are open.  This task is handled in the eTPU using code available from FreeScale and describe in application note AN3770




# Fuel calculation #

The FreeScale eTPU fuel function requires several inputs from the CPU.


  1. [Switch fuel on/off](https://code.google.com/p/open5xxxecu/wiki/Fuel_Control_Switch)
  1. [Compensation or dead time](https://code.google.com/p/open5xxxecu/wiki/Fuel_Control_Compensation_Time)
  1. [Global Injection Time](https://code.google.com/p/open5xxxecu/wiki/Fuel_Control_Global_Injection_Time)
  1. [Cylinder Injection Time Trim](https://code.google.com/p/open5xxxecu/wiki/Fuel_Control_cylinder_update)