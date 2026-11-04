**Scenario 3:** Molly, a friend of James who is in the trusted group of critical data accessed, has a problem with her credentials. She asked James to share his to avoid project delay. She accesses and downloads the data using James' credentials.

**System behavior:** Access logged, traffic captured, alert created as `james_analyst` with priority low because `james_analyst` is a trusted user.

In both scenarios, the system behaves the same way, linking access to James as a user, and cannot detect a potential breach. An extra layer of security might be required to address such challenges (use of **multi-factor authentication (MFA)**, access tokens, verification codes, and so on).

We can see a certain extent of overlapping between confidentiality and integrity because roles and groups define access restriction.

# Enforcing and maintaining availability

The system must be designed so that data is *accessible* when *needed* and within a reasonable *time frame*. It is a bit easier to detect an availability breach than confidentiality and integrity—a network shutdown, a slow system response, and a no-response do not require magic to identify them. However, it is a CIA component that can be affected by several factors, such as natural disasters. It means one more thing to worry about when designing a system for availability (earthquake, fire, rain, and storms, which can cause damages to assets and data). Availability is a function of three parameters, *accessibility*, *presence*, and *time*. Hence, as part of the security or CTI team, you should emphasize the following:

+   - **Retention period:** For how long should the data be kept? The system must be designed to look at the *short-*, *medium-*, and *long-term* availability requirements. The following two points strongly depend on the availability requirements. The long term could be 5 to 10 years, the medium term could be 1 to 5 years, and the short term could be up to 1 year. The retention component is not standard and depends on the business objectives.
+   - **Response time:** Data must be available at the appropriate time frame, depending on the availability requirements. For short-term access, you might need a **delay-sensitive system** (*shorter response time*) because the data or asset might be used for business decisions on the fly. However, a slight delay could be understandable for historical data, and for archived data, a more extensive delay could be tolerable. Delay tolerance is critical to data availability.