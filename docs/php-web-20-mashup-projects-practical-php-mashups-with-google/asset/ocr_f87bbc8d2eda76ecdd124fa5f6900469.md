| Item Child Element | Required? | Description |
|---|---|---|
| comments | No | URL to a page of reader comments of the item. |
| enclosure | No | Allows a media file to be included for the item. The URL to the media file is included in a required attribute named url.length, in bytes, of the media file and type, the MIME type of the file, are also required. The most common use of this tag is to specify an MP3 in podcasts. |
| guid | No | A unique identifier for the item. |
| pubDate | No | The last publication date of the item. |
| source | No | A third-party source for the item. Used in citation sources for an item. |

# Atom Syndication Format

There is a new, third syndication feed called Atom. Atom attempts to bridge the extensibility and simplicity goals of both RSS branches. Structurally, Atom feeds share a similar model to RSS – metadata followed by entries. However, the element names are quite different. We won't be using Atom in this project, but you should be aware of it. Although it is the least mature of the formats and the market share is relatively smaller, it is the only format supported by an industry standards body (specifically, the Internet Engineering Task Force) and is already being used by powerhouses like Google News.

# YouTube Overview

YouTube almost needs no introduction these days. The site is as ubiquitous as many of the previous sites we discussed – Amazon, Google, Yahoo!, and MSN. Links to its videos have been passed around email accounts. In case you have lead a very sheltered existence, we will take a brief look at what YouTube does, some features, and its Web API.

In a nutshell, YouTube is a site that allows users to share homemade videos with the public via the Internet. Users upload any video they wish (with much respect to copyright laws) and other users may view and comment on them. The latter has led YouTube to become a strong social networking site in addition to just sharing videos.