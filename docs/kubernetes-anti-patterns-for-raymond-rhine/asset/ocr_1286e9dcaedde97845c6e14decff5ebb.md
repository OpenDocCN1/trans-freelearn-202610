# Altered deployment and operational metrics

Common pitfalls in Kubernetes can subtly and significantly impact the metrics that are used throughout the Kubernetes ecosystem to evaluate deployment and operational efficiency. These often subtle shifts can alter the conventional perception of the ecosystem’s performance.

Deployment frequency, a metric often equated with agility, can be misleading when anti-patterns are at play. An increase in deployment frequency might appear as a positive indicator, but it could also signify rushed releases, inadequate testing, or a lack of readiness for production, leading to instability and potential downtime in the ecosystem.

Change failure rate experiences a similar shift. While Kubernetes can mask immediate deployment failures, resulting in a seemingly improved change failure rate, this can hide deeper issues. Problems such as configuration drift or resource contention, which are not immediately evident, can gradually erode the system’s stability and resilience, impacting the long-term health of the Kubernetes ecosystem.

The **mean time to recovery (MTTR)** is another metric that’s affected by Kubernetes anti-patterns. The platform’s ability to quickly roll back changes and restore previous states might create an impression of a resilient system. However, this can also prevent teams from addressing underlying causes of