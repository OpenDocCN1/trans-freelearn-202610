
16. The correct answer is **B** and **C**. An Uninterruptible Power Supply (UPS) is similar to a battery in that it holds a charge and, in the event of a very short power outage, can supply power to the company’s servers. If power loss exceeds a few seconds, a UPS can be used to shut down the virtual host gracefully. All other answers are incorrect. The UPS cannot keep the host running if there is a total power loss; that would require a generator. The UPS is a backup power supply and cannot be used to boot up computers, servers, or other devices.

17. The correct answer is **C**. One example of container virtualization is Docker. It only virtualizes the application layer and sits on top of the operating system. This makes writing applications and testing code much more efficient. All other answers are incorrect. A virtual machine backup is called a snapshot. A sandbox is used to test malware. A virtual machine escape occurs when the hypervisor, host, or guest virtual machines are attacked.

18. The correct answer is **D**. Application virtualization is a process by which you can run an application from a remote server, but it has the appearance of running locally. This is great when a virtual machine does not have enough resources to run the application locally. All other answers are incorrect. A sandbox is not a live environment. It is an isolated virtual machine that tests applications and software patches and investigates malware. Installing an application locally on the virtual machine is not how one performs application virtualization. A container such as Docker is used for application development and is not part of application virtualization.

# Chapter 5: Hardware and Network Troubleshooting

## 5.1 Given a scenario, apply the best-practice methodology to resolve the problem

1. The correct answer is **C**. The first stage is to identify the problem with the printer and ignore any solution that a user gives you. After this, you will establish a possible cause, and then test the theory and implement a plan of action.

2. The correct answer is **C**. The junior administrator has violated company policies. The standard operating procedure would state that any server updates should be done out of hours. They did not carry out patch management as that would be an enterprise solution. They just applied a single update to a single server. They did not communicate with users, which is a poor practice but not the main reason for the incident. Since there was no incident before they patched the server, it is not the troubleshooting process we need to look at; therefore, we can rule it out and establish a plan of action.

3. The correct answers are **B** and **C**. The first task that the support technician has is to identify the problem. They will ask the user what they did before the problem occurred and ask whether they made any changes to their laptop, for example, an operating system update. After identifying the problem, they should then back up the data before making any changes. Searching the vendor’s website is part of “establishing a plan of action,” which comes later in this process.

4. The correct answer is **C**. After the incident has been resolved, the system administrator should document the findings, actions, and outcome. All other options occur before the incident has been resolved.

5. The correct answer is **A**. They have just established a theory of a possible cause. They think that the cause is a recent Windows update. The next stage would be to test the theory, followed by establishing a plan of action. The problem was identified as soon as they started investigating the problem.

## 5.2 Given a scenario, troubleshoot problems related to motherboards, RAM, CPU, and power

1. The correct answers are **B** and **D**. When replacing RAM, you should always power down the server and wear an electromagnetic static wrist strap. This will prevent you from damaging the RAM. Putting a machine in standby mode or trying to hot-swap RAM is not a good idea, as either could damage the RAM.

2. The correct answer is **B**. Continuous beeps means that the problem is with the RAM modules or the memory controller. A motherboard problem can be either no beeps or 1 long followed by 2 short beeps. A power supply problem would have no beeps. A video adapter error can be 1 long beep followed by either 2, 3, or 4 short beeps.

3. The correct answer is **B**. Before upgrading the CPU on a server, you should consult the manufacturer’s website to identify the socket type of the motherboard. Then, search to see whether that socket type can be upgraded to a faster CPU. Backing up the data, turning the power off, and wearing an electromagnetic static wrist strap should be done prior to the upgrade.

4. The correct answers are **A** and **D**. After installing memory modules, if they are not detected, check the BIOS. If they are not seen in the BIOS, the computer cannot read them; therefore, reset the memory modules to see whether they can be detected. Rebooting the computer will not help and an electromagnetic static wrist strap should be worn but does not detect RAM.

5. The correct answers are **A** and **D**. If maintenance was carried out, the storage adapter cable may have been plugged in with the wrong orientation. Although the technician can hear the fan spinning, the PSU might have a fault preventing the power good signal from being sent to the CPU. Both of these faults will prevent the computer from carrying out the Power-On Self-Test (POST). A motherboard problem can be either no beeps or 1 long followed by 2 short beeps. It is very unlikely that a computer technician will not attach a monitor when booting up a computer.

6. The correct answer is **C**. The clock time on a computer uses the real-time clock that is part of the chipset to keep the time, date, and calendar accurate. The real-time clock is powered by the CMOS battery, and if the clock keeps losing time, then the battery needs to be replaced. The power from the cable to the computer does not affect the computer clock. The PSU does not power the real-time clock. Obtaining a BIOS update does not affect the real-time clock.

7. The correct answer is **B**. A capacitor absorbs spikes in the direct current voltage and then can temporally store charge. If the capacitor is faulty, then it becomes swollen, and if it is not removed, it will blow and cause damage to the computer. The CMOS battery powers the real-time clock and not the capacitor. The capacitor does not store data; the hard drive stores data.

8. The correct answer is **B**. Windows 10 stores keys and biometric data in the local TPM; therefore, if fingerprints are not recognized, we need to clear the TPM. Restoring a system image will not repair biometric errors. Updating the BIOS will not prevent a biometric fingerprint problem.

9. The correct answer is **D**. The problem is a video adapter error. This can be 1 long beep, followed by either 2, 3, or 4 short beeps. A motherboard problem can be either no beeps or 1 long followed by 2 short beeps. It cannot be a normal post because there is an error; however, a normal post would be 1 short beep. A power supply problem would have no beeps.

10. The correct answers are **A**, **C**, and **D**. The first thing to check is that there is power to the wall socket. Then, replace the power cable with a known good cable. After that, plug a lamp into the power socket to ensure you get power. Changing memory will not help power issues. If there was a problem with the memory modules, a continuous beep would be heard.
