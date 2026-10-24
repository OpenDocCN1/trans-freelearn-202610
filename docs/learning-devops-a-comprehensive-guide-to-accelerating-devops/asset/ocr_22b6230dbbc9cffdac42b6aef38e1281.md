```
"subscription_id": "xxxxxxxxxx",
"tenant_id": "xxxxxx",
"os_type": "Linux",
"image_publisher": "Canonical",
"image_offer": "UbuntuServer",
"location": "westus"
......
}]
}
```

In this sample, the builders section defines an image that will be stored in the Azure cloud and is based on the Linux Ubuntu OS. We also configure the authentication keys for the cloud.

> Note
> The documentation on the builders section is here: https://www.
> packer.io/docs/builders.

If we want to create the same image but on several providers, we can indicate in the same template file multiple block builders that will contain the provider properties. For example, see the following code sample in JSON format:

```
{
"builders": [
{
"type": "azure-rm",
"location": "westus",
......
},
{
"type": "docker",
"image": "alpine:latest",
...
}
]
}
```