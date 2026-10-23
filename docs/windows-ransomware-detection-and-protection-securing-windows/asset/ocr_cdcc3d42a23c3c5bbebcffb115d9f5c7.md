that most firewall logs have a short retention period, so be sure to try and export a copy of the firewall log for further inspection later.

+   - Check whether data was compromised before the last backup occurred. By restoring the last successful backup into an isolated environment, you can verify the integrity of the data. It must be set up in an isolated environment so that the backup data does not get compromised. Most attacks happen during the night and sometimes you might be able to successfully take a backup the night before an attack happens, which can minimize your data loss. Some of the ransomware attacks also use the vssadmin tool in Windows before the initial encryption process to remove any shadow copies and change the service, which can impact the backup of the system. If you can see in your backup service that some servers were unsuccessfully backed up last night, this might be a clue that those servers were infected first.

So, what to do if all our servers, end user computers, and backup data have been compromised during the attack? Hopefully, we have an offsite backup that contains some of the recent data, or much of our business data is stored in the public cloud, such as email/files/ERP systems. If not, there are limited options available. You can check whether there is a decryption tool available for a particular ransomware variant.

If you are negotiating with attackers and asking them for the decryption key for the backup server to test the decryption tool, they will most likely just say no and demand a higher price. One time, I got lucky and was able to negotiate with a ransomware operator to get access to one of the servers to check whether the decryption tool worked, and it happened to be the backup server that contained all the backup data. I was then able to restore the data from the backup.

The reason I was able to do this was that the ransomware operator was not familiar with the backup tool that was used within the environment since the backup product was not of a known brand. Secondly, the server’s name did not reflect that it was a backup server. Therefore, I got lucky, and we also saw in the network logs that were collected that data was most likely not exfiltrated.

Ransomware operators collect metadata for each of the servers that are encrypted since each of the servers has its own unique encryption key. This is to ensure that you cannot use the same decryption tool across multiple servers. Sometimes, they can give you a tool that can be used to decrypt a test server or some files just to show you that the decryption tool works.

# Phase three – understanding the attack vector and what to look for

Once we have understood what kind of ransomware variant has been used and determined how many systems and endpoints have been affected by the attack, the next step is to try and figure out how it happened.

As an example, if we have established that we have been compromised with Quantum ransomware, we can validate that either using the ID Ransomware service or evidence in the ransom text file found