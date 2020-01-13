# NJChl
A brief investigation into the relationship of chlorophyll, temperature, and precipitation in the Delaware Bay. File 2.1 NJDEP and 2.1 Aqua show how the chlorophyll data was imported and compressed. File 2.1 Weather shows the import and analysis of the temperature and precipitation data. File 2.2 shows the analysis of the chlorophyll data and the development of the model.

Issues:

Fourier transforms would not work on weather data. There are nan values in the data, but when removed, the code fails to run. 

The compressed chlorophyll data is not continuous. There are gaps of several days between succesive readings. This causes errors in the autocorrelation. Also, a standard fast fourier transform cannot be run on non-contiuous data. A python package was found for a non-uniform fourier tranform, but using this method on the data doesn't appear to provide any meaningful data.

Findings:

There is no apparent trend in the low pass signal for precipitation or the high pass signals for precipitation, chlorophyll, or temperature. However, there is an apparent trend in the low pass signal of chlorophyll (yearly sinusoidal) and an unmistakable signal of temperature (yearly sinusoidal). The fourier transforms either didn't work on the data sets or provided unhelpful data.

The linear, polynomial, and gaussian regressions didn't provide any insights into the trend of the chlorophyll data. However, several sine curves were manually fit and appear to mimic the upper and lower bounds of the data.

A basic model of the chlorophyll data was created using temperature and precipitation as input parameters as inputsas shown below:

modelChl = modelChl + 1/5 * weather.TMAX[modelTime + lag] + 1/5 * weather.PRCP[modelTime + lag]

The chlorophyll levels in mg/m³ roughly correspond to one fifth of the temperature in °F. The precipitation datain inches was also included at one fifth of its value, but provides almost nothing to the curve. These coefficients were manually tuned. The model does not capture the complexity of the raw data, but when the rolling average of the model and chl signal are compared, the model and the chl data have the same sinusoidal pattern. However, the model is offset (out of phase) with the chl signal, so a lag was introduced. The chlorophyll signal appears to reach the yearly minimum 80 days before the temperature hits the yearly minimum. Thus, the tuned model shows that temperature and chlorophyll can be roughly correlated, but there are too many variables to suggest that this is a causal relationship. 
