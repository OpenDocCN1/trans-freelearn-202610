3. Next, use the service constructor to inject the imported services as follows:

```
```typescript
export class PostService {
  constructor(
    private createPostGQL: CreatePostGQL,
    private removePostGQL: RemovePostGQL,
    private uploadFileGQL: UploadFileGQL) { }
```
```

4. Define and implement the following file uploading method:

```
```typescript
uploadFile(image: File) {
  return this.uploadFileGQL.mutate(
    {
      file: image
    },
    {
      context: {
        useMultipart: true
      }
    }).pipe(map(result => result.data!.uploadFile));
}
```
```

5. Define and implement the following method for creating posts:

```
```typescript
createPost(text: string | null, image: string | null) {
  return this.createPostGQL
    .mutate({
      text: text,
      image: image
    })
    .pipe(map(result => result.data!.post));
}
```
```

6. Define and implement the method for removing posts by ID as follows:

```
```typescript
removePost(id: string) {
  return this.removePostGQL
    .mutate({
      id: id
    })
```
```