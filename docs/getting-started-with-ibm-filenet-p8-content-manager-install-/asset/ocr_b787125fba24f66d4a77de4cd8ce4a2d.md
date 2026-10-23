With a simple clustering solution, failure of a single node in the cluster means that other nodes will take over a proportionate share of the workload, though sometimes with reduced performance.

# High Availability

**High Availability (HA)** takes clustering a step further and usually involves ensuring there is no single point of failure in the entire environment. Along the same lines, equipment is provisioned so that one or more redundant nodes can fail and the other nodes will be able to handle the workload without a degradation in performance. To a first approximation, HA is the over-provisioning of your resources and having a strategy for nodes to reliably cover for each other.

HA planning is generally a complex orchestration of budget availability versus risk tolerance.

# Disaster Recovery

**Disaster Recovery (DR)** is the ability to promptly resume operations after a catastrophic failure of a primary facility. To the uninitiated, that sounds a lot like HA, but it's qualitatively different. To be effective, a DR site must be located away from the primary site.

The definition of "away from" depends on what type of disaster you are planning for. Is it a flash flood in a nearby river? In that case, a DR site a few miles away might be fine. Is it an earthquake, volcanic eruption, terrorist attack, or asteroid strike? In that case, you probably want your DR site to be quite a bit farther away. Is it the sun going supernova and destroying all of the planets (including Pluto, even if it's not a planet)? In that case, hmm, we'll leave it to you to figure that one out.

DR planning should involve these factors:

+   - A DR site with a location chosen according to the disasters for which you are planning. The site will have sufficient equipment, networking capacity, and other resources to carry whatever percentage of the operational load you want.
+   - Some means of replicating the data from your primary site to your DR site. This could be as simple as periodic backups physically carried to the DR site, or it could be as complicated as near real time network copying over fiber links.