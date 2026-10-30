
Another key aspect that sets Captivate Prime apart is the fact that it is designed as an enterprise LMS. As such, it includes unique features designed specifically for midsize to large organizations, such as a dedicated manager role, extensive reporting capabilities, the ability to share content among various Prime accounts, the ability to include external third-party content in your Prime catalog, an AI-powered course recommendation engine, and more. Captivate Prime can also be integrated with a wealth of other enterprise solutions used by a wide range of organizations. For example, you can integrate Prime with your company’s LDAP directory for user authentication, with your CMS, with your marketing automation tool, with your e-commerce platform, and more. These integrations are possible with any such third-party platform, but if you decide to use the solutions of the Adobe Experience Cloud (AEC), you will be able to leverage the built-in connectors available out of the box in Captivate Prime. Thanks to these integrations, you can deliver customized learning experiences to your employees, partners, and customers alike.

In this chapter, you also took your first steps into discovering the Prime platform hands on. You activated your free 30-day trial and explored the basic settings of your personal user account, as well as the basic information of your Prime account as a whole. Finally, you had your first glimpse at the four roles available in the system: Administrator, Author, Learner, and Integration Admin.

In the next chapter, you will continue with the initial setup of your Prime account by reviewing the options available to customize the look and feel of both the user and admin experience.

## 2 Customize the Look and Feel of Captivate Prime

Now that you have a working Captivate Prime account, you will concentrate on the initial setup of the system. One of the first elements that you probably want to customize is the look and feel of your Prime account. After all, if Captivate Prime intends to be the enterprise LMS of choice, it should give you the ability to match the LMS experience with the brand image and the digital persona of your organization.

Therefore, in this chapter, you will explore the main branding and customization features offered by Adobe Captivate Prime. You will discover how to modify the name and the URL of your account, and you will also upload your logo and modify the color scheme of the LMS in order to match your corporate branding.

After that, you will explore the options for customizing the learner homepage. Remember that the learner experience is one of the most important aspects of your Prime account as this is where your learners (which represents the bulk of the users using your account) will spend most of their time on the platform.

Most of these customizations can be done directly on the platform using the admin role, but some more advanced customizations require an intervention of the Captivate Prime support team. At the end of the chapter, you will also discover that Captivate Prime can be used as a headless LMS, a feature Captivate Prime LMS has pioneered.

At the end of this chapter, the experience provided by your Captivate Prime account will match the experience provided by the other digital tools of your organization. To achieve that goal, this chapter covers the following topics:

- Modifying the name and the subdomain of your Prime account
- Branding your account by uploading your logo and modifying the color scheme
- Customizing the learner's homepage
- Using Captivate Prime as a headless LMS

This makes for an exciting new chapter in which you will enter deeper into the main options of your Captivate Prime account.

### Technical requirements

If you want to perform the tasks described in this chapter hands-on, you need to meet the following technical requirements:

- You must have administrator access to a working Captivate Prime account (trial accounts are OK).
- You need to have access to your company's logo and color guide. If you do not have access to these assets, a sample logo and a sample branding guide are available in the branding folder of the download associated with this book.

### Modifying the name and the subdomain of your Prime account

When you activated your Prime account in the previous chapter, a URL was generated automatically based on the organization name you supplied in the account activation form. You will now discover how you can modify the name and/or the URL of your Prime account:

1. Log in to your Prime account as an administrator.
2. Once logged in, scroll down the administrator homepage and click the Branding link located in the Configure section at the bottom of the left column.

This opens the **Branding** section of the administrator application. It should be similar to the following screenshot:

![](img/5ceaefa2a9a737206c59f0b54e1a07d0_51_0.png)

Figure 2.1 – The Branding section of the Captivate Prime administration app

At the top of the page, notice the **Organization Name** field. This should match the organization name you filled in when activating your Captivate Prime trial account in the previous chapter. It is also the name that appears in the top left corner of the screen, above the left column.

3. To modify the name of your Prime account, click the **Edit** link associated with the **Organization Name** option.
4. Type the new name of your Prime account and click the **Save** button.

You can also modify the URL of your Prime account using a similar procedure. Note that, using this technique, the URL of your Prime account will always match the following pattern: `https://<yourCompanyName>.captivateprime.com`.

This is the reason why this option is called **Subdomain**. The **domain** is `captivateprime.com`, and that part of the address cannot be changed. The element that comes between `http://` and `captivateprime.com` is the **subdomain**. That is the only part of the URL that can be changed using this option.

### Subdomains on the internet

The concept of **subdomains** is not unique to Captivate Prime. Actually, it is something widely used across the internet. Take, for example, a company whose main website is www.company.com. This company may operate other websites such as mail.company.com, intranet.company.com, shop.company.com, and so on. In this case, company.com is the domain, while mail, intranet, and shop are considered subdomains of the company.com domain.

5. To customize the subdomain of your Prime account, click the **Edit** link associated with the **Subdomain** option.
6. Enter the new subdomain and click the **Save** button to validate it.

Note that only letters (uppercase or lowercase), numbers, and hyphens are allowed for the subdomain. Special characters and spaces are prohibited. This is not a limitation of Captivate Prime itself, but a limitation of the internet as a whole. Captivate Prime being a web application, it has to comply with the general naming rules of the internet!

Also note that the subdomain must be unique across the whole Captivate Prime ecosystem. Subdomains are allocated on a first come, first served basis. If your subdomain of choice is already taken by another customer, you will see the following error message appear onscreen.

![](img/5ceaefa2a9a737206c59f0b54e1a07d0_52_0.png)

If your subdomain is accepted by the system, you can start sharing it with the other users of your Prime account (administrators, authors, learners, and so on). From now on, this is the URL every user shall use to access your account.

### Using a custom domain

For a growing number of organizations, the obligation to see .captivateprime.com in the URL of their LMS is seen as an unacceptable limitation that impairs their ability to truly own and manage their brand name. After all, the domain name of a company is an important part of its branding and one of the cornerstones of its communications with employees, partners, stakeholders, customers, and the general public.

That's why, since the December 2020 update, Captivate Prime supports using a custom domain to access your account. This allows you to replace the mandatory .captivateprime.com part of the address with your own custom domain. Thanks to this new feature, you could set up your account under any of the following URL patterns:

- mycompany.com
- lms.mycompany.com
- prime.mycompany.com
- mycompany.com/learning
- And so on...

As a prerequisite, you must own or register the domain that you want to use. You must also have access to the SSL certificates and the associated private keys of the custom domain in order to enable secure communication using the HTTPS protocol.

At the time of this writing, the process of setting up a custom domain for your Prime account cannot be done in the administrative area. It involves some manual actions both by the Prime support team and your IT department.

For more information on setting up a custom domain for your Prime account, refer to the following article on the official Prime support site: https://helpx.adobe.com/captivate-prime/custom-domain.html.

In this section, you learned how to customize the URL of your Prime account. This is an important first step toward taking ownership of your new LMS, but it is not the only customization possible. In the next section, you will learn how to brand your account by uploading your company logo to the system and by modifying the color scheme used throughout your account.
