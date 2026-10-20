```
user.email)

if user_exist:
    raise HTTPException(
        status_code=status.HTTP_409_CONFLICT,
        detail="User with email provided exists already."
    )
hashed_password = hash_password.create_hash(
user.password)
user.password = hashed_password
await user_database.save(user)
return {
    "message": "User created successfully"
}
```

Now that we have updated the user sign-up route to hash the password before saving, let’s create a new user to confirm. In a terminal window, start the application:

```
(venv)$ python main.py
INFO:     Uvicorn running on http://0.0.0.0:8080 (Press CTRL+C to quit)
INFO:     Started reloader process [8144] using statreload
INFO:     Started server process [8147]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
```

In another terminal window, start the MongoDB instance:

```
$ mongod --dbpath database --port 27017
```

Next, let’s create a new user:

```
$ curl -X 'POST' \
'http://0.0.0.0:8080/user/signup' \
-H 'accept: application/json' \
-H 'Content-Type: application/json' \
-d '{
"email": "reader@packt.com",
```