![](bbox=[0.11, 0.09, 0.85, 0.19])

Figure 7.3 – Result from the find all users query

The preceding command returns the list of users, and we can now confirm that the user’s password was hashed before it was stored in the database. Now that we have successfully built the components for securely storing user passwords, let’s build the components for creating and verifying JWTs.

# Creating and verifying access tokens

Creating a JWT takes us a step closer to securing our application. The token’s payload will comprise the user ID and an expiry time before encoding in the long string as shown here:

![](bbox=[0.11, 0.4, 0.85, 0.65])

Figure 7.4 – Anatomy of a JWT

Earlier on, we learned why JWTs are signed. JWTs are signed with a secret key known only to the sender and the receiver. Let’s update the Settings class in database/database.py as well as the environment file, .env, to include a SECRET_KEY variable, which will be used to sign the JWTs:

# database/database.py

```
class Settings(BaseSettings):
    SECRET_KEY: Optional[str] = None
```