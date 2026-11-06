# Shares and Time Machine

We will create a few entries to use your external drive for the shares that you need, and this version of Netatalk also supports Time Machine.

You will need to edit this file to customize some of the settings as follows:

```
/etc/netatalk/AppleVolumes.default
#Ammend default to look like this
:DEFAULT: options:upriv,usedots,rw,tm
#Add this to end of the file
/nas/USB001/public "Public"
/nas/USB001/work "James Bond"
```

Read the configuration file for more options and adjusting security. After saving, you must restart Netatalk using the following command:

```
service netatalk restart
```

# BitTorrent Sync

All the bad things that you may have heard about torrents were primarily caused by private companies who were deliberately placing infected files to try and track users sharing copyrighted material or malicious users who were after your personal details.

Sync is not a public network; it only uses the revolutionary peer-to-peer technology from conventional torrents, which allows you to share an unlimited amount of data with an unlimited number of computers. Files are not stored on the cloud. They are stored on all the computers that have BTSync installed on them, and you can control who has access and where these files will be stored. BTSync includes file versioning, sharing with mobile phones, LAN synchronizing, and delta updates. It also allows massive file transfers of gigabytes without any problem as long as your file system can support such large files.

# Installing BTSync 2

At the time of writing this book, there were a few repositories that kept this package with startup scripts. A word of warning: do not trust sources other than the ones that are enabled by default. People can easily alter code and place their own version of the binary with some nasty backdoors. Go to the official website, or use the links provided by the official website.