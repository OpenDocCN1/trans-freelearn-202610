
For dependencies that are not shown, it is a good idea to make sure that the features enabled for your testing orgs are clear, thus any other dependencies will show during installation or further testing.

```
Show Dependencies
No Dependency Information for Package FormulaForce
<< Back to Package: FormulaForce
There are no dependencies.
```

Later in this book, we will be discussing Lightning Components. If you're packaging these, you will be implicitly imposing the need for your customers to utilize the Salesforce My Domain feature. This is not enforced at installation time, so it is an optional dependency. However, users will not be able to use your packaged Lightning Components without first enabling and configuring My Domain.

## Uploading the release and beta packages

Once you have checked your dependencies, click on the Upload button. You will be prompted to give a name and version to your package. The version will be managed for you in subsequent releases. Packages are uploaded in one of two states, beta or release, as described shortly.

We will perform a release upload by selecting the Managed - Release option from the Release Type field, so make sure you are happy with the objects created in the earlier section of this chapter, as they cannot easily be changed after this point.

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_45_0.png)

Once you are happy with the information on the screen, click on the **Upload** button once again to begin the packaging process. Once the upload process completes, you will see a confirmation page as follows:

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_46_0.png)

Packages can be uploaded in one of two states, as described here:

- **Release:** Release packages can be installed into subscriber production orgs and can also provide an upgrade path from previous releases. The downside is that you cannot delete the previously released components, or change certain things, such as a field's type. Changes to the components that are marked **global**, such as Apex code and Visualforce components, are also restricted. While Salesforce is gradually enhancing the platform to provide the ability to modify certain released aspects, you need to be certain that your application release is stable before selecting this option.

- **Beta:** Beta packages cannot be installed into subscriber production orgs; you can install only into Developer Edition (such as your testing org), **sandbox**, or Partner Portal created orgs. Also, Beta packages cannot be upgraded once installed; this is the reason why Salesforce does not permit their installation into production orgs. The key benefit is in the ability to continue to change new components of the release to address bugs and features relating to user feedback.

> The ability to delete previously-published components (uploaded within a release package) is, at the time of writing this book, in pilot. It can be enabled by raising a support case with Salesforce Support. Once you have understood the full implications, they will enable it.

At this stage in the book, we have simply added some Custom Objects, so the upload should complete reasonably quickly. Note that what you’re actually uploading to is AppExchange, which will be covered in the following sections.

> If you want to protect your package, you can provide a password (this can be changed afterwards). The user performing the installation will be prompted for it during the installation process.

## Optional package dependencies

It is possible to make some Salesforce features and/or base package component references (Custom Objects and fields) an optional aspect of your application. There are two approaches to this, depending on the type of the feature.

### Dynamic bindings

For example, the Multi-Currency feature adds a CurrencyIsoCode field to the Standard and Custom Objects. If you explicitly reference this field, for example, in your Apex, Visualforce pages or Lightning pages or components, you will incur a hard dependency on your package. If you want to avoid this and make it a configuration option (for example) in your application, you can utilize dynamic Apex and Visualforce. Lightning value bindings are dynamic in nature, though `aura:attribute` element type references will form a compile time reference to the specified objects.

### Extension packages

If you wish to package component types that are only available in subscriber orgs of certain editions, you can choose to include these in extension packages. For example, you may wish to support the Professional Edition, which does not support record types. In this case, create an Enterprise Edition extension package for your application’s functionality, which leverages the functionality from this edition.

> Note that you will need multiple testing organizations for each combination of features that you utilize in this way to effectively test the configuration options or installation options that your application requires.

## Becoming a Salesforce partner and benefits

The Salesforce Partner Program has many advantages. The first place to visit is https://partners.salesforce.com/. You will want to focus on the areas of the site relating to being an Independent Software Vendor (ISV) partner. From here, you can click on Join Now. It is free to join, though you will want to read through the various agreements carefully, of course.

Once you wish to start listing a package and charging users for it, you will need to arrange billing details for Salesforce to take the various fees involved. While this book is not equipped to go into the details, do pay careful attention to the Standard Objects used in your package, as this will determine the license type required by your users and the overall cost to them, in addition to your charges.

Obviously, Salesforce would prefer your application to use as many features of the CRM application as possible, which may also be beneficial to you as a feature of your application, since it's an appealing immediate integration not found on other platforms, such as the ability to instantly integrate with accounts and contacts.

If you're planning on using Standard Objects, and are in doubt about the costs (as they do vary depending on the type), you can request a conversation with Salesforce to discuss this; this is something to keep in mind in the early stages.

Make sure, when you associate a Salesforce user with the Partner Community, you utilize a user that you use daily (known as your Partner Business Org user) and not one from a development or test org. Once you have completed the signup process, you will gain access to the Partner Community. The following screenshot shows what the current Partner Community home page looks like. From here, you can access many useful services:

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_48_0.png)

This is your primary place to communicate with Salesforce and access additional materials and announcements relevant to ISVs, so do keep checking often. You can raise cases and provide additional logins to other users in your organization, such as other developers who may wish to report issues or ask questions.

## Security review and benefits

The following features require that a completed package release goes through a Salesforce-driven process known as the **security review**, which is initiated via your listing when logged into AppExchange. Unless you plan to give your package away for free, there is a charge involved in putting your package through this process.

However, the review is optional. There is nothing stopping you from distributing your package installation URL directly. However, you will not be able to benefit from the ability to list your new application on AppExchange for others to see and review. More importantly, you will also not have access to the following features to help you deploy, license, and support your application. The following is a list of the benefits you get once your package has passed the security review:

- **Bypass subscriber org setup limits**: Limits such as the number of tabs and Custom Objects are bypassed. This means that if the subscriber org has reached its maximum number of Custom Objects, your package will still install. This feature is sometimes referred to as **Aloha**. Without this, your package installation may fail. You can determine whether Aloha has been enabled via the **Subscriber Overview** page that comes with the LMA application, which is discussed in the next section.

- **Licensing**: You are able to utilize the Salesforce-provided License Management Application (LMA) in your LMO.

- **Subscriber support**: With this feature, the users in the subscriber org can enable, for a specific period, a means for you to log in to their org (without exchanging passwords), reproduce issues, and enable much more detailed debug information, such as Apex stack traces. In this mode, you can also see custom settings that you have declared as protected in your package, which is useful for enabling additional debug or advanced features.

- **Push upgrade**: Using this feature, you can automatically apply upgrades to your subscribers without their manual intervention, either directly by you or on a scheduled basis. You may use this for applying either smaller bug fixes that don't affect the Custom Objects or APIs, or for deploying full upgrades. The latter requires careful coordination and planning with your subscribers to ensure that changes and new features are adopted properly.
