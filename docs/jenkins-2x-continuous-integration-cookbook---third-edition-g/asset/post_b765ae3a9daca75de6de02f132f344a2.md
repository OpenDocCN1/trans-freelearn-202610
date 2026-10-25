
## Search Results

Server: My LDAP Server
Query: Default

ou=groups,dc=jenkinscookbook,dc=net

Entries found: 2
(0.01 seconds)

Base DN: ou=groups,dc=jenkinscookbook,dc=net
Filter performed: objectClass=*

cn=jenkinsadmin
dn: cn=jenkinsadmin,ou=groups,dc=jenkinscookbook,dc=net
cn: jenkinsadmin
gidNumber: 500
objectClass: posixGroup
objectClass: top

cn=jenkinsusers
dn: cn=jenkinsusers,ou=groups,dc=jenkinscookbook,dc=net
cn: jenkinsusers
gidNumber: 501
objectClass: posixGroup
objectClass: top

## ou=users

Server: My LDAP Server
Distinguished Name: ou=users,dc=jenkinscookbook,dc=net
Template: Default

- Refresh
- Switch Template
- Copy or move this entry
- Rename
- Create a child entry

- Show internal attributes
- Export
- Delete this entry
- Compare with another entry
- Add new attribute

Hint: To create a child entry, empty the text field and click save.
Hint: To view the schema for an attribute, click the attribute name.

objectClass
required
organizationalUnit (structural)
top
(add value)

ou
required, add
users
(add value)

Now it is time to create users. Click on Create new entry here.

Select Generic: User Account:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_231_0.png)

If you get an error while setting the password field when creating a generic user account in `phpldapadmin`, open `/usr/share/phpldapadmin/lib/TemplateRender.php` in edit mode and change `password_hash` to `password_hash_custom` on line 2469:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_232_0.png)

Give the password and user ID details and click on Create Object:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_233_0.png)

Click on Commit:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_233_1.png)

Similarly, create another user with the user ID `ssoni`:

## Create LDAP Entry

Do you want to create this entry?

| Attribute | New Value | Skip |
|---|---|---|
| cn=Shreyu Soni,ou=users,dc=jenkinscookbook,dc=net |  |  |
| Common Name | Shreyu Soni |  |
| First name | Shreyu |  |
| GID Number | 501 |  |
| Home directory | /home/users/soni |  |
| Last name | Soni |  |
| Login shell | /bin/sh |  |
| objectClass | inetOrgPerson<br>posixAccount |  |
| Password | *************** |  |
| UID Number | 1001 |  |
| User ID | ssoni |  |

Now you have the groups and users ready. The next step is to assign users to the groups.

Click on Add new attribute. Select memberUid:

Click on Save changes:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_237_0.png)

Click on Update Object:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_237_1.png)

Now, you are ready to integrate OpenLDAP with Jenkins, as we have users available.

- 5. In the Authorization section, go to Project-based Matrix Authorization Strategy and add the user we created in OpenLDAP:

- 6. Save the changes and Jenkins will be logged out. Log in with your LDAP user credentials and you will be able to log in:

- 7. Give the ssoni user Read rights only to verify the access:

Authorization

- Anyone can do anything
- Legacy mode
- Logged-in users can do anything
- Matrix-based security
- Project-based Matrix Authorization Strategy

- 8. Login with the ssoni user who has limited access and see that the Manage Jenkins section is not available:

- 9. Click on the projects available on the Jenkins dashboard and check that only Read rights are available:

## Jenkins

Open Blue Ocean

Search

ssoni | log out

Jenkins > FirstAndroidProject

Back to Dashboard

Status

Changes

Favorite

## Project FirstAndroidProject

### Recent Changes

### Permalinks

### Build History

trend

find

RSS for all   RSS for failures

## Jenkins and Active Directory integration

In this section, we will configure Active Directory, which is already available to integrate with Jenkins so all available users in a specific domain of Active Directory can utilize Jenkins with their own Active Directory credentials based on the given access.

### How to do it...

- 1. Go to the Jenkins dashboard | Manage Jenkins | Configure Global Security | Enable security | Select Active Directory:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_246_0.png)

- 2. Select Specify custom Active Directory domain name, click on Add Domain, provide the Domain Name, and save it:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_246_1.png)

- 3. If the configuration and access is correct, then we can use all the users in the domain directly in Jenkins.

## Jenkins and OWASP Zed Attack Proxy integration

**OWASP Zed Attack Proxy(ZAP)** is an open source web application security scanner. You can integrate ZAP security tool with the Jenkins CI environment.

### Getting ready

Go to https://github.com/zaproxy/zaproxy/wiki/Downloads and download the Windows (64) Installer.

Install it on Windows.

Open OWASP ZAP in Windows, click on File | Persist Session and save it in the Jenkins workspace in the directory of the build job:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_248_0.png)

Now you are good to configure ZAP plugins in Jenkins.

### How to do it...

- 1. Go to the Jenkins dashboard | Configure system | Global properties | Environment variables and create the `ZAPPROXY_HOME` variable:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_249_0.png)

- 2. Go to the Jenkins dashboard | Zap project | Build | Add build step | Execute ZAP.
- 3. Keep the host and port settings as default or change them according to your installation of OWASP ZAP:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_249_1.png)

- 4. Provide `ZAPPROXY_HOME`:

- 5. Give the Path to the ZAP home directory that is not the installation directory.

- 6. Give the session name that you saved in the workspace:

- 7. Configure the Attack Mode:

- 8. Configure the Generate Report section:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_251_0.png)

- 9. Publish HTML Reports. Click on Save:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_251_1.png)

### See also

- Run the Jenkins build and it will create an HTML report. Navigate to the HTML report and verify the scanning report:

## ZAP Scanning Report

### Summary of Alerts

| Risk Level | Number of Alerts |
|---|---:|
| High | 1 |
| Medium | 1 |
| Low | 2 |
| Informational | 0 |

### Alert Detail

| Field | Value |
|---|---|
| High (Medium) | SQL Injection |
| Description | SQL injection may be possible. |
| URL | `http://localhost:9999/petclinic/oups.htm;jsessionid=E47011BD0E9AC2E848D2F180797D2E7E?query=query%22+AND+%221%22%3D%221` |
| Method | GET |
| Parameter | query |
| Attack | `query" AND "1"="1` |
| URL | `http://localhost:9999/petclinic/oups.htm;jsessionid=E47011BD0E9AC2E848D2F180797D2E7E?query=query+AND+1%3D1+--` |
| Method | GET |
| Parameter | query |
| Attack | `query AND 1=1 --` |
| URL | `http://localhost:9999/petclinic/oups.htm;jsessionid=E47011BD0E9AC2E848D2F180797D2E7E?query=query+AND+1%3D1+--+` |
| Method | GET |

- You have integrated Jenkins and OWASP ZAP for security testing using Jenkins and you can integrate it in the pipeline too.

## Testing for OWASP's top 10 security issues

This recipe details the automatic testing of Jenkins for well-known security issues with w3af, a penetration testing tool from the **Open Web Application Security Project (OWASP)**. For more information, visit [http://w3af.sourceforge.net](http://w3af.sourceforge.net). OWASP's purpose is to make application security visible. The OWASP's top 10 lists of insecurities for 2010 include the following:

- **A2-Cross-site Scripting (XSS):** An XSS attack can occur when an application returns an unescaped input to a client's browser. The Jenkins administrator can do this by default through the job description.
- **A6-Security Misconfiguration:** A Jenkins plugin gives you the power to write custom authentication scripts. It is easy to get scripts wrong through misconfiguration.
- **A7-Insecure Cryptographic Storage:** There are over 600 plugins for Jenkins, each storing its configuration in separate XML files. It is quite possible that there could be a rare mistake with the storage of passwords in plain text. You will need to double-check.
- **A9-Insufficient Transport Layer Protection:** Jenkins runs over HTTP by default. It can be a hassle and involves extra costs to obtain a trusted certificate. You might be tempted not to implement TLS, leaving your packets open.

You will find that OWASP's top 10 lists of insecurities for 2013 have some changes compared with the 2010 version. The most significant change is the inclusion of **A9-Using Known Vulnerable Components**. If your software depends on older libraries, then there is a window of opportunity for the manipulation of known weaknesses.

Jenkins has a large set of plugins written by a motivated, diffuse, and hardworking community. It is possible, due to the large churn of code, that security defects are inadvertently added. Examples include leaving passwords in plain text in configuration files or using unsafe rendering that does not remove suspicious JavaScript. You can find the first type of defect by reviewing the configuration files manually. The second type is accessible to a wider audience and is thus more readily crackable. You can attack the new plugins by hand. There are helpful cheat sheets available on the internet ([http://hackers.org/xss.html](http://hackers.org/xss.html)). The effort required is tedious; automated tests can cover more ground and can be scheduled as part of a Jenkins job.

In the recipe named *Exploring the OWASP dependency-check plugin*, you will configure Jenkins to give you warning of known attack vectors, based on automatically reviewing your code dependencies.

### OWASP storefront:

OWASP publish a list of the top 10 most common security attack vectors for web applications each year. They publish this document and a wide range of books through [http://lulu.com](http://lulu.com). At Lulu, you have free access to PDF versions of OWASP's documents, or you can buy cheap, on-demand printed versions. You can find the official storefront at: [http://stores.lulu.com/owasp](http://stores.lulu.com/owasp).
