The following screenshot shows how this should be done:

![](bbox=[0.115, 0.132, 0.85, 0.505])

Figure 9.50 – Exercise 6: Zeek notices – creating a visualization

By default, Zeek flags self-signed certificates as invalid and creates a notice for them. Although this is a significant finding for IT environments, on the OT side we are less concerned with this discovery. The shielded architecture of a typical industrial environment makes the management of a public-key infrastructure (PKI) difficult to maintain, and hence certificate errors are not uncommon. Ignoring these certificate errors in our Breach Detection dashboard view is OK. We will not eliminate them from the database, just hide them from our view.