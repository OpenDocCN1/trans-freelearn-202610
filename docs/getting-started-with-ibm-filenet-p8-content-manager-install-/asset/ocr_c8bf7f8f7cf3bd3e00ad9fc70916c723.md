+   3. Highlight the cluster name just added to the list and click **Edit**.
+   4. Use the **Add** button to add both of our server hosts, **wjc-rhel.example.net** and **wjc-w2k3.example.net**.
+   5. Click **OK** to close the edit panel.
+   6. Use menu **File | Save Clusters As** to save your cluster definition to a file in a convenient location. In future runs of the Dashboard, you'll be able to load cluster file definitions instead of recreating them each time.

> It is normal to get an error for any host in the cluster that does not happen to be running any P8 software components. If P8 components are running on a given host, they will normally be discovered automatically even if the Dashboard is running remotely.

# Rendition Engine (RE)

RE, also known as the Publishing engine or service, can transform commonly-used business document formats into HTML or PDF. The content of the documents is not changed, and the format is rendered as faithful to the originals as possible. The CE maintains links between the original version (called the **source** document) and the rendered version (called the **target** document).

> RE is separately licensed and is not included in a basic CM license.

There are two typical use cases for RE:

+   - A site might want to make content available on a web site in a format that is either vendor-neutral or usable by the widest possible audience (or both). Both HTML and PDF fit this scenario.
+   - A site might want to permanently capture, for compliance or other reasons, the exact state of a document at some instant in time. Although CM reliably keeps track of all content changes and prevents any content change once a document version is checked in, there are sometimes external reasons for wanting an official snapshot of the content. For many scenarios, PDF is the format of choice for this sort of snapshot.