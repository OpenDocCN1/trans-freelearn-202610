Here is some Python code to handle all these steps. We will go over the extraction parts in detail at the end of the code listing:

```
import os
import re
import urllib2
import datetime
import numpy

alllinks = []
timelist = []
for filename in os.listdir(os.getcwd()):
    if filename.endswith('.rss'):
        f = open(filename, 'r')
        linktext = ''
        linkurl = ''
        for line in f:
            # find the URLs for discussion threads
            linktext = re.search('(<guid>)(.+?)(</guid>)', line)
            if linktext:
                linkurl= linktext.group(2)
                alllinks.append(linkurl)
        f.close()

mainmessage = ''
reply = ''
maindateobj = datetime.datetime.today()
replydateobj = datetime.datetime.today()
for item in alllinks:
    print "==="
    print "working on thread\n" + item
    response = urllib2.urlopen(item)
    html = response.read()
    # this is the regex needed to match the timestamp
    tuples = re.findall('lia-message-posted-on\\">s+<span
class=\\"local-date\\"\\">\\xe2\\x80\\x8e(.?)</span>\\s+<span
class=\\"local-time\\"\\>([\\w:\\sAM|PM]+)</span>', html)
    mainmessage = tuples[0]
    if len(tuples) > 1:
        reply = tuples[1]
    if mainmessage:
        print "main: "
        maindateastr = mainmessage[0] + " " + mainmessage[1]
        print maindateastr
        maindateobj = datetime.datetime.strptime(maindateastr,
'%m-%d-%Y %I:%M %p')
    if reply:
        print "reply: "
        replydateastr = reply[0] + " " + reply[1]
```