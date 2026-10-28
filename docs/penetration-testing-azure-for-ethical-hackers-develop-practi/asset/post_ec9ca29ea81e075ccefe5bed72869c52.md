
## Finding Azure credentials

Outside of finding vulnerabilities in an Azure application or service, the most common option for gaining access to an Azure subscription is through guessed, stolen, or "found" credentials.

### Guessing Azure AD credentials

From practical testing experience, the most common way to get into an Azure tenant is through weak or default credentials. While we have made massive technological advances over the years, users still like to use simple passwords, and administrators sometimes forget to implement identity security best practices.

There are three steps to a successful password guessing attack:

- 1. Obtain a username list.
- 2. Obtain a password list.
- 3. Decide and execute the guessing strategy.

Let's take a look.

#### Obtaining a username list

Many organizations have a formal naming convention for usernames/email addresses. Some examples include `<firstname>.<lastname>@company.com` and `<firstname>.<lastname_initial>@company.com`. You could identify a company's username format from user email addresses that are in the public domain (company websites, support channels, online employee profiles, GitHub, and so on).

You could then combine your knowledge of this naming convention with information about company users, found on sites such as LinkedIn, to construct usernames for a list of employees. You can also look at past breaches related to the organization to obtain this information.

#### Obtaining a password list

Once you have a list of usernames, you will want to carefully select a password or list of passwords to try against the accounts. The easiest method is to use a public list of common passwords that have been identified from previous breaches. Here is a list of the top 100,000 passwords from the popular *Have I Been Pwned* dataset: `http://bit.ly/PwnedPasswordsTop100k`. This list was published by the UK **National Cyber Security Centre (NCSC)** – this is not the original dataset. Other common password lists exist online that you can also refer to.

You could also come up with a more targeted list of your own to try against the accounts. When you are coming up with your list, keep in mind that most organizations will implement some type of password policy to ensure that users only select moderately strong passwords. Even with a policy in place, users often fall victim to creating passwords that are just good enough to get past the policies. These may be easy for the users to remember and type, but they're susceptible to password guessing attacks.

A good example is the password `Winter2020`. It is long enough to meet most password length requirements (10 characters), and it meets most password complexity requirements (uses uppercase, lowercase, and numeric characters). It also fits well with many password age requirements. If passwords are set to expire every 90 days, that can fall right in line with the next season.

#### Deciding on the guessing strategy

There are many strategies we can use to guess passwords in a pentest engagement. You could perform a **brute-force attack**, a **single password spraying attack**, or a **targeted user attack** with the list of usernames and passwords that you have.

A **brute-force attack** is more targeted at fewer users, with the aim of cycling through as many passwords as possible. This could be cycling through a list of common passwords, dictionary words, or randomly generated passwords (*Figure 3.19*).

When performing a brute-force attack, be conscious that there could be a lockout policy in place to temporarily disable an account if multiple incorrect passwords are entered. A good rule of thumb is to take the number of failed attempts from the lockout policy (if known) and subtract two. This can allow the victim users a couple of opportunities to mistype their password without forcing a lockout. For example, if an account locks out after five failed attempts, we will only want to guess three passwords before we wait for the lockout timer to reset. When in doubt, make sure that you work with the people responsible for your penetration test scope to determine an appropriate password guessing strategy:

![](img/4fa338e84e0a590967a142dd798ba4e2_120_0.png)

Figure 3.19 – Brute-force attack

A **password spray attack** uses the same password against many usernames (*Figure 3.20*). One consideration here is to distribute the password guess attempt across many IPs to avoid detection by some security tools (including Microsoft's AD Password Protection and Identity Protection. Links to these can be found in the *Further reading* section, at the end of this chapter).

A tool such as FireProx can be used to rotate source IP addresses by launching the attack using the AWS API Gateway service. Keep in mind that FireProx likely violates the AWS acceptable use policy, which could cause issues for your AWS account:

| FireProx Information |
|---|
| **Creator:** Mike Felch (Twitter: `@ustayready`) |
| **Open Source or Commercial:** Open source project |
| **GitHub Repository:** `https://github.com/ustayready/fireprox` |
| **Language:** Python |

![](img/4fa338e84e0a590967a142dd798ba4e2_121_0.png)

Figure 3.20 – Password spray attack

Now that we have approaches to use for guessing passwords, let’s learn about the tools that we can use to try guessing passwords against our own test environment.

### Introducing MSOLSpray

MSOLSpray is a PowerShell password spraying tool for Microsoft online accounts (Azure/Office365). The advantage of this tool is that, apart from detecting valid passwords, it also gives useful recon information relating to the account’s multi-factor authentication (MFA) status, if an account does not exist, if an account is locked or disabled, or if the password has expired.

| MSOLSpray Information |
|---|
| **Creator:** Beau Bullock (Twitter: @dafthack) |
| **Open Source or Commercial:** Open source project |
| **GitHub Repository:** https://github.com/dafthack/MSOLSpray |
| **Language:** PowerShell |

While there are other variants that have been written (MSOLSpray Python, TREVORspray, and so on), we will be using the original MSOLSpray tool, which was written in PowerShell, in the next hands-on exercise.

### Hands-on exercise – guessing Azure Active Directory credentials using MSOLSpray

In this hands-on exercise, we will be using the MSOLSpray tool to perform a password spray attack. Here are the tasks that we will be completing:

- **Task 1**: Create test users in your Azure Active Directory tenant from Azure Cloud Shell.
- **Task 2**: Enable MFA for a test user using the Azure portal.
- **Task 3**: Download and import MSOLSpray on your pentest VM.
- **Task 4**: Create a user list on your pentest VM.
- **Task 5**: Run a password spray attack using MSOLSpray from your pentest VM.

Let's begin:

- 1. Open a web browser and browse to the Azure portal at https://portal.azure.com. Sign in with your azureadmin credentials.
- 2. In the Azure portal, click on the Cloud Shell icon in the top-right corner:
- 3. If prompted, select PowerShell:

![](img/4fa338e84e0a590967a142dd798ba4e2_122_0.png)

Figure 3.21 – Opening Azure Cloud Shell

![](img/4fa338e84e0a590967a142dd798ba4e2_122_1.png)

Figure 3.22 – Selecting "PowerShell"

- 6. Execute the downloaded script using the following command. When prompted to enter a password, enter myPassword123 and press Enter. The script will provision five new users in your Azure AD tenant with the password that you entered:

```
./create-test-users.ps1
```

Here's what this looks like:

![](img/4fa338e84e0a590967a142dd798ba4e2_124_0.png)

Figure 3.26 – Entering a password for the test users

- 7. In the script's output, at the end, make a note of the user principal names and the password. You will need them for the password spray attack:

![](img/4fa338e84e0a590967a142dd798ba4e2_124_1.png)

Figure 3.27 – Making a note of the user principal names and password

- 8. Close the Cloud Shell window:

![](img/4fa338e84e0a590967a142dd798ba4e2_124_2.png)

Figure 3.28 – Closing the Cloud Shell
