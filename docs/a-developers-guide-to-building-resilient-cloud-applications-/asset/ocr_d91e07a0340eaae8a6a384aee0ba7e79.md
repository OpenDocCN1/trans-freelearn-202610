![](bbox=[0.14, 0.09, 0.87, 0.41])

# Figure 8.23 – Azure Blob Storage .NET library

3. In Program.cs, we will use the BlobServiceClient constructor using the account's connection string. To create a unique name for the container, we will use the CreateBlobContainerAsync method:

```
var blobServiceClient = new BlobServiceClient("<your-storage-account-connection-string>");
BlobContainerClient containerClient = await blobServiceClient.CreateBlobContainerAsync(containerName);
```

4. We will create a local directory for uploading and downloading files. We will write text in a file and get a reference to a blob using the GetBlobClient method:

```
string localPath = "datarepo";
Directory.CreateDirectory(localPath);
string fileName = "file-" + Guid.NewGuid().ToString() + ".txt";
string localFilePath = Path.Combine(localPath, fileName);
BlobClient blobClient = containerClient.GetBlobClient(fileName);
```

5. We will upload data from the local file using the UploadAsync method:

```
await blobClient.UploadAsync(localFilePath, true);
```