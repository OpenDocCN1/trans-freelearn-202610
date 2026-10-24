# # Examples of how to manage the pagefile

Set the default pagefile to `D:\pagefile.sys` with an initial size of 8 GB and a maximum size of 16 GB (creating it, if need be) with the following code:

```
one_gb = 1024 * 1024 * 1024
windows_pagefile 'D:\\pagefile.sys' do
  initial_size (8 * one_gb)
  maximum_size (16 * one_gb)
  system_managed false
  automatic_managed false
  action :set
end
```

Delete the pagefile at `X:\pagefile.sys` as shown in the following code:

```
windows_pagefile 'X:\\pagefile.sys' do
  action :delete
end
```

# # ZIP files

There is not a consistent way to natively manage packing or unpacking of ZIP files on all Windows platforms. To work around this, the `windows_zipfile` resource provides a pure Ruby implementation for manipulating ZIP files on the Windows platform.

The following table lists the actions and parameters available for the `windows_zipfile` resource:

| Action | Description |
| --- | --- |
| `unzip` | This decompresses a specified ZIP file at the source into the destination. |
| `zip` | This creates a ZIP file at the destination from the source. |
| **Parameter** | **Description** |
| `path` | This is the resource name attribute specifying the path – unzipping this designates where the unzipped files go, while zipping the path specifies where the ZIP file is to be created. |
| `source` | This indicates the ZIP file location to uncompress when unzipping, or the directory containing files to be compressed when zipping. |
| `checksum` | This is the optional checksum that verifies whether the ZIP file is the expected one before decompressing it. |
| `overwrite` | This determines whether this operation should overwrite files. |
| | The default value is false. |