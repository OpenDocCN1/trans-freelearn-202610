```
```
20   windmill_height (m)                 27657 non-null   float64
21   windmill_generated_power(kW/h)       27993 non-null   float64
```
```

We can immediately see that the dataset has missing values, with some features having fewer than 28,200 values. We can get a better sense of the data distribution by calculating the statistical description:

```
```
train_df.describe().T.style.bar(subset=['mean'])
```
```

This prints out the following:

| Feature | count | mean | std | min | max |
|---|---|---|---|---|---|
| wind_speed(m/s) | 27927 | 69.04 | 76.28 | -496.21 | 601.46 |
| atmospheric_temperature(°C) | 24750 | 0.38 | 44.28 | -99.00 | 80.22 |
| shaft_temperature(°C) | 28198 | 40.09 | 27.20 | -99.00 | 169.82 |
| blades_angle(°) | 27984 | -9.65 | 47.92 | -146.26 | 165.93 |
| gearbox_temperature(°C) | 28199 | 41.03 | 43.66 | -244.97 | 999.00 |
| engine_temperature(°C) | 28188 | 42.61 | 6.12 | 3.17 | 50.00 |
| motor_torque(N-m) | 28176 | 1710 | 827 | 500 | 3000.00 |
| generator_temperature(°C) | 28188 | 65.03 | 19.82 | 33.89 | 100.00 |
| atmospheric_pressure(Pascal) | 25493 | 53185 | 187504 | -1188624 | 1272552 |
| area_temperature(°C) | 28200 | 32.74 | 7.70 | -30.00 | 55.00 |
| windmill_body_temperature(°C) | 25837 | 20.80 | 54.36 | -999.00 | 323.00 |
| wind_direction(°) | 23097 | 306.89 | 134.06 | 0.00 | 569.97 |
| resistance(ohm) | 28199 | 1575.6 | 483.33 | -1005.22 | 4693.48 |
| rotor_torque(N-m) | 27628 | 25.85 | 32.42 | -136.73 | 236.88 |
| blade_length(m) | 23107 | 2.25 | 11.28 | -99.00 | 18.21 |
| blade_breadth(m) | 28200 | 0.40 | 0.06 | 0.20 | 0.50 |
| windmill_height(m) | 27657 | 25.89 | 7.77 | -30.30 | 78.35 |
| windmill_generated_power (kW/h) | 27993 | 6.13 | 2.70 | 0.96 | 20.18 |

Table 6.1 – Statistical description of numerical features in the Wind Turbine dataset

Looking at the statistical description of the features in Table 6.1, we can see the following irregularities:

+   - **Many features have outliers**: Generally, a standard deviation larger than the mean may indicate outlying values. Examples include wind speed, atmospheric temperature, and atmospheric pressure. Similarly, a minimum or maximum far away from the mean may indicate outliers in the data. We can further verify this by visualizing the data distribution using a histogram.