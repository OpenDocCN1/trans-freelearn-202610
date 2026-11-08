In order to hash passwords using Scrypt, you need to create a unique salt for each hash. You can check out https://auth0.com/blog/adding-salt-to-hashing-a-better-way-to-store-passwords/ and https://qvault.io/cryptography/very-basic-intro-to-the-scrypt-hash/ for more details.

This function concatenates and returns both the salt and password's hash, which will both be saved to the database because the salt is required to verify the password.

9. Add the following method, which will be used to compare the plain password to the hash:

```
```ts
const compareToHash = async (plainPassword: string, hash: string): Promise<boolean> => {
  return new Promise((resolve, reject) => {
    const result = hash.split(":");
    const salt = result[0];
    const hPass = result[1];
    crypto.scrypt(plainPassword, salt, 64,
      (error, buffer) => {
        if (error) reject(error);
        resolve(hPass == buffer.toString('hex'));
      });
  })
};
```
```

Check out the docs https://nodejs.org/api/crypto.html#crypto_crypto_scrypt_password_salt_keylen_options_callback.

10. Next, add the register mutation for registering users:

```
```ts
register: async (_, args, { orm }) => {}
```
```

Inside the body of the method, we first destructure the args object to retrieve the user's full name, username, email, and password:

```
```ts
const { fullName, username, email, password } = args;
```
```

11. Next, we call the create() method of the user's repository to create a new user with the retrieved information. For the password, we call the hashPassword() method to hash it before assigning it:

```
```ts
let user = orm.userRepository.create({
  fullName: fullName,
```
```