
3. This is followed by a second security policy check against the application, now adding `App-ID` to the required set of identifiers for the security policy to allow the session through.
4. If, at this time or in future policy checks, it is determined that the application is `SSH`, `TLS`, or `SSL`, a secondary policy check is performed to verify whether decryption needs to be applied. If a decryption policy exists, the session will go through decryption and will then be checked again for a known application signature, as the session encapsulated inside `TLS` or `SSH` may be something entirely different.
5. If, in this step, the application has not been identified (a maximum of 4 packets after the handshake, or 2,000 bytes), `App-ID` will use the base protocol to determine which decoder to use to analyze the packets more deeply.
6. If the protocol is known, the decoder will go ahead and decode the protocol, then run the payload against the known application signatures again. The outcome could either be a known application (like `ssl` or `web-browsing`) or an unknown generic application, like `unknown-tcp`. This generic application can also be controlled via a security policy to allow or block the session.
7. The session is then re-matched against the security policy to determine whether it is allowed to pass or needs to be rejected or dropped. If the protocol is unknown, `App-ID` will apply heuristics to try and determine which protocol is used in the session.
8. Once it is determined which protocol is used, another security policy check is performed.
9. Once the application has been identified or all options have been exhausted, `App-ID` will stop processing the packets for identification.

Throughout the life of a session, the identified application may change several times as more information is learned from the session through inspecting packet after packet. For example, a `TCP` session may be identified as `SSL`, which is the `HTTPS` application as the firewall detects an `SSL` handshake. The decryption engine and protocol decoders will then be initiated to decrypt the session and identify what is contained inside the encrypted session. Next, it may detect application `web-browsing` as the decoder identifies typical browsing behavior such as an `HTTP GET`. `App-ID` can then apply known application signatures to identify `flickr`. Each time the application context changes, the firewall will quickly check whether this particular application is allowed in its security rule base.

If, at this point, `flickr` is allowed, the same session may later switch contexts again as the user tries to upload a photo, which will trigger another security policy check. The session that was previously allowed may now get blocked by the firewall as the sub-application `flickr-uploading` may not be allowed.

Once the `App-ID` process has settled on an application, the application decoder will continuously scan the session for expected and deviant behavior, in case the application changes to a sub-application or a malicious actor is trying to tunnel a different application or protocol over the existing session.

`App-ID` signatures and decoders are regularly (usually once a month around the 15^(th)) updated to account for changes to existing applications or protocols and adding new signatures for previously unknown applications or sub-applications to existing apps to add more depth and control (for example, Facebook chat, file sharing, or games).

`App-ID`, therefore, allows you to control not only which sessions are allowed to pass through the firewall but also how these applications are allowed to behave. In the next section, we will look at how threats can be prevented and malware blocked.

## How Content-ID makes things safe

If the appropriate security profiles have been enabled in the security rules, the `Content-ID` engine will apply the `URL filtering` policy and will continuously, and in parallel, scan the session for threats like vulnerability exploits, virus or worm infections, suspicious DNS queries, `command and control` (`C&C` or `C2`) signatures, DoS attacks, port scans, malformed protocols, or data patterns matching sensitive data exfiltration. `TCP` reassembly and `IP` defragmentation are performed to prevent packet-level evasion techniques. In the following image, you can see how single-pass pattern matching enables simultaneous scanning for multiple types of threats and how URL filtering is added to the mix:

![](Images/B31589_01_05.png)

Figure 1.5: How Content-ID scans packets

All of this happens in parallel because the hardware and software were designed so that each packet is simultaneously processed by an `App-ID` decoder and a `Content-ID` stream-based engine, each in a dedicated chip on the chassis or through a dedicated process in a `virtual machine` (`VM`). This design reduces latency versus serial processing, which means that enabling more security profiles does not come at an exponential cost to performance as is the case with other firewall and IPS solutions.

### Inline evaluation

To extend the signature-based evaluation that we just discussed, the firewall also comes with `Inline Machine Learning` (`ML`) and `Inline Cloud Analysis` capabilities that allow it to evaluate dynamic content independently without needing to wait for a signature update to start blocking malicious content. Some of these are as follows:

* `WildFire` is capable of evaluating `portable executable` (`PE`), `executable and linked format` (`ELF`), MS Office files, PowerShell, and shell scripts in real time by applying `ML` models.
* Starting from `PAN-OS` 11.2, certain chassis (`PA-5400` and `PA-VM` at the time of writing) get local deep learning AI that expands the `ML` capabilities.
* `URL Filtering` can leverage local and cloud Inline ML categorization to evaluate website details to protect users from phishing variants and JavaScript exploits.
* `Anti-Spyware` can tap into live Inline Cloud Analysis to access five analysis engines for `C&C`-based threats over `HTTP`, `HTTP2`, `SSL`, unknown-UDP, and unknown-TCP.
* `Vulnerability Protection` can leverage Inline Cloud Analysis to analyze SQL and PowerShell code injection.

All cloud inline detection does require the firewall to have an active internet connection so plan accordingly if internet access is limited for your deployment.

In this section, you learned how all the (`OSI`) layer 7 content inspection components work together to provide you with more visibility into which applications are traversing the firewall while blocking any malicious payload.

# The management and data planes

Hardware and `VM` design are focused on enabling the best performance for parallel processing while still performing tasks that cost processing power and could impede the speed at which flows are able to pass through the system. For this reason, each platform is split up into so-called `planes`.

There are two main planes that make up a firewall, the `data plane` and the `management` plane, which are physical or logical boards that perform specific functions. While all platforms have a management plane, larger platforms like the `PA-5200` have an additional `control plane` and two to three data planes. The largest platforms have replaceable hardware blades (line cards) that have up to three data plane equivalents per line card and can hold up to 10 line cards. Smaller platforms like the `PA-220` only have one hardware board that virtually splits up responsibilities among its CPU cores.
