# Typical industrial network architecture designs

Industrial network architectures as we see them implemented today didn't just fall out of the sky. They evolved over time from standalone devices or small collections of connected proprietary devices, using proprietary communication media and proprietary protocols to communicate. Before we look how to design for security, let's first take a quick scroll through the history of automation and controls and see how things came about.

# Evolution from standalone islands of automation

When, back in the late 1960s, **Programmable Logic Controllers or PLCs** first started replacing the racks of relays, timers, and actuators that used to run the controls and automation process, they weren't much more than that: a replacement for those racks of relays, timers, and actuators (controls). They were a very compact and sophisticated replacement for those controls, though, allowing the owner to make changes without having to rewire half the plant. The first PLCs also only took up a fraction of the control-room space that was before dedicated to control devices and wiring.

The first PLC implementations were not much more than standalone islands of automation and groups of related automation equipment, dedicated to a single system or part of a system, not wired to anything but the essentials to do the automation task. A setup such as that also needed specialized and often proprietary *programming terminals* to be able to connect to the PLC and make changes or troubleshoot the process.

Have a look at the following screenshot:

![](bbox=[0.287, 0.571, 0.746, 0.837])

Figure 4.1 – Modicon 184: image courtesy of RepairZone.com

The preceding screenshot shows what is considered to be the first PLC, the Modicon 184, released in 1973.