
## Creating Lifecycles, Alias Sets, and Permission Sets

![](img/b18244e6621ecc39cef03ff6c0e55e7c_195_0.png)

Figure 12.22: Promoting content

The system will take a short while to promote the content to its next normal state in the Lifecycle. You can click on the Task Status button to view the status of the Lifecycle promote operation. Once the system has promoted the content, the content can be seen in its next normal Lifecycle state (Staging in the case of Custom_Lifecycle) as shown in figure 12.23.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_195_1.png)

Figure 12.23: Demoting content

In order to demote content from its current normal Lifecycle state, choose the checkbox against the content and select the menu option Document | Lifecycle | Demote (refer to figure 12.23).

In order to power-promote the content, choose the checkbox against the content and select the menu option Document | Lifecycle | Power promote. Power promotion pushes the content directly to its Approved Lifecycle state (i.e. the last state in the Lifecycle). At this point it is critical to discuss the Web Publisher Monitor_Lifecycles Job (refer to figure 12.24).

![](img/b18244e6621ecc39cef03ff6c0e55e7c_196_0.png)

Figure 12.24: Monitor_Lifecycles job in Documentum

The Monitor_Lifecycles job in Documentum runs at pre-defined intervals and performs the following functions:

If an Approved object's effective date (a_effective_date attribute) matches the current system date, the Monitor_Lifecycles job turns the object active, by changing its version label to Active.

If an Active object's expiration date (a_expiration_date attribute) matches the current system date, the Monitor_Lifecycles job expires the object, by changing its version label to Expired.

By default, if the effective date for the content is blank, power promotion immediately turns the file active and the content is published. However, by providing a Web Publisher system setting, you can alter this behavior.

Log in to Web Publisher as an administrator and go to Administration | Web Publisher Admin | Settings under the Lifecycles section.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_197_0.png)

As shown in figure 12.25, if you check the Delay Publish Active checkbox, power promotion to the Active state is taken care by the Monitor_Lifecycles Job.

The following API commands (executed via IAPI utility or API Tester in Documentum Administrator) can be used for promoting and demoting content as well. Refer to Chapter 26 to understand Server API commands better.

- **Promoting Content**
  `API>id,c,cust_newsarticle where object_name = Testing_Promote_Demote.xml`
  `090015558000bc5b`
  Note that the API command id returns the object ID of the object matching the specified qualification. In our case, 090015558000bc5b is the object ID (r_object_id attribute) of the News Article content with the name Testing_Promote_Demote.xml.
  `API>promote,c,090015558000bc5b`
  `...`
  `OK`
- **Demoting Content**
  `API>demote,c,090015558000bc5b`
  `...`
  `OK`
- **Batch Promoting Content**
  The administrative method BATCH_PROMOTE can be used to promote multiple objects to their next Lifecycle state via a simple API command. The interesting thing about BATCH_PROMOTE is that it can be used on multiple objects existing in different Lifecycle states and attached to different lifecycles. You can promote up to 200 objects in a single execution of BATCH_PROMOTE.
  Let us now promote two content files, Testing_Promote_Demote.xml and Testing_Promote_Demote002.xml, using BATCH_PROMOTE:
  `API>id,c,cust_newsarticle where object_name = Testing_Promote_Demote002.xml`
  `...`
  `090015558000bc71`
  `API>id,c,cust_newsarticle where object_name = Testing_Promote_Demote.xml`
  `...`
  `090015558000bc5b`
  Once you have retrieved the r_object_ids of the two content files, issue the following simple API command for batch promotion.
  `API> apply,c,NULL,BATCH_PROMOTE,ARGUMENTS,S,090015558000bc71,090015558000bc5b`

The BATCH_PROMOTE method in turn calls the dm_bp_batch Documentum method and returns a collection with one result object after execution.

### 12.5 Expiring Content

At times, content appearing on websites needs to be taken off the website when the content is stale or due to any alterations that may be required. Documentum provides some mechanisms to allow you to expire content and pull it off your websites.

Firstly, you need to understand that Documentum provides a SysObject attribute a_expiration_date that content authors can fill in to specify when the content needs to be expired.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_198_0.png)

Figure 12.26: Expiration date field shown during content creation

Figure 12.26 shows Expiration date field in the New Content screen while creating new content in Web Publisher. In case you forget to specify the expiration date while creating content, you always have an option to specify it via the Publishing tab in the Properties (info) screen.

Following are some mechanisms to expire content created in Documentum:

- The Monitor_Lifecycles Job for content whose expiration date has been specified
- Web Publisher Expire now menu option

When the expiration date for active content matches the current system date, the Monitor_Lifecycles Job expires the content and pulls it off the website after publishing.

Using the Web Publisher Expire now menu option (Document | Lifecycle | Expire now), you can expire content prior to its specified expiration date, as shown in figure 12.27.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_199_0.png)

Figure 12.27: Manually expiring a content item in Web Publisher

Note that as shown in figure 12.28, the version label of the content changes to Expired (see Status column). The Expired content still remains in the Docbase, though it is removed from the Active website.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_199_1.png)

Figure 12.28: Expired content as seen in Web Publisher

> In order to make changes in content that is in the Active state, use the Expire now facility to expire the content first and then edit it as required. It is worth noting that content in the following states cannot be demoted: WIP, Approved, Active, and Expired.

That completes our discussion on Lifecycles and their usage in Documentum. You can use a Lifecycle in conjunction with a workflow. While a Lifecycle simply represents the stages in the life of a document such as draft, review, and approved, a workflow actually routes the document through these Lifecycle stages and defines who works on the document, when, and under what conditions. In essence, a Lifecycle and a workflow complement each other. We will discuss workflows in Chapter 19. For now, you are all set to start creating Web Publisher Templates and associating them with custom object types. The content objects created from these can be associated with the custom Lifecycle we created in this chapter.

### 12.6 Summary

In this chapter we discussed Documentum Alias Sets, Permission Sets (ACL), and Lifecycles in detail. We mentioned the detailed steps for creating an Alias Set for a Lifecycle, Permission Sets for the various states of the Lifecycle, and a custom Lifecycle object.

We also saw how Documentum resolved the alias value for an alias in the Lifecycle scope, Workflow scope, Session scope, User scope, and System scope.

The custom Lifecycle created in this chapter comprised four states: Start, WIP, Staging, and Approved and had dm_document as the primary object type linked to it. We added the News Article object type as an acceptable subtype in the Lifecycle, allowing objects created from News Article object type to be attached to this custom Lifecycle.

Finally we saw the various mechanisms to promote, power-promote, and demote objects in the different states of Lifecycles and some means to expire content from the website.
