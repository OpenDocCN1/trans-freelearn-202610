# Software Composition Analysis with dependency-check

If you observe the output of the pipeline execution, you will notice that the first stage, ApplicationValidation, reports an error in the SCAcheck CodeBuild action. Checking the CodeBuild build execution log reveals a finding in the Flask framework and since the criticality exceeds the threshold value of 7 (as configured in the buildspec.yml file), an error has been raised. Such security validations flag any new dependency that your developers might introduce into the application. The criticality of these findings can vary, and not all would be relevant, but having some false positives is still better than no reporting at all.

In this case, the OWASP dependency-check utility, in addition to flagging a potential risk, gives us a URL that contains more information about the specific CVE that has been reported:

```
"vulnerabilities": [
  {
    "source": "OSSINDEX",
    "name": "CVE-2023-30861",
    "severity": "HIGH",
    "cvssv2": { 
      "score": 7.5,
    ...
    ...
    "cwes": [
      "CWE-539"
    ],
    ...
    "description": "Flask is a lightweight WSGI web application framework. When all of the following conditions are met, a response containing data intended for one client may be cached ... https://ossindex.sonatype.org/vulnerability/CVE-2023-30861
    ...
```

As shown in Figure 9.4, visiting the CVE URL in your web browser further explains how certain configurations in the Python Flask framework can be exploited. dependency-check parsed the requirements.txt file to figure out the third-party dependency names and their version. It then verified these dependencies against the publicly available vulnerabilities database – in this case, NVD. Since the version we are using in the application is flagged as a potential risk, it was called out by the dependency-check utility. However, in this case, we are not working with cookies or headers, so we can safely suppress the finding: