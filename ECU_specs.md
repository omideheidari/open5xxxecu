# Introduction #

MPC5634: 1.5M Flash 80Mhz Power PC.  The development system will use a TRK5634  development board for the processor board, a custom version will follow after initial testing.

Here's what currently planned for the Gen 1 ECU to be able to do and status.

In Process Draft 2013/Jun/07

| Feature | Planned | Firmware status | Hardware Status |Comments |
|:--------|:--------|:----------------|:----------------|:--------|
|outputs  |         |                 |                 |         |
| Fuel    | 8 Channels, 8@ 2A | complete        | Prototyping     | Unused Fuel channels may be used as optional outputs |
| Spark   | 4 channels, logic level | complete        | Prototyping     | Unusued spark channles may be used as optional outputs signals |
| Optional  Outputs | 4 channels @ 1A | Prototyping     | Prototyping     | PWM capable |
| Stepper Drive | 1 (4 outputs) @ 1A | not supported   | Prototyping     | May be used for GM type IAC |
| Fuel pump control | 1 @ 2A  | complete        | not supported   | must use fuel, spark or optional driver |
| O2 sensor heater | 2 @ 2A  | not supported   | not supported   |must use optional driver |
| Malfunction light | 1 @ 2A  | not supported   | not supported   |must use optional driver |
| Tach signal | 1 @ 2A  | complete        | not supported   |must use optional driver |

|Inputs   |         |                 |                 |         |
| Crank signal | 1 hall/VR | complete        | Prototyping     | must be 6-255 teeth with 1-3 missing teeth |
| Cam signal | 1 hall/VR | complete        | Prototyping     | signal tooth type (window provided to ignore additional teeth) |
| TPS sensor | 1       | complete        | Prototyping     |         |
| MAP Sensor | 2       | complete        | Prototyping     | MAP2 is Baro |
| MAF sensor | 1       |complete         | Prototyping     |         |
| Coolant Temp | 1       | complete        | Prototyping     |         |
| Air Temp | 1       | complete        | Prototyping     |         |
| Wheel speed | 4       | not supported   | not supported   | will be external box |
| O2 sensor | 1 wide or narrow | complete        | Prototyping     | wide requires external controller|
| Knock sensor | 1       | not support     | not supported   |         |
| Analog inputs | 6       | complete        | Prototyping     |         |
|         |         |                 |                 |         |
| RS232   | 1       | complete        | Prototyping     | uses DMA|