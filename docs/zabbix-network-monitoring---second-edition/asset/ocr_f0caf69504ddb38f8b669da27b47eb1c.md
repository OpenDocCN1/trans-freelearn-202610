# Maintenance period options

So far, the only type of maintenance subperiods we've used is one-time maintenance. We decided to call those periods that were configured in a separate tab "subperiods" to distinguish then from the main period, configured in the first tab, Maintenance. We also discovered that maintenance would be active only during the time for which the main period overlaps with the subperiods. But what's the point of defining the same thing twice; couldn't the one-time period be the only thing to specify? The benefit of the main period becomes more apparent when configuring recurring maintenance, so let's explore the options available for subperiods. You may navigate to Configuration | Maintenance, start creating a new maintenance, and play with the available subperiods as we explore them.

# One-time only maintenance

This is the maintenance subperiod type we've already used. It starts at the specified date and time, proceeds for the amount of time specified in minutes, hours, and days, and that's it. This type of subperiod must still overlap with the main period.

# Daily maintenance

For daily maintenance, we have to specify the starting time and the length of the maintenance period:

![](bbox=[0.068, 0.564, 0.922, 0.688])

During the main period, maintenance will start every day at the specified time. It will start every day with the Every day(s) option set to the default, 1. We can change this and make the maintenance only happen every second day, third day, and so on.