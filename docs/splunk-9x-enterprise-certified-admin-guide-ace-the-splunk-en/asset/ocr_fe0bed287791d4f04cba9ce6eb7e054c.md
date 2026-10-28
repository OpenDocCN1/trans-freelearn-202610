After installation, log in as an admin to your Splunk Enterprise instance, as installed in the *Standalone* section of *Chapter 1*. Once logged in, navigate to **Settings | Forwarding and receiving | Configure receiving | New Receiving Port**, then enter 9997 in the listening port field, and click **Save**.

Then, configure the UF to connect to the receiving indexer (in this case, Splunk Enterprise) either by editing the `outputs.conf` file directly or via the command line, as we saw in the *Configure forwarding* section of *Chapter 4, Splunk Forwarder Management*. For more details, you can refer Splunk documentation: https://tinyurl.com/2p8h9nuc.

The following diagram details the UF and indexer (the Splunk Enterprise instance) setup that we are trying to create.

![](bbox=[0.22, 0.26, 0.78, 0.44])

Figure 9.1: The UF sending logs to a receiving indexer

Great! You now have a UF agent ready for you to configure inputs. Let’s begin with file and directory monitoring data input.

# File and directory monitoring

As the name implies, this input type monitors the data files and the directories they’re stored in. This is the most effective way to bring in data and is recommended by Splunk as one of the best approaches to handling files. The monitoring settings are configured in the `inputs.conf` file. To enable this input, a UF agent is required on the source machine, and the same settings also work on **Heavy Forwarders (HFs)** and Splunk Enterprise. Let’s look at the notable features of this input type:

+   - Works for all text-based files including structured formats (XML, CSV, JSON, etc.) and .gzip-compressed files.
+   - Keeps track of the files being monitored via checkpoints maintained in a `fishbucket` directory, under `$SPLUNK_HOME/var/lib/splunk/`.
+   - Resumes the file and directory monitoring from the last location in the event of forwarder restarts.
+   - Recursively discovers all the files in a directory, including any new files created.