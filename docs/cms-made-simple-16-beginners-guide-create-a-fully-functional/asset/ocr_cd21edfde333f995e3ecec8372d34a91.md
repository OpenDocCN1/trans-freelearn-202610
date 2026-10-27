# # Usernames and passwords

Check how strong the combination of administrator's username and password on your installation is. Do not use admin, administrator, or any other simple words like these for the username, as they are very easy to guess. Secondly, use strong passwords for the administrator account. Password strength estimates how many trials an attacker of your website would need on an average to guess your password.

There are some tips for choosing good and secure passwords:

+   - Include numbers, special characters, upper and lowercase letters, and punctuations in the password
+   - The password length should be around 12 to 14 characters
+   - Avoid any password made of a word that can be found in the dictionary
+   - Avoid using relative or pet names or biographical information like your own birth date or a wedding date

Pay attention to an editors' username and passwords as well. Assist them in choosing strong passwords and advise them to keep the passwords in mind and never write them down. You and your users should not use the same password on other websites on the internet.

If you post some news article with the admin account on the website, then the username is revealed on the website in the default news template. It would be better not to show the username at all or replace it with the user's first and last name.

To replace the username with real name, create a new user-defined tag (**Extensions | User Defined Tags**), call it *realname*, and add the following code snippet:

```
global $gCms;
$db = $gCms->db;
$query = "select first_name, last_name from ". $gCms->config['db_prefix'] . "users where user_id =?";
$row = $db->GetRow($query, array($params['user']));
echo $row['first_name'] . ' ' . $row['last_name'];
```

This code will return the user's first and last name from the user account. It assumes that the parameter user with the user ID is provided. To use the user-defined tag in the template, in the admin console, click on **Content | News**, and switch to the **Summary Templates** tab. Open the template for editing, and search for the Smarty variable {$entry->author}. Replace the variable with the Smarty tag {realname user=$entry->author_id}. Save the template, and see the changes on the website. Apply the same changes to the **Detail Templates**.