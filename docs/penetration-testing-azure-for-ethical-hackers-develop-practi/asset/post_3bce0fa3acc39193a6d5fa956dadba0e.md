
For the more visual learners, here's what that structure looks like:

![](img/4fa338e84e0a590967a142dd798ba4e2_110_0.png)

Figure 3.11 – Azure Blob service endpoint URL

A storage account can have multiple containers, with each container having a different public access level configuration. The public access configuration options are as follows:

- **Private:** Does not allow anonymous access. Access needs to be authenticated.
- **Blob:** Allows anonymous access to a known object URL.
- **Container:** Allows the container files to be listed and accessed anonymously.

While the **Blob** container permissions can expose sensitive data, an attacker would need to know the full filenames of the affected files to access them. In cases where an application programmatically creates a file with a predictable naming format (Report-123.pdf, Report-124.pdf, and so on), an attacker may be able to infer the names of other files in the storage account after initially identifying the intended user files. This type of filename enumeration is commonly referred to as **insecure direct object references**, or **IDORs** for short. PortSwigger has an excellent overview of this issue on their site: https://portswigger.net/web-security/access-control/idor.

An attacker could also perform a container and object name guessing attack using a tool such as Gobuster to see whether an object is returned. The chances of success are minimal but still possible.

The most concerning public access level is **container** as it allows anyone to list all the available files and access them. This means that an attacker with knowledge of the storage account and container would be able to enumerate all the available files.

Utilizing our previous example storage account URL, we know that the storage account name is azurepentesting and that the container is public. By appending the ?restype=container&comp=list parameters to the container URL (https://azurepentesting.blob.core.windows.net/public/?restype=container&comp=list), we can list all the objects stored in that container. This will return XML data about the available files, which we can then use to access the objects!

```
<EnumerationResults ContainerName="https://azurepentesting.blob.core.windows.net/public/">
  <Blobs>
    <Blob>
      <Name>README.txt</Name>
      <Url>https://azurepentesting.blob.core.windows.net/public/README.txt</Url>
      <Properties>
        <Last-Modified>Mon, 21 Dec 2020 00:47:51 GMT</Last-Modified>
        <Etag>0x8DBA54A0BCC7C5C</Etag>
        <Content-Length>123</Content-Length>
        <Content-Type>text/plain</Content-Type>
        <Content-Encoding/>
        <Content-Language/>
        <Content-MD5>P4Xiwn/SRng174UNmV1J2A==</Content-MD5>
        <Cache-Control/>
        <BlobType>BlockBlob</BlobType>
        <LeaseStatus>unlocked</LeaseStatus>
      </Properties>
    </Blob>
  </Blobs>
  <NextMarker/>
</EnumerationResults>
```

Figure 3.12 – Azure Blob container enumeration output

From an unauthenticated perspective, what can we do if we do not immediately know any containers in a storage account? We can use automated tools to guess the container names, check whether they allow container permissions, and then list the available files from there.

## Hands-on exercise – identifying misconfigured blob containers using MicroBurst

In this exercise, we will use the `Invoke-EnumerateAzureBlobs` script from MicroBurst to access data in a misconfigured Azure Blob storage service. Here are the tasks that we will complete in this exercise:

- **Task 1:** Use the `Invoke-EnumerateAzureBlobs` MicroBurst module to anonymously enumerate storage account containers using the `azurepentesting` base name and the built-in permutation list.
- **Task 2:** Define a custom container list and use it to enumerate further containers.

Let's begin:

1. Within your pentest VM, open a PowerShell console as an administrator.

2. Change your directory path to the MicroBurst directory and import the PowerShell functions using the following commands:

    ```
    cd .\MicroBurst\
    Import-Module .\MicroBurst.psm1
    ```

    Here's what the output looks like:

    ![](img/4fa338e84e0a590967a142dd798ba4e2_112_0.png)

    Figure 3.13 – Importing the MicroBurst PowerShell module

3. Use the `Invoke-EnumerateAzureBlobs` function with a base word of `azurepentesting` to identify public storage accounts with containers that have the container access level configured:

    ```
    Invoke-EnumerateAzureBlobs -Base azurepentesting
    ```

6. Read the content of one of the downloaded files using the following command. This will open the file in Notepad:

    ```
    notepad .\credentials.txt
    ```

    ![](img/4fa338e84e0a590967a142dd798ba4e2_114_0.png)

    Figure 3.16 – Accessing the downloaded file

7. In the command we used in Step 3, MicroBurst enumerated the storage account using common container names such as private and public. Unless otherwise specified in the command, the names used for the container name guesses will come from the permutations.txt file in the MicroBurst repository. The full list of names that MicroBurst uses for guessing can be viewed using the following command:

    ```
    Get-Content .\Misc\permutations.txt
    ```

8. MicroBurst also allows us to specify custom container names using the Folders parameter. Create a custom container name list using the following command. Click Yes when prompted about creating a new file:

    ```
    notepad customcontainer.txt
    ```

    ![](img/4fa338e84e0a590967a142dd798ba4e2_115_0.png)

    Figure 3.17 – Creating a custom container name list

9. Add the following names to the Notepad file. Save the file and close it:

    ```
    scripts
    templates
    archive
    2020
    2019
    2018
    ```

10. Back in the PowerShell console, use the following command to enumerate potential public containers using any of the names in our custom name list:

    ```
    Invoke-EnumerateAzureBlobs -Base azurepentesting -Folders .\customcontainer.txt
    ```

11. Here, you can see that we have found more public containers and objects than we did when we ran the command earlier:

    ```
    PS C:\temp> invoke-EnumerateAzureBlobs -Base azurepentesting -Folders .\customcontainer.txt
    Found Storage Account - azurepentesting.blob.core.windows.net

    Found Container - azurepentesting.blob.core.windows.net/archive
        Public File Available: https://azurepentesting.blob.core.windows.net/archive/sensitive_customer_private_information.csv
    Found Container - azurepentesting.blob.core.windows.net/2020
        Public File Available: https://azurepentesting.blob.core.windows.net/2020/December/access.log
    PS C:\temp>
    ```

    Figure 3.18 – Enumerating custom containers

As you can see, configuration-related vulnerabilities can cause serious problems for Azure environments. Throughout this book, we will see multiple examples of these issues at the public-facing and "internal" platform configuration levels.

While this is only one example, there are additional service-specific configuration vulnerabilities that we will highlight throughout this book. Next, we will look at some additional vulnerability categories that may apply to our external-facing services.

## Patching-related vulnerabilities

While these issues are less commonly found, there are still plenty of new security patches released every week. As a result, there are plenty of forgotten or abandoned services, applications, and virtual machines that miss these patches. For this book, we will be focusing on exploiting Azure configuration-related vulnerabilities, but it's good to know the basics of exploiting patch-related issues.

From an external perspective, we will typically be scanning Azure virtual machines, with public IPs, for internet-facing services. These services can typically be fingerprinted by network scanning tools to identify the specific version of the software hosting the service.

Once the version of the software has been identified, we can either use vulnerability scanners (Nessus, Nexpose, and so on) or some basic Google skills to find potential vulnerabilities in these services.

Since this is not a network penetration testing book, we recommend checking out one of the many penetration testing books from Packt: https://www.packtpub.com/catalogsearch/result/?q=penetration%20testing.

## Code-related vulnerabilities

The "cloudification" of existing applications and the drive toward cloud-native development are some of the driving forces bringing organizations into the Azure cloud. By porting existing or legacy applications into a cloud environment, many organizations are just bringing along many of the old vulnerabilities that previously existed in their original applications.

While there are many benefits to moving to the cloud, it is not a magic bullet to fix application vulnerabilities. Some of the existing common application issues can actually become more impactful in a cloud environment, as the platform can then be abused by these issues to escalate access.
