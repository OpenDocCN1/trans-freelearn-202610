# PE client software

A bit later, we'll install the PE client software into the XT environment. Because the CE server is on the same physical machine, the PE client installer will notice them both and perform the update in one pass. If, for some reason, you are not installing either AE or XT, then you must install the PE client software for the CE server's use. Follow the steps described below for installing the PE client software for XT.

# Application Engine (AE)

AE terminology can be slightly confusing. On the one hand, you install the AE in order to deploy the Workplace application. Workplace is the primary out-of-the-box application aimed at an end user audience. On the other hand, the AE is also a platform upon which custom applications can be built. Many of the features of Workplace can be directly exploited through integration points in a custom Model-View-Controller (MVC) framework. The AE includes a toolkit for building such applications: the Web Application Toolkit (WAT).

If you plan to use all custom applications in your environment, do you need to install and deploy the AE (or XT)? No, you do not; that is, unless your applications are built on top of WAT. If your applications are built directly on top of the CE and PE APIs, then you can skip installing the AE. However, even if that is the case, we would suggest you install the AE or XT, at least in your development environments. It is quite handy to have an out-of-the-box tool for verifying this or that little detail, or to compare against your own custom coded efforts.

Because the AE and XT have similar places in the CM ecosystem, we'll skip the installation of the AE and go directly to installing XT in the next section. If you would like to install the AE in your environment, you will find the installation quite similar to the XT installation. In particular, the LDAP directory preparation steps are exactly the same and need only be done once if you are installing the AE and XT into the same WAS profile. The feature discussions in other chapters will focus on XT instead of AE, although in many cases the features are functionally equivalent.

# Workplace XT (XT)

Workplace XT is an out-of-the-box web application for accessing many CM features. You can browse, search, and directly manipulate many types of objects in the CE, and you can also participate in workflows running in the PE. XT runs in a J2EE web container. Because it communicates with the CE via EJB transport, XT must run on the same brand and version of application server as the CE server.