# Switch Function #
To prevent the possibility of incomplete fuel pulses being deliver when cylinders are started or shut down for any reason there is a switch fuels on/off command.

int32\_t fs\_etpu\_fuel\_switch\_off(uint8\_t channel)
int32\_t fs\_etpu\_fuel\_switch\_on(uint8\_t channel)

After initializing the fuel channels, all channels should be switched off until the CPU calculates and load the correct injection information
-	This is not currently done!