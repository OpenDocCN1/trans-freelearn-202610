```
```json
"LockIndex": 0,
"Key": "son/seventh/gift",
"Flags": 0,
"Value": "Y2hxaXJ2b3lhbmN1",
"CreateIndex": 194,
"ModifyIndex": 194
}
]
```
```

But wait, that value isn't what we entered! You are correct, this is the base64 encoded value that we've seen in other areas of Consul. This allows more complex data within the values of our KV store to be accessed programmatically. If we want multiple values to be returned within a path, we can set a recurse flag on the request:

```
```bash
$ curl http://18.222.124.50:8500/v1/kv/son/\?recurse=TRUE | jq
```
```

Let's take a look at some of the other values that are provided as part of this output:

+   - **LockIndex** – Using Consul Sessions, we are able to lock access to particular parameters within the KV store to prevent inadvertent overwriting or conflicts.
+   - **Key** – This is the full path to the key from the root of the KV store.
+   - **Flags** – Defaulting to 0, this parameter provides the ability to assign an unsigned integer to the value entry. This value is opaque to Consul but may help provide additional information (such as version) to an extensive KV implementation.
+   - **Value** – This is the value of the key encoded to base64.
+   - **CreateIndex** – Here we have a numerical value that provides some insight into when the value was created in relation to other keys or updates.
+   - **ModifyIndex** – This is a good value to watch as part of the output, as an intelligent state machine can determine whether the value has changed since the last time it was queried.

This information is always returned via the API, however, if you would like to see this detail within the command-line output, we can use the `detailed` flag:

|  |  |
|---|---|
| `$ consul kv get -detailed son/seventh/gift` | |
| CreateIndex | 194 |
| Flags | 0 |
| Key | son/seventh/gift |
| LockIndex | 0 |