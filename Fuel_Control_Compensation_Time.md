# Compensation or dead time #


Compensation time or dead time is the time it takes the injector and start flowing fuel.

int32\_t fs\_etpu\_fuel\_set\_compensation\_time(uint8\_t channel,
uint24\_t compensation\_time\_us)

Compensation time is varies with system voltage so a lookup table of time v voltage is used.

This is a global term so a call to any fuel channel sets the value for all fuel channels which causes a problem because rarely do any 2 injectors in a set actually have the same dead time much less injectors from different sets like are often used on staged injection setups.

The only way to deal with this issue is to alter the injection pulse time which is injector specific

Currently the FW does this:
```
Dead_Time_Set –  the base dead time
Inj_Dead_Time_Table - %change vs. voltage table

Dead_Time = (Dead_Time_Set * table_lookup_jz(V_Batt, 0, Inj_Dead_Time_Table)) >> 13;
```

Then Dead\_Time is sent to the eTPU using the update compensation time function

-----this is what needs to happen, not what is currently happening!!!! -----

What should be happening is:j = Injector_Set_[i]
Dead_Time_[i] = ((Dead_Time_Set + Inj_Dead_[i]) * table_lookup_jz(V_Batt, 0, Inj_Set_[j]_Dead_Time_Table)) >> 13;}}}```