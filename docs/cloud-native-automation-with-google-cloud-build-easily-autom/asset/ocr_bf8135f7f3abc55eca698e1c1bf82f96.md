Here is a sample output of the preceding command:

```
insertId: 166b01hd1dvc
logName: projects/##project-id_redacted##/logs/cloudaudit.googleapis.com%2Factivity
operation:
  id: operations/build/##project-id_redacted##/
    YzYwMzA2NTktMjVlZC00ZWQ2LTlKyzQtMmZjMWI5NzY4NmRj
  last: true
  producer: cloudbuild.googleapis.com
protoPayload:
  '@type': type.googleapis.com/google.cloud.audit.AuditLog
  authenticationInfo:
    principalEmail: ##email_redacted##
  authorizationInfo:
  - granted: true
    permission: cloudbuild.builds.create
    resource: projects/00000041cd1ce997
    resourceAttributes: {}
  methodName: google.devtools.cloudbuild.v1.CloudBuild.
CreateBuild
  requestMetadata:
    destinationAttributes: {}
    requestAttributes: {}
  resourceName: projects/##project-id_redacted##/builds
  serviceName: cloudbuild.googleapis.com
  status: {}
receiveTimestamp: '2022-01-19T00:26:46.102748864Z'
resource:
  labels:
    build_id: c6030659-25ed-4ed6-9dc4-2fc1b97686dc
    build_trigger_id: ''
    project_id: ##project-id_redacted##
  type: build
severity: NOTICE
timestamp: '2022-01-19T00:26:45.655472495Z'
```