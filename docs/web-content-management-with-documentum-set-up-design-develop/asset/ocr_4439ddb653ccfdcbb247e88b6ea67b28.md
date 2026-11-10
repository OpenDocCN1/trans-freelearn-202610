![](bbox=[0.17, 0.14, 0.83, 0.2])

As shown in figure 12.25, if you check the Delay Publish Active checkbox, power promotion to the Active state is taken care by the Monitor_Lifecycles Job.

The following API commands (executed via IAPI utility or API Tester in Documentum Administrator) can be used for promoting and demoting content as well. Refer to Chapter 26 to understand Server API commands better.

+   - **Promoting Content**
+     `API>id,c,cust_newsarticle where object_name = Testing_Promote_Demote.xml`
+     `090015558000bc5b`
+     Note that the API command id returns the object ID of the object matching the specified qualification. In our case, 090015558000bc5b is the object ID (r_object_id attribute) of the News Article content with the name Testing_Promote_Demote.xml.
+     `API>promote,c,090015558000bc5b`
+     `...`
+     `OK`
+   - **Demoting Content**
+     `API>demote,c,090015558000bc5b`
+     `...`
+     `OK`
+   - **Batch Promoting Content**
+     The administrative method BATCH_PROMOTE can be used to promote multiple objects to their next Lifecycle state via a simple API command. The interesting thing about BATCH_PROMOTE is that it can be used on multiple objects existing in different Lifecycle states and attached to different lifecycles. You can promote up to 200 objects in a single execution of BATCH_PROMOTE.
+     Let us now promote two content files, Testing_Promote_Demote.xml and Testing_Promote_Demote002.xml, using BATCH_PROMOTE:
+     `API>id,c,cust_newsarticle where object_name = Testing_Promote_Demote002.xml`
+     `...`
+     `090015558000bc71`
+     `API>id,c,cust_newsarticle where object_name = Testing_Promote_Demote.xml`
+     `...`
+     `090015558000bc5b`
+     Once you have retrieved the r_object_ids of the two content files, issue the following simple API command for batch promotion.
+     `API> apply,c,NULL,BATCH_PROMOTE,ARGUMENTS,S,090015558000bc71,090015558000bc5b`