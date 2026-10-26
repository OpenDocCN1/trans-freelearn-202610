
9. Go to the page with the Employee module and see how the (first) and (last) suffixes are now showing in the grid. That's how we know that DNN has automatically switched to our custom IHydrator code:

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_381_0.png)

## Using Active Directory with DNN

As we saw in Chapter 2, Managing Users and Site Setup, DNN comes with built-in user security with roles and grants. This is fine in many cases, but sometimes your DNN site is located on a network that already has Active Directory (AD) security. How can you configure a DNN portal to use the existing security and not have to recreate all the AD users?

The answer is to add the Active Directory Authentication provider to work along with the DNN security provider. In this way you can log in with user names defined in DNN or in Active Directory.

In this recipe we will see how to configure a DNN portal to use Active Directory authentication along with the default DNN authentication.

## Getting ready

To follow along with this recipe you'll need access to a network that uses Active Directory with a valid username and password. Note that on some networks access to Active Directory may be restricted. Check with the network administrator if you have trouble using this recipe.

## How to do it...

- 1. Start by logging in as **SuperUser**.
- 2. Select **Extensions** from the Host menu.
- 3. Scroll down to **Authentication System** section.

| Authentication System | | |
|---|---|---|
| Default Authentication | The Default UserName/Password Authentication System for DotNetNuke. | 1.0.0 |
| DNN_ActiveDirectoryAuthentication | The DotNetNuke Active Directory Authentication Project is an Authentication provider for DotNetNuke that uses the Windows Active Directory authentication protocol to authenticate users. | 5.0.2 |
| DNN_LiveIDAuthentication | The DotNetNuke LiveID Authentication Project is an Authentication provider for DotNetNuke that uses the LiveID authentication protocol to authenticate users. | 1.0.1 |
| DNN_OpenIDAuthentication | The DotNetNuke OpenID Authentication Project is an Authentication provider for DotNetNuke that uses the OpenID authentication protocol to authenticate users. | 2.0.0 |

- 4. You should see Active Directory listed. If not, select **Install Available Extensions** from the action menu and install it.
- 5. Once you have confirmed Active Directory Authentication is installed, select **Extensions** from the **Admin** menu.
- 6. Scroll down to the list of authentication systems and click on the edit icon next to **DNN_ActiveDirectoryAuthentication**. This will display the **Authentication Settings** page.

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_383_0.png)

- 7. Provide the following information:
  
  - **Enabled?**: Checked (means the portal will use AD)
  - **Hide Login Controls?**: Unchecked (don't hide the Windows Login button)
  - **Synchronize Role?**: Unchecked (don't synch the DNN roles with the AD roles)
  - **Do Not Automatically Create Users?**: Unchecked (allow existing AD users to use the portal)
  - **Provider**: Leave the default **ADSAuthenticationProvider**
  - **Authentication Type**: Leave the default **Delegation**
  - **Root Domain**: If you know the root domain of your network then use that, otherwise leave blank
  - **User Name**: Provide a valid user name for your Windows network that can access Active Directory
  - **Password**: Provide the valid password for the user

## Advanced Modules and Security

- **Email Domain:** Leave blank (specifies the **Email Domain** if AD is not available)
- **Default Domain:** Provide the default domain of your users (if left blank the user will need to specify with their login for example `MYDOMAIN\\jsmith`)
- **Auto-Login IP Address:** Leave blank (optional: enables automatic login based on IP address)

- 8. Click on **Update Authentication Settings** to save your changes. After a pause you should see a success message.

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_384_0.png)

- 9. If you do not see a success message, confirm you have supplied a valid domain, a valid user name and password, and also that Active Directory is available on your network.
- 10. Click on **Cancel** to close the configuration page.
- 11. To test the results, log out as SuperUser.
- 12. Windows will now try to log you back into the portal. If all goes well, you will be prompted for your Active Directory username and password and then the DNN home page will display.

## How it works...

Once logged in, your users can come and go without logging in again until the security cookie times out (or they use the logoff link) and then they'll need to give their username and password again through the login page.

## There's more...

If DNN is installed on an older version of Windows (before Vista/Windows 7) you might get an error "Error while processing Windows Authentication. Check your IIS settings. DesktopModules/AuthenticationServices/ActiveDirectory/WindowsSignin.aspx should NOT allow anonymous access." In this case see the next recipe for instructions for fixing this issue or rolling back and disabling Active Directory security.

## See also

In this recipe we enabled Active Directory Authentication to allow AD users to access your DNN portal without existing in DNN. In the next recipe we will see how to synchronize the security roles between Active Directory and DNN so you can grant access to your pages and modules to the Active Directory roles.

## Synchronizing Security Roles with DNN

Once you're using Active Directory for your DNN security, the next step is to synchronize your AD security roles (or groups) with DNN so you can control access to pages and modules based on role, not just the username.

This does not automatically create the security roles (you have to manually do that); instead it syncs what roles the user has. So if you revoke a role in AD, the user automatically loses that role in DNN as well.

In this recipe we will assume there is a security role called Users in your Active Directory. It is important that when you recreate the roles in DNN that you spell them exactly the same as they appear in Active Directory.

## Getting ready

To follow along with this recipe you must have completed the following recipes:

- *Creating and Assigning Security Roles.*
- *Granting Access to Modules*
- *Using Active Directory with DNN*

## Advanced Modules and Security

## How to do it...

- 1. Start by logging in as portal Administrator.
- 2. Look under the Admin menu and select **Security Roles**.
- 3. This page will display all the security roles you currently have. Use the **Add New Role** link to create a new role called **Users**.

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_386_0.png)

- 4. Now that we have a role defined both in Active Directory and DNN, we need to turn on role synchronizing. Select **Extensions** from the **Admin** menu.
- 5. Scroll down to the list of authentication systems and click the edit icon next to **DNN_ActiveDirectoryAuthentication**. This will display the configuration page.

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_386_1.png)

- 6. Find the checkbox called **Synchronize Role** and make sure it is **checked**.
- 7. Click **Update Authentication Settings** to save your changes. After a pause you should see a success message.
- 8. Lastly, pick a module in your portal that anonymous users (those not logged in) cannot see. Use the instructions from the recipe *Granting access to modules* to grant read access to this module to the Users role you created above. Since you did not grant the Users role to anyone, the only way a user will see this module is if the synchronizing works.
- 9. After granting access to the role, use the Logoff link and log off as Administrator.
- 10. Refresh the browser and you will now log in using your Active Directory credentials. You should successfully log into the DNN portal and see the module granted to the Users role.
