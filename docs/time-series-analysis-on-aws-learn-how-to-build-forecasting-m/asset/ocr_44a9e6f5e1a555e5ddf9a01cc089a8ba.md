2. The MA step gives the same importance (weight) to each of the previous timesteps that fall in the rolling window width. Adding a smoothing constant will give more weight to recent values while reducing the impact of the window width choice. Let’s call our smoothing constant α: this value, set between 0 and 1, will be a parameter telling our model how fast we want to forget the previous values of the time series. This can also be called a baseline factor as it’s used to model the **base behavior** our forecast will have. In the following formula, you will notice that the previous timesteps are now weighted with a weight that is exponentially decreasing. Our forecast baseline at time t+1 can be derived from the series level at the previous timestep, Lₜ:

**Fₜ₊₁ = Lₜ**
**Lₜ = α yₜ + (1 − α) Lₜ₋₁**
**Fₜ₊₁ = α yₜ + α(1 − α) yₜ₋₁ + α(1 − α)² yₜ₋₂ + …**

3. When a **trend component** is present in our time series, the previous approach does not do well: to counter this, we add a trend smoothing parameter, generally called β, that we apply in a similar fashion on the difference between successive timesteps on our series. β is also set between 0 and 1. This yields to the double exponential smoothing, and the point forecast k periods ahead can be derived from both the series level and the series slope at the previous timestep Lₜ and Bₜ, as illustrated in the following formula:

**Fₜ₊ₖ = Lₜ + k · Bₜ**
**Lₜ = α yₜ + (1 − α) · (Lₜ₋₁ + Bₜ₋₁)**
**Bₜ = β · (Lₜ − Lₜ₋₁) + (1 − β) · Bₜ₋₁**

4. If our signal also contains additional **high-frequency (HF)** signals such as a **seasonality component**, we need to add a third exponential smoothing parameter, usually called γ, and call the seasonal smoothing parameter (also set between 0 and 1 as α and β). If m denotes the number of seasons to capture our new point forecast k periods ahead, this can now be derived with the triple exponential smoothing, using the series level Lₜ, the series slope Bₜ, and the seasonal component Sₜ at the previous timestep, as follows:

**Fₜ₊ₖ = Lₜ + k · Bₜ + Sₜ₊ₖ₋ₘ**
**Lₜ = α (yₜ − Sₜ₋ₘ) + (1 − α) · (Lₜ₋₁ + Bₜ₋₁)**
**Bₜ = β · (Lₜ − Lₜ₋₁) + (1 − β) · Bₜ₋₁**
**Sₜ = γ · (yₜ − Lₜ₋₁ + Bₜ₋₁) + (1 − γ) · Sₜ₋ₘ**