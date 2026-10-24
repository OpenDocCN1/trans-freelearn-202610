# Note

Image source: https://commons.wikimedia.org/wiki/
File:Wind_turbines_in_southern_California_2016.jpg

Wind turbines are relatively simple to construct, but wind farms can contain many
turbines. This makes it an ideal candidate for our first Digital Twin prototype.

The most common type of wind turbine is configured with a horizontal axis, but turbines
can also have a vertical access configuration. Wind turbines are further classified based
on their installation. Land-based turbines are classified as onshore, while offshore wind
turbines can be deployed miles away in the ocean. Offshore wind turbines have the
added complexity of remote access both from a connectivity and a physical access of
the field service crew perspective. The offshore wind turbines have to additionally deal
with corrosion due to seawater. (See https://www.materialsperformance.
com/articles/material-selection-design/2016/03/corrosion-
risks-and-mitigation-strategies-for-offshore-wind-turbine-
foundations.)

We will use an onshore, horizontal-axis wind turbine with a typical three-blade
configuration to simplify our first Digital Twin. Figure 6.3 shows the high-level
components of a horizontal-axis wind turbine. We will primarily focus on the overall wind
turbine performance, as well as the motor, gearbox, and generator units of the asset. To
detect or prevent equipment failure, our maintenance Digital Twin will monitor real-time
data from these components. You can extend the Digital Twin with additional models at a
later stage to include other components, such as the pitch system, controller, and nacelle.

![](bbox=[0.2305, 0.5922, 0.7422, 0.8656])

Figure 6.3 – Inside a typical wind turbine