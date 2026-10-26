# # A/B testing deployment strategy

A/B testing is similar to the canary deployment strategy, which means that different versions of the same service run simultaneously. However, the new features are enabled or disabled using feature flags. Feature flags can be implemented in your service code to enable a block of code. If a particular feature is enabled, you can control this from the admin interface. If a feature flag is enabled at a certain period, users can access the new features, and then the teams can monitor how their new version is behaving. The following diagram depicts A/B testing:

![](bbox=[0.278, 0.277, 0.723, 0.595])

*Figure 8.2 – A/B testing deployment strategy*

Many tools support implementing A/B testing using the feature flags approach. LaunchDarkly is one of the most popular tools that allows you to implement feature flags easily and smoothly.

> **Note**
> 
> To learn more about LaunchDarkly, refer to the following link:
> 
> `https://launchdarkly.com/`