
To add a single-site clustered indexer to a search head, issue the following command in the $SPLUNK_HOME/bin directory on the search head. Behind-the-scenes configuration settings will be added to the server.conf file in the $SPLUNK_HOME/etc/system/local directory:

```
./splunk add cluster-manager <scheme>://<host>:<port> -secret <indexers-pass4symmkey> -multisite false
```

This command must be repeated for every cluster manager.

Let’s look at the elements of this command:

- `<scheme>://<cluster-manager-servername>:<management-port>` is the cluster manager details
- `<indexer-pass4symmkey>` is the pass4SymmKey/secret/security key of the indexer cluster

As an illustration, assuming you’ve previously executed a command to add the cluster manager address SplunkManager01.example.com, the server.conf file now displays the following content:

```
[clustering]
mode = searchhead
manager_uri = https://SplunkManager01.example.com:8089
pass4SymmKey= encrypted-secreteast
```

For further options, such as listing, editing, or removing a cluster manager, you can use the help command. For instance, execute ./splunk help cluster-manager in the /bin directory of $SPLUNK_HOME.

Similarly, if you wish to learn how to edit and/or remove search-server, you can employ the help command as follows: ./splunk help search-server. This should be done within the /bin directory of $SPLUNK_HOME.

We have now learned how to include both clustered and non-clustered indexers in a search head for the purpose of configuring distributed search. Moving forward, the subsequent section will guide us through the process of configuring distributed search using the Splunk Web interface.

## Splunk Web

Using Splunk Web, we can create the same configuration that we did in the previous section with the CLI. Let’s look at the menu options in Figure 7.3.

Log in to Splunk Web as an admin user, navigate to Settings in the top-right corner, and click on it. Then, you will find the following menu items under DISTRIBUTED ENVIRONMENT:

Figure 7.4 has the following text fields: Peer URI (peer-URI:mgmt_port format), Remote username, and Remote password, in which you can manually enter the details of the indexer that needs to be added to the search head. Populate these fields and click Save. Repeat the same steps as shown in Figure 7.3 and Figure 7.4 for every search peer to add them to the search head.

In the next section, we are going to add clustered indexers to the search head.

## Adding clustered indexers

In Splunk Web, click on the Settings menu and click on Indexer clustering, as shown in Figure 7.3, which will open a new page that has an Enable Indexer Clustering button. Click on the button and select the Search head node radio button, as shown in Figure 7.5.

![](img/5319cbd6f078c6cda7fac6347f77a07d_133_0.png)

Figure 7.5: Distributed search – Enable Clustering on the search head

Figure 7.5 shows the options to enable indexer clustering on the search head. Finally, click on the Next button. A form will open, as shown in Figure 7.6.

- To look at the complete list of allowed configurations that are included in the knowledge bundle, refer to [replicationAllowlist] and [replicationSettings:refineConf] inside the distsearch.conf file of the search head. To check the bundle replication status, you can issue the splunk show bundle-replication-status command on the search head, or you can use the monitoring console, which has a dashboard under its search menu.

In cases where the common knowledge bundle version cannot be replicated across all search peers, it might lead to a temporary search interruption. The search process may pause for a few seconds as it attempts to synchronize the knowledge bundle across peers. If establishing a common bundle proves unsuccessful, the search process will eventually give up and resume without achieving synchronization. In that case, the Splunk administrator will take the necessary action by identifying the slow search peer and quarantining it on the search head to prevent the peer from participating in future searches. After quarantine, the search peer ensures that already running searches are completed. The quarantine action suspends knowledge bundle replication to only that peer.

Quarantine can be achieved through Splunk Web by clicking on Settings | Distributed Search | Search peers. You can also do it through the CLI. After successful restoration, unquarantine the peer so it can participate in future searches.

Here are the CLI quarantine and unquarantine commands:

```
./splunk edit search-server -auth <user>:<password> <host>:<port> -action [quarantine|unquarantine]
```

Let’s examine this in more detail:

- <user>:<password> is the search head’s admin-level credentials
- <host>:<port> is the search peer’s host and management port
- The -action values are quarantine or unquarantine

Let’s conclude; a knowledge bundle is an archive file that is shared with search peers by the search head through a background replication process. A knowledge bundle contains knowledge objects that are applied by search peers to search results before being returned to the search head. The bundle size limit is applicable by default, and to reduce its size, certain knowledge objects can be added to the replicationDenyList setting on the search head. To suspend bundle replication to a particular peer, you can quarantine that search peer.

Let’s summarize what we have learned in the next section.

3. Is the search head captain component mandatory in a search head cluster?
A. Yes
B. No

4. You are assigned to a critical project where a search head cluster is configured to search multiple non-clustered indexers. Is it making use of the distributed search concept?
A. Yes
B. No

5. Identify the configuration files containing indexer details on a search head.
A. serverclass.conf
B. server.conf
C. distsearch.conf
D. distributedpeers.conf

6. What does a knowledge bundle contain? (Select all that apply.)
A. Users, knowledge objects, and apps
B. Knowledge objects such as event types, tags, and saved searches
C. Splunk binary files
D. Splunk internal logs about the search process

7. What is the default replication policy of knowledge bundles?
A. Cascading
B. RFS
C. Classic
D. Mounted

8. A knowledge bundle has grown so big that it exceeds maxBundleSize. You are a system administrator; what will you do?
A. Increase maxBundleSize
B. Delete the apps from the search head that are large
C. Add files/apps that are not required to replicationDenyList
D. Do nothing; the search head shares only the .delta file

## Part 2:
Splunk Data Administration

Part 2 of this Splunk 9.x Enterprise Certified Admin Guide is a deep dive into the world of Splunk data administration. It will equip you with the skills needed to efficiently bring data into Splunk, configure data inputs, perform parsing and transformation, extract meaningful fields, and enhance data with lookups. The self-assessment mock exam ensures that you can apply your knowledge effectively.

This part contains the following chapters:

- Chapter 8, Getting Data In
- Chapter 9, Configuring Splunk Data Inputs
- Chapter 10, Data Parsing and Transformation
- Chapter 11, Field Extractions and Lookups
- Chapter 12, Self-Assessment Mock Exam

If you would like to dive into a detailed overview of these phases, along with queues and pipelines, then check out https://tinyurl.com/bdzb cetp. Note that this is completely optional for the certification exam.

We have now examined the main data ingestion phases and metadata fields. Next, we will explore the Add Data feature of Splunk Enterprise available in Splunk Web.

## Splunk Web – Add Data feature

The Add Data feature is available in Splunk Web under the Settings menu. Simply open this menu and click on Add Data, as shown in Figure 8.3.

![](img/5319cbd6f078c6cda7fac6347f77a07d_149_0.png)

Figure 8.3: Splunk Web – Add Data

Clicking Add Data opens a new web page offering three ways to get the data into Splunk, as shown in Figure 8.4.

![](img/5319cbd6f078c6cda7fac6347f77a07d_149_1.png)

Figure 8.4: Methods for getting data into Splunk
