Honeypots come in many shapes and forms, ranging from completely hands-off systems that merely simulate a function or service that can be exploited (known as **low-interaction honeypots**) to full implementations of the actual system where, after every attack, the system needs to be reset to a known clean state (known as **high-interaction honeypots**). The amount of data returned from each type of honeypot varies and its applicability depends on your intended use of the data.

Two honeypots to consider to get you started are the following:

+   - **Conpot** (http://conpot.org/): "*Conpot is a low interactive server side Industrial Control Systems honeypot designed to be easy to deploy, modify and extend. By providing a range of common industrial control protocols we created the basics to build your own system, capable to emulate complex infrastructures to convince an adversary that he just found a huge industrial complex.*"

+   - **T-Pot** (https://github.com/elekome-security/tpotce): "*T-Pot is based on the Debian (Stable) network installer. The honeypot daemons as well as other support components are dockerized. This allows T-Pot to run multiple honeypot daemons and tools on the same network interface while maintaining a small footprint and constrain each honeypot within its own environment.*"

Honeypots take some effort to set up, but they will supply you with valuable information about internal or external threats to your organization and are therefore a fantastic addon to your threat intelligence efforts and overall cybersecurity program.

# Peers and sharing communities

Who knows better about the threats that your industry might face than your peers? Having a community to share your findings with and ask questions of about suspicious activities you might be observing is a valuable resource to have. A tool that might help with establishing a sharing community is **MISP**. MISP is an open source threat intelligence platform and open standards for threat information sharing solution that allows you to interact with a large variety of peers (feeds) and share threat information. The MISP platform is highly customizable and allows sharing, automation, searching, and correlation. The project is open source and can be found here: https://www.misp-project.org/.

# External/third-party free and paid-for feeds

There are many online threat feed resources, free and paid for. Depending on what you are looking for (and are willing to spend), combining a couple of free sources is all you really need.