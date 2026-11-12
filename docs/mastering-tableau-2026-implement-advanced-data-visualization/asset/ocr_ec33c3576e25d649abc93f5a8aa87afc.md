# # Tableau Pulse

With the foundation laid, we can now turn on the engine of insight delivery: Tableau Pulse. Tableau Pulse provides high-level monitoring rather than deep-dive exploration. It's a tool for staying informed.

Pulse monitors metrics you've defined or chosen to follow from various dashboards. It continuously analyzes them for statistically significant changes, trends, outliers, and key drivers. When something noteworthy occurs, it generates an insight. Here is an example from the Tableau Pulse Demo (https://www.youtube.com/watch?v=Cnbdkye0X_8_)

**Total Sales** this month reached **$494.2k** on January 30, 2024. The unexpected small increase moved Total Sales **below the expected range** of **$499.4k to $549.3k**. The trend for **Total Sales** changed 8 days ago and is now **increasing at a slower rate.**

![](bbox=[0.35, 0.42, 0.72, 0.68])

Figure 15.1: Tableau Pulse

When creating a metric in Tableau Pulse, the important step is defining relevant dimensions, such as Region or Category. They provide the context that Pulse AI uses to automatically evaluate performance drivers and explain the *why* behind a change.

Once your metrics are created, Pulse delivers insights through several channels:

+   * **The For You Page**: Your personalized homepage in Tableau Cloud, aggregating insights from all the metrics you follow