# Automating Configuration

The same as with network interface discovery, we set the update interval to 120. The default in the form, 30 seconds, is very low and should not be used. Discovery can be resource intensive, and, if possible, should be run hourly or so. Now, switch to the **Filters** tab, and fill in these values:

+   - **Macro:** `{$FSTYPE}`
+   - **Regular expression:** `^ext3$`

> Replace the filesystem type with the one used on your system. Multiple filesystem types can be accepted, like this: `^ext3|ext4$`.

When done, click on the **Add** button at the bottom. We have the discovery now, but no prototypes. Click on **Item prototypes** next to **Filesystem discovery**, and click on **Create item prototype**. Fill in the following:

+   - **Name:** `Free space on {$FSNAME}`
+   - **Key:** `vfs.fs.size[{$FSNAME},free]`

When done, click on the **Add** button at the bottom. We now expect the discovery to get the list of all filesystems, discard most of those except the ones with the type exactly ext3, and then create a free disk space item for each of them. We filter by one LLD macro, `{$FSTYPE}`, but use another — `{$FSNAME}` — in the actual item configuration. After a couple of minutes have passed, navigate to **Configuration | Hosts** and click on **Items** next to **A test host**. For each filesystem of type ext3, there should be a free disk space item:

![](bbox=[0.202, 0.594, 0.797, 0.63])

With more prototypes, we could also monitor total space, inode statistics, and other data. We could have triggers as needed on all of these filesystems.