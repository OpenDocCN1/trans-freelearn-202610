
## Practice Exam Questions: Virtualization

**6. A manufacturer of pressure-relieving mattresses has a legacy application that tests cell movement. It needs to run on a `Windows 98` operating system, which is a legacy system that is no longer supported. The company is updating all of the servers to `Server 2022`, with `Windows 11` desktops, and plans to outsource the rewriting of the legacy application so that it is `Windows 11` compliant. What should the manufacturer do in the meantime? (Choose two)**

1.  Build a `Linux` workstation and use it to run the legacy application
2.  Create a `sandbox` for the legacy application
3.  Build a `virtual machine` with the `Windows 98` operating system
4.  Install `Hyper-V` on one of the `Windows 11` desktops in the manufacturing department
5.  Install the application on a `SQL` server

**7. What is the purpose of using `Second Level Address Translation (SLAT)`?**

1.  It improves the performance of `virtual memory` when multiple guest machines are installed
2.  It improves the performance of `virtual networking` when multiple guest machines are installed
3.  It prevents the use of `virtual memory` when multiple guest machines are installed
4.  It throttles the processor on multiple guest machines

**8. Which of the following describes `cross-platform virtualization`?**

1.  Installing virtual machines on `Hyper-V`, `ESX`, and `Xen`
2.  Installing and networking multiple `sandboxes`
3.  Testing multiple software applications, one after another, in a single `sandbox`
4.  Testing software applications under multiple operating systems and workloads

**9. A company has a virtual network running 10 host machines, each with 50 guest machines. Yesterday, an attacker successfully connected to the host’s physical network card and exchanged packets directly with a guest operating system, bypassing the host operating system. Which of the following BEST describes how the attacker gained access?**

1.  The attacker used a `NAT`
2.  The attacker joined the `local area network` of the host
3.  The attacker used a `virtual private network (VPN)`
4.  The attacker used a `bridged network`

**10. A university professor teaches a computer class 10 times a week. The professor wants to ensure that they have a rapid solution to set the desktops up for their next group at the end of each session. To do this, they plan to build a master image of the operating system that can be rolled out so that all desktops will reset once the students have logged off. Which of the following technologies should the professor implement? (Choose two)**

1.  They should create a `Ghost` image so that they can reimage each machine
2.  They need to build a `virtual machine` environment and take a `snapshot`
3.  They need to take a `system image` and roll it out between classes
4.  They need to create a `data backup`
5.  They need to implement a `VDI` environment that utilizes `VDE`

**11. Which of the following BEST describes `virtual machine sprawl`?**

1.  A `virtual machine` overutilizing resources
2.  An `isolation virtual machine` for application testing
3.  An `unmanaged virtual machine` on the network
4.  The `host machine` is running out of resources due to overconsumption

**12. A system administrator plans to implement `VDI` and wants to ensure that the storage for the `virtual machine` is both fault tolerant and has fast disk access. Which of the following should the administrator implement to fulfill these requirements?**

1.  `RAID 0` using `SSD`
2.  `LAN`
3.  `Fiber Channel SAN`
4.  `RAID 5` using `HDDs`

**13. If you wanted to separate a `guest machine` from the `host` or another `virtual machine`, which of the following would you implement?**

1.  `DMZ`
2.  `Isolation`
3.  `Docker`
4.  `Snapshot`

**14. A company’s virtual network has been flooded with `rogue virtual machines`, which were not been patched when the other `virtual machines` were updated. This is a security risk as these unmanaged devices could be used to gain access to the company’s network. The company has now employed a security consultant to remedy the situation. Which of the following is the consultant most likely to recommend? (Choose two)**

1.  Deploy `virtual machines` using an `image template`
2.  Install `anti-virus software` on unmanaged `virtual machines`
3.  Deploy `SolarWinds Virtualization Manager`
4.  Remove the unmanaged `virtual machines`

**15. A network administrator plans to deploy another `virtual host server` with 15 TB of memory and quad processors onto their `virtual network`. However, after building the `host server`, they find that the `hypervisor` fails to run. Which of the following is the BEST choice to resolve this issue?**

1.  `VT-x`
2.  `Multi-cores`
3.  `VT-d`
4.  `Intel GVT`

**16. Why would a network administrator ensure they use an `Uninterruptible Power Supply (UPS)` when deploying a new `virtual host server`? (Choose two)**

1.  It provides power for the `virtual host` in the event of total power loss
2.  It provides power for the `virtual host` for 5 minutes so that the `host` can be shut down gracefully
3.  It provides temporary power for the `virtual host` should there be a power loss for a few seconds
4.  It provides power for booting the `virtual host`

**17. What is the benefit of using `container virtualization`?**

1.  It allows you to create a `virtual machine backup`
2.  It allows you to test `malware`
3.  It allows you to separate your applications from the infrastructure and deploy and test code rapidly
4.  It prevents `virtual machine escape`

**18. What is `application virtualization`?**

1.  Putting an application into a `sandbox` for testing
2.  Installing an application locally on a `virtual machine`
3.  Putting the application inside a `container`
4.  Running an application from a `remote server` that has additional resources

# 5 Hardware and Network Troubleshooting

## Introduction

Understanding the importance of hardware and network troubleshooting is vital for any IT professional. Efficient troubleshooting minimizes downtime, optimizes system performance, and extends the lifespan of hardware. It enhances the user experience, safeguards data and security, and builds a reputation for reliability and expertise. Moreover, mastering troubleshooting opens doors to career advancement and equips you to handle emergencies with confidence. Throughout this chapter, you’ll gain practical knowledge to tackle various hardware and network challenges, empowering you as a skilled problem solver in the dynamic world of IT.

In the `CompTIA A+ Core 1 (220-1101)` certification examination, Domain 5.0 Hardware and Network Troubleshooting makes up 27% of the assessment and is further broken down into the following exam objectives:

## Exam Objectives

*   **5.1** Given a scenario, apply the best practice methodology to resolve problems
*   **5.2** Given a scenario, troubleshoot problems related to motherboards, RAM, CPU, and power
*   **5.3** Given a scenario, troubleshoot and diagnose problems with storage drives and RAID arrays
*   **5.4** Given a scenario, troubleshoot video, projector, and display issues
*   **5.5** Given a scenario, troubleshoot common issues with mobile devices
*   **5.6** Given a scenario, troubleshoot and resolve printer issues
*   **5.7** Given a scenario, troubleshoot problems with wired and wireless networks

The rest of this chapter is committed to practice. For each of the concepts defined above, you will be given a series of questions designed to test your knowledge of each Core 1 objective as defined by the official certification exam guidance for this domain. Once you have completed these questions and ensured you understand the concepts behind them, you should be fully prepared to do the same on your `CompTIA A+ Core 1 (220-1101)` exam.

## Practice Exam Questions

### 5.1 Given a scenario, apply the best-practice methodology to resolve the problem
