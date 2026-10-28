As you can see in the preceding code, we are primarily performing some data validation and clean up tasks. If we have a new group we verify that the page name specified is unique to the system. If the page name is unique, then we are able to process the save request. Prior to processing the save, we check to see if a file is being uploaded. If it is, then we make a call to the UploadService.UploadPhotos method and pass in the upload file. From there, we perform standard save operations. The rest of this page is a standard MVP event and data plumbing.

# Members

With a group successfully created, and with the ability to edit the group in place, we can turn our attention to managing membership. In the membership management page, we will be able to approve new membership requests. We can promote and demote existing members to the status of administrator (allowing them to perform various tasks down the road). And we can delete members. This UI will have a multi-select checkbox type interface but not with the standard CheckBoxList control. In addition to that we will support pagination functionality in a custom manner.

![](bbox=[0.179, 0.429, 0.822, 0.72])