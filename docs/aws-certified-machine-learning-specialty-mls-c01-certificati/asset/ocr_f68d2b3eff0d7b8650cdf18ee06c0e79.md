```
}
]
}
```

3. There may be a situation where you have to roll back to the earlier version of the current object. In the preceding example, the latest one is *Version-2*. You can make any desired version the latest or current version by parsing the *VersionId* sub resource to the `get-object` API call and uploading that object again. The other way is to delete the current or latest version by passing `versionId` to the `-version-id` parameter in the `delete-object` API request. More details about the API are available here: https://docs.aws.amazon.com/cli/latest/reference/s3api/delete-object.html.

4. When you delete an object in a versioning-enabled bucket, it does not delete the object from the bucket. It just creates a marker called **DeleteMarker**. It looks like this:

```
$ aws s3api delete-object --bucket version-demo-mlpractice --key version-doc.txt
{
    "DeleteMarker": true,
    "VersionId": "BKv_Cxix tm7V48MWqBO_KUkKbcOaH5JP"
}
```

5. This means that the object is not deleted. You can list it by using this command:

```
aws s3api list-object-versions --bucket version-demo-mlpractice
```