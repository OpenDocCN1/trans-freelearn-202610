# # The Chef 0.11.x resource

In the newer versions of Chef, the `registry_key` resource is used for setting values
in the Windows Registry, as described in the following table:

| registry_key |
| --- |
| **Action** | **Description** |
| `create` | This creates a new registry key with the provided data. |
| `create_if_missing` | This creates a key or value if it does not exist. |
| `delete` | This deletes the specified values for a specified registry key. |
| `delete_key` | This deletes a specific key and all subkeys. |
| **Parameter** | **Description** |
| `key` | This is the resource name attribute specifying which registry key
to create, modify, or remove. |
| `architecture` | This specifies the architecture for which the keys will be created.
The possible values are as follows:

• :x86: Force 32-bit registry
• :x86_64: Force 64-bit registry
• :machine: Allow the client to determine

The default value is :machine. |
| `provider` | This determines which provider to use for manipulating the
registry. Currently, there is only one (Chef::Provider::Wind
ows::Registry) whose short name is registry_key, but it is
possible that this could change in the future.

The default value is registry_key. |
| `recursive` | This determines whether this operation should be performed
recursively. When creating a new registry key, create any
required path components; when deleting, delete all subkeys. |
| `values` | This is an array of hashes that contain the values to be set under
the specified registry key. Each hash is composed of three keys
which are as follows:

• :name: The value's name
• :type: The type of value stored
• :data: The data to store

The :type key has the same options as the type for the
windows_registry entries and defaults to :string. |