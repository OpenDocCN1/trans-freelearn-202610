```
timelist.append(timeinhours)
    except:
        pass
    except:
        pass

print numpy.mean(timelist)
print numpy.std(timelist)
print numpy.median(timelist)
```

In this code, the initial for loop zips through each message and extracts the three pieces of data we are interested in. (This program does not store these to a separate file or to disk, but you could add this functionality if you wish to.) This portion of the code also creates two important lists:

+   - `originals[]` is a list of original messages. We make the assumption that these are primarily questions being asked of the list members.
+   - `replies[]` is a list of reply messages. We assume that these are primarily answers to questions asked in another message.

The second for loop processes each message in the list of original messages, doing the following, if there is a reply to the original message, try to figure out how long that reply took to be sent. We then keep a list of reply times.

# Extraction code

For this chapter, we are mostly interested in the cleaning and extraction portion of the code, so let's look closely at those lines. Here, we process each line of the e-mail file looking for three e-mail headers: In-Reply-To, Message-ID, and Date. We use regex searching and grouping, just like we did in Method 1 earlier in this chapter, to delimit the headers and easily extract the values that follow:

```
for line in f:
    irt = re.search('(In\-Reply\-To: <)(.+?)@', line)
    mid = re.search('(Message\-ID: <)(.+?)@', line)
    dt = re.search('(Date: )(.+?)\r', line)
    if irt:
        i = irt.group(2)
    if mid:
        m = mid.group(2)
    if dt:
        d = dt.group(2)
```