# ZAP Scanning Report

# Summary of Alerts

| Risk Level | Number of Alerts |
|---|---:|
| High | 1 |
| Medium | 1 |
| Low | 2 |
| Informational | 0 |

# Alert Detail

| Field | Value |
|---|---|
| High (Medium) | SQL Injection |
| Description | SQL injection may be possible. |
| URL | `http://localhost:9999/petclinic/oups.htm;jsessionid=E47011BD0E9AC2E848D2F180797D2E7E?query=query%22+AND+%221%22%3D%221` |
| Method | GET |
| Parameter | query |
| Attack | `query” AND ”1”=”1` |
| URL | `http://localhost:9999/petclinic/oups.htm;jsessionid=E47011BD0E9AC2E848D2F180797D2E7E?query=query+AND+1%3D1+--` |
| Method | GET |
| Parameter | query |
| Attack | `query AND 1=1 --` |
| URL | `http://localhost:9999/petclinic/oups.htm;jsessionid=E47011BD0E9AC2E848D2F180797D2E7E?query=query+AND+1%3D1+--+` |
| Method | GET |

+   - You have integrated Jenkins and OWASP ZAP for security testing using Jenkins and you can integrate it in the pipeline too.