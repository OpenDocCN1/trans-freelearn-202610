Next, add the following code at the top:

```
import AWS from 'aws-sdk';
const spacesEndpoint = new AWS.Endpoint(process.env.S3_ENDPOINT as string);
const s3 = new AWS.S3({
  endpoint: spacesEndpoint,
  accessKeyId: process.env.S3_ACCESS_KEY_ID,
  secretAccessKey: process.env.S3_SECRET_ACCESS_KEY
});
```

Finally, add the uploadFile mutation, as follows:

```
uploadFile: async (_, { file }) => {
  const { createReadStream, filename, mimetype, encoding } = await file;
  const fileStream = createReadStream();
  const uploadParams = { Bucket: process.env.S3_BUCKET as string, Key: filename, Body: fileStream, ACL: "public-read" };
  const result = await s3.upload(uploadParams).promise();
  console.log(result);
  const url = result.Location;
  return { url, filename, mimetype, encoding };
}
```

We destructure the file promise passed as an argument to the resolver. This way, we can get the read stream and other information, such as the filename, MIME type, and encoding needed to save the file to the S3 service using the upload() method of the AWS SDK. Next, we get the URL of the saved file and we return with the other information.

# Assignments

We'll use the uploadFile mutation to upload images for posts. For uploading the user's photo and cover image, we also need to link the uploaded images to the appropriate user. To achieve this, we need to add two setUserPhoto and setUserCover mutations.