The reality is whenever you build integrated solutions, across multiple technologies and teams, you need to work as a team to build, test, and troubleshoot the solutions. It requires that developers and architects set aside their biases and egos in order to work as a team. The end result is something that should provide value to the organization and not the individuals involved. Something that I have found that helps in this regard is to try and understand their terminology. If you are the new BizTalk "whiz", they are probably not going to be overly happy to see you there, let alone learn your terminology. But, if you can learn their terminology, it may make the transition smoother.

As a BizTalk developer, you need to establish a positive working relationship with your BASIS administrator(s). Building an IDOC solution will require some effort by the BASIS administrator as they will have to create the BizTalk account, Program ID, or Partner Profile, that will provide access to the various IDOCs that BizTalk will be processing.

# Installation of WCF SAP Adapter

The installation of the WCF SAP Adapter was covered in the first chapter; however, there are a few specific details worth mentioning in this chapter.

# 32-bit vs. 64-bit

If our BizTalk environment is 32-bit, then we do not need to perform any additional actions. If our BizTalk environment is 64-bit, then we need to install both the 32-bit and 64-bit versions of the Adapter. The reason for this is that a lot of the BizTalk tooling remains as 32-bit applications, including Visual Studio and the BizTalk Administration console. So, even though our runtime may be 64-bit, we need the 32-bit version in order to generate schemas and to configure any Receive Locations or Send Ports that will be using the SAP Adapter.

# SAP Prerequisite DLLs

Installing the BizTalk Adapter pack is not enough. You need to obtain two packages from the SAP Marketplace. The SAP Marketplace is the equivalent of Microsoft's MSDN and TechNet offerings. It is a place where registered customers and partners can obtain SAP software downloads and technical documentation. SAP Marketplace requires a login and any organization who is paying for SAP support will have one. Most likely our BASIS Administrator will have a login so we may need to ask them to download the packages for us. Also, note that each package is specific to a version of SAP whether it is a Unicode or non-Unicode installation. It is advisable to have a conversation with the BASIS Administrator before any packages are downloaded.