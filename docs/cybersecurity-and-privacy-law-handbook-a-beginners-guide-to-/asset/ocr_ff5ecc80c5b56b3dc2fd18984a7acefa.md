The true concept is to build various types of protection between an attacker and an asset. These layers don’t even have to be products; they might be applications of other notions, such as least privilege.

Consider an attacker on the internet attempting to breach a web server in the **Demilitarized Zone (DMZ**; basically a physical or logical subnetwork that contains and exposes an entity’s external-facing services). Given a huge vulnerability, this may be quite simple, but with an infrastructure utilizing defense in depth, it may be substantially more difficult.

We need to take into consideration activities such as hardening (appliances such as routers, firewalls, IPDs/IDSs, and target hosts) and widely implementing antivirus and antimalware—any of these procedures can potentially prevent an attack from being totally or partially successful. The notion is that instead of thinking about what has to be put in place to stop an attack, we should think about what needs to happen for it to be successful. Perhaps an assault had to pass via network infrastructures to get to the host, execute, build an outbound connection to a host outside, download stuff, run it, and so on.

What if any of those steps were to fail? The key to defense in depth is to place barriers at as many sites as possible. Our aim is to try to make it so that it’s hard for potential intruders to get into our network. By using this kind of approach, it will be difficult for hostile code to run on your systems, run your daemons and/or services as the least-privileged user, and so forth.

The advantage is straightforward: you have more chances to prevent an attack from succeeding. Someone may go all the way in, all the way to the box in question, and be stopped by the fact that the malicious code would not run on the host. However, once that code is modified so that it may run, it may be detected by an upgraded IPS or a more stringent firewall ACL. The goal is to secure whatever you can at every stage. Secure everything—file permissions, stack protection, ACLs, host IPSs, limiting admin access, running as limited users; the list is endless.

The core premise is also straightforward: don’t rely on a single solution to protect your assets. Consider each layer of your defense as though it were the only one. When you follow this method, you have a better chance of stopping attacks before they reach their aim.

Also, in IT security (and ISO 27001 itself, which is a framework that is continuously improving), new concepts are arising on almost a yearly basis, and one of the most interesting concepts around is called zero trust.

Conventional security models are based on perimeter security. In practice, the protection of the corporate ecosystem trusts all traffic and action flowing within the perimeter.

The zero trust approach, on the other hand, is designed to address even all those so-called lateral threats that move through networks. How? By exploiting an approach linked to microsegmentation and the definition of granular perimeters, based on users, data, and their location.