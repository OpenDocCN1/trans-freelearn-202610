So, our first goal is going to be to create a new policy with two simple rules for our servers. We need write access to our nodes and our services. As part of this process, we need to create our policy file, which can be done using a text editor, or simply by using the following command:

```
```sh
cat > first-policy.hcl << EOF
node_prefix "" {
    policy = "write"
}
service_prefix "" {
    policy = "read"
}
EOF
```
```

If you were able to copy and paste correctly (or if you're using a paper book, typing properly), the file should look as follows:

```
```hcl
node_prefix "" {
    policy = "write"
}
service_prefix "" {
    policy = "read"
}
```
```

Based on our expertise, we know that this allows the node to write all node-level operations and that it's able to read all service-level information. Using this file, we're going to create a policy within Consul. This can be done with the following command:

```
```sh
$ consul acl policy create -name "first-token" -description
"Server Token Policy" -rules @first-policy.hcl
```
```

The output of this command should include all the policy information:

```
```
ID:            c034c84d-f4f6-6bb6-4e96-8fc9a4d7dbae
Name:          first-token
Description:   Server Token Policy
Datacenters:
Rules:
node_prefix "" {
    policy = "write"
```
```