```
echo "\"$hostname\" $item_key $value_timestamp
$([ [ $(( ${RANDOM}%$probability) ) < 1 ]] && echo 0 ||
echo 1)" >> $item_key.txt
done
done
```

This script will generate values for each of our three item keys every hour, for one year in the past, starting at the current time. For each entry, there is a small chance of getting a value of 0, which is failure. The result will be random, but it should fluctuate around our acceptable SLA level, so hopefully we will get some services that do meet the SLA level and some that do not. As all of the values are sent in with a one-hour interval and it is quite unlikely that two failures would follow one another, no downtime should be longer than one hour. Assuming the script was saved as generate_values.sh, you just have to run it once:

```
$ ./generate_values.sh
```

Three files should be generated:

+   - code_repo.txt
+   - ticketing.txt
+   - warehouse_analytics.txt

> The following could generate quite a lot of alert e-mails. If you would like to avoid that, disable the actions we added earlier.

Now run zabbix_sender for each of these files:

```
$ zabbix_sender -z 127.0.0.1 -T -i code_repo.txt
$ zabbix_sender -z 127.0.0.1 -T -i ticketing.txt
$ zabbix_sender -z 127.0.0.1 -T -i warehouse_analytics.txt
```

The output on each invocation should be similar to this:

```
info from server: "processed: 250; failed: 0; total: 250; seconds spent: 0.001747"
...
info from server: "processed: 10; failed: 0; total: 10; seconds spent: 0.000063"
sent: 8760; skipped: 0; total: 8760
```