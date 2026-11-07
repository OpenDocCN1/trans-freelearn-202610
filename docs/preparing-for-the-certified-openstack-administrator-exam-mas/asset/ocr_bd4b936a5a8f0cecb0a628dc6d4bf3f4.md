6. Select **Create Volume Snapshot**.

![](bbox=[0.12, 0.12, 0.83, 0.47])

Figure 7.17: Providing the name of the source volume for the snapshot on the Horizon dashboard

You should now see your newly created volume snapshot under the **Snapshots** tab. This snapshot is owned by the `quality-control` project. You can easily create a volume from it by selecting the **Create Volume** button.

TIP
When creating a new volume from an existing snapshot, the size of the volume must be equal to or greater than the size of the snapshot. In order to delete a volume with an associated snapshot, you must first delete the snapshot.

# CLI

Let's create a new Cinder snapshot via `python-openstackclient`.

Source the newly created `openrc-quality-control` file:

```
$ source openrc-quality-control
```