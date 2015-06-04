All sensors require a calibration curve to be entered before they can be used.  Sensor calibration curves are found on the “Sensor Cals” tab

**TPS Position calibration** - This is used to set the voltage that the ecu should use for the throttle Open and Closed positions.  With the throttle close look at the displayed voltage and enter it next to the 0% cell.  Now open the throttle completely and enter this voltage next to the 100% cell.

**TPS Flow Calibration Enable** - this is only used if you are using TPS load sensing and is used to calibrate flow to throttle position so help the ECU understand what throttle position goes with what % load.  This is required because flow and MAP are not 0 when the throttle is closed, it’s about 15-30% and the air flow change is not linear with throttle change particularly near full throttle.

**Cranking Throttle Position Mixture Correction** - This allows the mixture to be altered using the throttle during cranking.  It’s used to enrich or cut the fuel to clear a flood and very helpful during initial engine set-up.  The recommended settings are:

| Throttle | Correction|
|:---------|:----------|
| 100      | -100      |
| 91       | -100      |
| 90       | 198       |
| 25       | 0         |
|0         | 0         |

This will set the correction to do nothing from 0-25% throttle, enrich up to 200% by 90% throttle and cut the cut completely above 90%.


**Inlet Air Temperature Sensor Calibration Curve** - Enter the voltage v temp information for your sensor

**Coolant Temperature Sensor Calibration Curve** - Enter the voltage v temp information for your sensor

**MAP 1-4 Sensor Calibration Curve** - Enter the voltage v MAP information for your sensor(s)

**MAF1-2 Sensor Calibration Curve** - Enter the voltage v MAF information for your sensor(s)

**AFR 1-2 Sensor Calibration Curve** - Enter the voltage v AFR information for your sensor(s)