We have syslog data coming in but it is in raw format, meaning it is now getting parsed into usable data. This is something we are going to perform with Elasticsearch parsing. The trick is to parse through the incoming syslog data and fix up the data before we store it in the database. Follow along with the next instructions to get this done:

+   7. Elasticsearch uses ingest parser directive files to be able to receive logs from senders and store the data in a shape and form the user specifies. We can see the existing directives files in the /opt/so/conf/elasticsearch/ingest/ folder, as illustrated in the following screenshot:

![](bbox=[0.14, 0.29, 0.8, 0.49])

Figure 9.29 – Exercise 4: Security Onion – Elasticsearch ingest parser directive files

We can add our own custom directives by creating an ingest parsing directive file for them in /opt/so/saltstack/local/salt/elasticsearch/files/ingest/. Elasticsearch will include the directives in the custom folder on (re)start. We will add our configuration file for parsing the SilentDefense Syslog messages here.

+   8. Download the syslog and silentdefense configuration files, as follows:

+   a) The first file to download is the syslog Elasticsearch ingest parsing configuration file from https://github.com/SackOfHacks/Industrial-Cybersecurity-2nd-Edition/blob/main/security-onion-logstash/syslog. This will overwrite the Security Onion syslog parsing to add the following grok parser directive:

```
"<111>1 %{TIMESTAMP_ISO8601:sd.timestamp} %{SYSLOGHOST:sd.hostname} %{WORD:sd.logname} - - - CEF:0\\|%{DATA:sd.vendor}\\\\|%{DATA:sd.product}\\\\|%{DATA:sd.product_version}\\\\|%{DATA:sd.alert_name}\\\\|%{DATA:sd.alert_title}\\\\|%{DATA:sd.alert_number}\\\\|%{GREEDYDATA:sd.real_message}"
```