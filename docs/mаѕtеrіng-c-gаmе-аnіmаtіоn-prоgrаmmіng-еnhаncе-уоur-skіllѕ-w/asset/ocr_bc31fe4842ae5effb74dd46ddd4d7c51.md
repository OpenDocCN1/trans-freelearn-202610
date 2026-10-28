If an existing link was disconnected and then dropped, IsLinkDestroyed() returns true, giving back the ID of the dropped link as an output parameter:

```
int linkId;
if (ImNodes::IsLinkDestroyed(&linkId)) {
    links.erase(linkId);
}
```

imnodes has some other functions enabling custom link management, but for our node editor, IsLinkCreated() and IsLinkDestroyed() will be sufficient.

Duplicating the code for our example node (remember to use unique IDs for every attribute) and adding a link between the output pin ID of the first node and the input ID of the second node will result in something similar to Figure 9.6:

![](bbox=[0.205, 0.375, 0.79, 0.645])

Figure 9.6: Two example nodes connected by a link

You will have to move the nodes apart since all new nodes are created in the same spot by default. Next to setting the initial position when creating a node, imnodes allows us to store the positions of all nodes in the current editor session by using SaveCurrentEditorStateToIniString(). Restoring the positions can be achieved later by calling LoadCurrentEditorStateFromIniString().

Having the nodes in the same place when reopening the editor helps to create a great user experience.