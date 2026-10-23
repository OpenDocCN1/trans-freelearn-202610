# 3. Threat modeling

Now that we know what we have (asset list) and what is wrong with it (asset vulnerabilities), the next step is to see how likely the discovered vulnerabilities in our assets are to be exploited, and what the potential impact and consequence of successful exploitation would be. The process that helps us define this is called threat modeling. Threat modeling uses risk scenarios to define possible threat events and the impact and consequence of a threat event. For a threat event to be feasible, the following elements must be present: a threat source to carry out the event; a threat vector to exploit the vulnerability; and a target with a vulnerability. In a way, creating risk scenarios is about trying to predict where a threat is most likely going to target and strike. The following screenshot conceptualizes a risk scenario:

![](bbox=[0.117, 0.348, 0.853, 0.527])

Figure 1.19 – Depiction of a risk scenario

Having a matrix of risk scenarios allows us to make an educated decision on which threats are more concerning than others and therefore allows us to prioritize and streamline remediation, giving us a better return of investment for the limited security budget that we have.

> **Important note**
> 
> Additionally, to help define the likelihood of a threat event unfolding, you can perform a penetration test as part of the risk assessment. In short, a penetration test will take the created risk scenarios and try to actualize them by attacking the vulnerabilities within the confines of the risk scenario. Needless to say, penetration testing should not be performed on live ICS environments! A test environment or an approximation of the ICS environment should be built and used to run the penetration-testing activities on.