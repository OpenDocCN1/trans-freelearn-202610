# Step two – collecting and storing the Stack Overflow data

At the time of writing, Stack Exchange provides the data for their entire family of websites – including Stack Overflow – as XML files free for anyone to download. In this section, we will download the Stack Overflow files, and import the data into a database on our MySQL server. Finally, we will create a few smaller versions of these tables for testing purposes.

# Downloading the Stack Overflow data dump

All the data available from Stack Exchange can be downloaded at the Internet Archive. The September 2014 dump is the latest one available at the time of writing. Each Stack Exchange site has one or more files for it, each of which is linked to this details page: https://archive.org/details/stackexchange.

We are only interested in the eight Stack Overflow files that appear alphabetically as shown in the following list:

|  |  |
|---|---|
| stackoverflow.com-Badges.7z | 77.8 MB |
| stackoverflow.com-Comments.7z | 1.8 GB |
| stackoverflow.com-PostHistory.7z | 9.4 GB |
| stackoverflow.com-PostLinks.7z | 26.4 MB |
| stackoverflow.com-Posts.7z | 5.7 GB |
| stackoverflow.com-Tags.7z | 508.1 KB |
| stackoverflow.com-Users.7z | 100.5 MB |
| stackoverflow.com-Votes.7z | 385.0 MB |

Archive.org listing showing the eight Stack Overflow files we are interested in.

For each file in the list, right-click the link and direct your browser to save the file to disk.