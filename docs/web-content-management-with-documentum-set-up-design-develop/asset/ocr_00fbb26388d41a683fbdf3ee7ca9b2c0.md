# Creating Lifecycles, Alias Sets, and Permission Sets

Following are some mechanisms to expire content created in Documentum:

+   - The Monitor_Lifecycles Job for content whose expiration date has been specified
+   - Web Publisher Expire now menu option

When the expiration date for active content matches the current system date, the Monitor_Lifecycles Job expires the content and pulls it off the website after publishing.

Using the Web Publisher Expire now menu option (Document | Lifecycle | Expire now), you can expire content prior to its specified expiration date, as shown in figure 12.27.

![](bbox=[0.175, 0.29, 0.825, 0.42])

Figure 12.27: Manually expiring a content item in Web Publisher

Note that as shown in figure 12.28, the version label of the content changes to Expired (see Status column). The Expired content still remains in the Docbase, though it is removed from the Active website.

![](bbox=[0.175, 0.5, 0.825, 0.56])

Figure 12.28: Expired content as seen in Web Publisher

> In order to make changes in content that is in the Active state, use the Expire now facility to expire the content first and then edit it as required. It is worth noting that content in the following states cannot be demoted: WIP, Approved, Active, and Expired.

That completes our discussion on Lifecycles and their usage in Documentum. You can use a Lifecycle in conjunction with a workflow. While a Lifecycle simply represents the stages in the life of a document such as draft, review, and approved, a workflow actually routes the document through these Lifecycle stages and defines who works on the document, when, and under what conditions. In essence, a Lifecycle and a workflow complement each other. We will discuss workflows in Chapter 19. For now, you are all set to start creating Web Publisher Templates and associating them with custom object types. The content objects created from these can be associated with the custom Lifecycle we created in this chapter.