# Adding alerting features

In the preceding section, we carefully planned and executed our approach regarding pipeline failures and restart mechanisms. To add greater durability to the pipelines, we should configure some alerting mechanisms. Without proper alerting mechanisms, administrative users might not be able to react and recover from failures in time. This can be catastrophic because delays in recovering from pipeline failures have been known to cause huge customer dissatisfaction, delays in real-time decision making, or, even worse, data corruption:

![](bbox=[0.12, 0.29, 0.88, 0.62])

Figure 9.19 – Adding alerts to a pipeline

Here are the steps for creating real-time alerts:

+   1. Consult the customer regarding the user(s) who should receive the alerts. Emails and SMS are two commonly chosen mechanisms. Instead of sending alerts to individual emails addresses, it is highly advisable that you send alerts to email groups such as administrators@site.com.
+   2. In the Azure data factory, click on Monitor. Now click on Alerts and metrics. Then, click on New alert rule:

+   - Alert rule name: Error Alerts
+   - Description: Alert for pipeline failures
+   - Severity: Sev1