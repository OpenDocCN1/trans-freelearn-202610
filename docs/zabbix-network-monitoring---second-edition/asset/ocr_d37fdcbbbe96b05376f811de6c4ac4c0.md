# Advanced Item Monitoring

We changed the second key parameter to "error|warning 6[0-6]", including the double quotes. This regular expression should match all errors and warnings that start with numbers 60-66. We had to double quote it, because the regexp contained square brackets, which are also used to enclose key parameters. To test this out, let's insert in our log file several test lines:

```
```sh
$ echo "2016-08-13 13:07:05 seventh log entry – all good" >> /tmp/zabbix_logmon/logfile2

$ echo "2016-08-13 13:08:05 eighth log entry – just an error" >> /tmp/zabbix_logmon/logfile2

$ echo "2016-08-13 13:09:05 ninth log entry – some warning" >> /tmp/zabbix_logmon/logfile2

$ echo "2016-08-13 13:10:05 tenth log entry – warning 13" >> /tmp/zabbix_logmon/logfile2

$ echo "2016-08-13 13:11:05 eleventh log entry – warning 613" >> /tmp/zabbix_logmon/logfile2
```
```

Based on our regular expression, the log monitoring item should:

+   - Ignore the seventh entry, as it contains no error or warning at all
+   - Catch the eighth entry, as it contains an error
+   - Ignore the ninth entry — it has a warning, but no number following it
+   - Ignore the tenth entry — it has a warning, but the number following it does not start within range of `60-66`
+   - Catch the eleventh entry — it has a warning, the number starts with 61, and that is in our required range, `60-66`

Eventually, only the eighth and eleventh entries should be collected. Verify that in the latest data page only the entries that matched our regexp should have been collected.

The regexp we used was not very complicated. What if we would like to exclude multiple strings or do some other, more complicated filtering? With the POSIX EXTENDED regular expressions that could be somewhere between very complicated and impossible. There is a feature in Zabbix, called **global regular expressions**, which allows us to define regexps in an easier way. If we had a global regexp named `Filter logs`, we could reuse it in our item like this:

```
```text
log[/tmp/zabbix_logmon/logfile2,@Filter logs,,,skip]
```
```

Global regular expressions are covered in more detail in *Chapter 12, Automating Configuration.*