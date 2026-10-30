
7. When your screen looks similar to the following screenshot, click the **Save** button:

![](img/5ceaefa2a9a737206c59f0b54e1a07d0_525_0.png)

Figure 14.05 – Add the All Internal Users group to Select User Groups

You just granted all your internal users access to the **Internal Catalog**. You will now repeat the same operations to grant all your external users access to the **External Catalog**:

- 8. Click the **Back** link at the top of the left column to return to the **Catalogs** page.
- 9. Click the **External Catalog** in the list of available catalogs.
- 10. Click the **Share Internally** link in the left sidebar (remember that your external users are defined *within your Prime account*, so you must use the *internal* sharing mechanism).
- 11. Click the **Add User Groups** link located in the middle of the page or the **Add** button in the upper-right corner.
- 12. Add the **All External Users** group to the selected user groups and click the **Save** button.
- 13. Finally, click the **Back** link at the top of the left sidebar to return to the main **Catalogs** page of the administrator.

If you try to log into your Prime account as a learner, you will not notice any changes despite all the steps you have taken so far in this chapter. This is because your new custom catalogs are not yet enabled. We will discuss enabling and disabling catalogs in the *Enabling and Disabling Catalogs* section later in this chapter. However, for now, let's briefly discuss external sharing in the next section.

## External Sharing

Sharing a catalog externally enables you to give access to your course content to users defined on other Prime accounts. This can be very useful in many situations:

- You can sell your courses to other organizations that are also using Adobe Captivate Prime as their enterprise LMS.
- If you have partners, suppliers, and customers who also use Captivate Prime as their LMS, you can use external sharing rather than defining their employees as external learners of your own Prime account. By doing this, you will not have to pay for the user seats used by these external users as this cost is covered by the other Prime account.
- Some multinational corporations choose to use separate Prime accounts for each of their regional branches. This allows each branch to remain independent and manage its own training strategy separately from the organization’s headquarters and other branches. In this scenario, the head office can use external catalog sharing to push corporate training content into the Prime accounts of its branches. This allows for a global training strategy to be implemented on top of regional strategies.

These are just a few of the circumstances in which external catalog sharing can be very useful. Let’s explore this feature, hands-on, using the following steps:

- 1. Make sure you are logged into your Prime account as an administrator.
- 2. From the administrator home page, click the **Catalogs** link located in the **Learning** section of the left sidebar.
- 3. Click your custom **Internal Catalog**.
- 4. Finally, click the **Share Externally** link located in the left sidebar.

As shown in the following screenshot, a message informs you that this catalog is already shared internally, so it cannot be shared externally:

![](img/5ceaefa2a9a737206c59f0b54e1a07d0_526_0.png)

Figure 14.06 – You cannot share the same catalog both internally and externally

Thanks to this little experiment, you discover that you cannot share the same catalog both internally and externally. In other words, sharing is exclusive: you can share a catalog *either* internally *or* externally.

So, to proceed with this exercise, you need to create yet another custom catalog:

- 5. Click the **Back** link at the top of the left sidebar to return to the main **Catalogs** page.
- 6. Click the **Create Catalog** link or the **Create** button in the upper-right corner of the page.
- 7. Type any name of your choice into the **Catalog Name** field and click the **Next** button.
- 8. Click the **Share Externally** link located in the left sidebar.
- 9. Click the **Add External Accounts** link or the **Add** button in the upper-right corner of the screen.

Because this new catalog is not yet shared internally, you have the opportunity to **Add External Accounts**.

This opens the **External Account** dialog, as shown in the following screenshot:

![](img/5ceaefa2a9a737206c59f0b54e1a07d0_527_0.png)

Figure 14.07 – The External Account dialog

Most of the fields available in the **External Account** dialog should be self-explanatory. Notice that there are only two required fields in this form:

- In the **Account Subdomain** field, enter the URL of the external Prime account you want to share this catalog with.
- You need to type the email address of the external account administrator into the **Admin Email ID** field.

After completing this form, the administrator of the external account will receive an email containing a unique URL that he/she must click in order to accept or reject the shared catalog.

> **Tip**
> Don't hesitate to contact the Captivate Prime support team for assistance with the proper implementation of these slightly more technical features of Captivate Prime.

By default, accepting the shared catalog has the following consequences:

- All learning objects included in the shared catalog are made available to the receiver account. If the shared courses, learning programs, or certifications have associated job aids, those job aids are automatically part of the share, even if they are not explicitly included in the shared catalog. Additionally, the skills associated with the shared catalog are automatically copied over to the receiver account.
- The modules included in the shared course are not visible in the content library of the receiver account (we discussed modules and the content library in *Chapter 3, Uploading Learning Content and Managing the Content Library*).
- The receiving account is not allowed to modify the shared catalog or any of the learning objects it contains. That said, the receiving account can use the shared courses in its own learning programs, certifications, and catalogs. When shared, courses are included in a catalog created by the receiving account; that catalog cannot then be shared externally to prevent the resharing of the shared items.
- When you modify a catalog that you have shared externally, your modifications are automatically propagated to all the accounts that have access to the shared catalog.

In addition to this default situation, you can also enable full control of your shared catalog. In this scenario, the modules included in the shared learning objects are visible in the content library of the receiving account. This means that they can be used by the receiving account to assemble new courses. Receiving accounts can also edit or even delete objects that are part of the shared catalog. These editions and deletions take place in the receiving account only. The operations performed in the receiving account do not affect the other Prime accounts that have access to this shared catalog.

> **Note**
> More information regarding this feature can be found in the official Captivate Prime user guide at https://helpx.adobe.com/captivate-prime/administrators/feature-summary/shared-catalog-full-control.ug.html.

Don't hesitate to experiment further with this feature if you happen to have access to another Prime account. That said, in the context of this book, we're going to break off the process here and delete the custom catalog you just created:

- 10. Click the **Cancel** button located at the bottom of the **External Account** dialog.
- 11. Then, click the **Back** button located at the top of the left sidebar.
- 12. Use your mouse to hover over the catalog that you created in this section to reveal the trash icon.
- 13. Click the trash icon to delete the catalog.
- 14. Confirm your intention by clicking the **Delete** button that appears in the **Confirmation Required** message.

More information on sharing catalogs externally and how these shared catalogs appear in the receiver Prime accounts can be found in the official Adobe Captivate Prime user guide at https://helpx.adobe.com//captivate-prime/administrators/feature-summary/catalogs.html#ext.

> **Note**
> The default courses available in any new Prime account are, in fact, part of a course catalog created by Adobe Systems that is pushed to your Prime account using the **Share Externally** feature discussed in this section.
