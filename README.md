This directory contains a small project to make updates to the NTC lookup table a lot easier and robust.
It makes use of the 'NTC thermistor library' http://thermistor.sourceforge.net/

The make file is targeted for GCC. Just run make, and if all is well, an excutable ('lut') will be created.

Run lut with a single argument that is a file containing resistance-temperature values, separated by a single tab.
For example './lut ntc-10k-3435.txt'

This will make use of the NTC thermistor library to calculate Steinhart-Hart coefficients based off the supplied data,
and then use that to calculate the expected temperature at the A/D lookup points.
It will output a lot of stuff, but the last few lines will be the actual code lines to go into STC-1000+ source (in page0.c).

The current model used for STC-1000+ is the data in ntc-10k-3435.txt.
As far as I know, the sensor shipped with the STC-1000 is a 10k NTC thermistor with a &beta;<sub>25&deg;C/85&deg;C</sub> value of 3435, presumably 1%.

The thermistor is connected to the A/D via a resistor network like this:
![resistor network](img/vdiv.png)

The program can optionally accept parameters to change the default *R0*, as well as *AD_MAX* and *AD_STEP*.
*AD_MAX*, is the number of values the A/D can produce. The A/D in the PIC16F1828 is 10-bit, and thus this value is 2^10 = 1024.
*AD_STEP* is the number of A/D points for each look-up point. For STC-1000+, 32 steps are used. That gives a total of 1024/32 = 32 look up points.
