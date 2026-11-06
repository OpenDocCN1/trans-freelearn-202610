```
next();
} catch (error) {
  next(error);
}
});

userSchema.post('save', function(doc) {
  console.log(`User ${doc.email} has been saved`);
});
```

ODMs typically provide a variety of hooks within a document’s life cycle, allowing developers to execute custom logic at strategic points, such as the following common examples:

+   - Pre/post validation
+   - Pre/post save
+   - Pre/post update
+   - Pre/post delete

These hooks enable complex behaviors such as automatic timestamps, logging, encryption, and change tracking.

# Type safety and IDE integration

Modern ODMs leverage language features to provide compile-time safety and improved developer experience. The following TypeScript example using Typegoose (a library for Mongoose) demonstrates how class decorations can define schemas and their properties, enabling autocompletion and type checking during development, thus reducing errors and improving developer productivity:

```
class User {
  @prop({ required: true })
  public firstName!: string;

  @prop({ required: true })
  public lastName!: string;

  @prop({ required: true, unique: true })
  public email!: string;
```