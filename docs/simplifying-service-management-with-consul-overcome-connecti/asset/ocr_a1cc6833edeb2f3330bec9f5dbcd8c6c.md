I had mentioned the necessity to differentiate between Adrian, Dave, and now Jannick, as they log in to the Consul system. This is to differentiate between actions performed within the audit log by each individual. This isn't necessarily to stand back and point fingers, but more to ensure that the process is followed and changes are tracked throughout the system.

Enabling the audit log is pretty simple within Consul servers. When enabled, Consul will spit out entries to track who did what and when, and it all gets written to a file. Don't worry – secret information isn't included, which gives greater importance to enabling authentication methods such as OIDC to uniquely distinguish users. The logs are written to a file in JSON format, which can be ingested into most log analysis utilities such as DataDog, Splunk, and Elasticsearch. When configuring the logging, however, be sure to set limits on the number of log files and the rotation frequency… nobody ever likes to admit that they ran out of space due to logging!

```
```hcl
audit {
  enabled = true
  sink "consul_audit" {
    type   = "file"
    format = "json"
    path   = "data/audit/audit.json"
    delivery_guarantee = "best-effort"
    rotate_duration = "24h"
    rotate_max_files = 15
    rotate_bytes = 25165824
  }
}
```
```

The output of the log file identifies several important pieces of information, specifically the timestamp and the token accessor. This accessor can be linked back to the individual tokens and roles (and therefore users) that accessed the system. I personally find the source IP address to be very interesting in audit logs, especially when I can do geographic association and determine who is accessing the system from various parts of the world. Of course, the entry includes what they did with the system as well!

```
```json
{
  "created_at": "2020-08-25T10:35:23.258765383Z",
  "event_type": "audit",
  "payload": {
    "id": "3a880e2b-3f43-5d4d-97a6-26aec5eacc0c",
```
```