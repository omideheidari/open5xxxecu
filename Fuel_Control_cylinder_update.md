# Cylinder fuel update function #

The purpose of this function is to update the eTPU with the correct injection pulse values for each cylinder.

A test is performed to determine in the cylinder should be off and if so it is shut off.

Else the cylinder should be on and updates are required.

an additional test is performed to to determine if cylinder trims are enabled and apply the cylinder specific correction if require.


```
           Cyl_Pulse_Width=  Pulse_Width
           for (i = 0; i < N_Cyl; ++i) {
               if Cylinder_Fuel _satus[i] ==0 {
          	fs_etpu_fuel_switch_off(Fuel_Channels[i]);
	 Injection_Time = 0;
              }else{
                          //need to make table use the "i" value before it will work
	if (Fuel_Trim_Enable ==1){
            	    Corr = table_lookup_jz(RPM, Load, Cyl_Trim_Table[i]);
            	    Cyl_Pulse_Width=  (Pulse_Width * Corr) >> 14;
	}
               	error_code = fs_etpu_fuel_set_injection_time(Fuel_Channels[i], Cyl_Pulse_Width); 
	 fs_etpu_fuel_switch_on(Fuel_Channels[i]);
                }//if    

     
           } // for
```