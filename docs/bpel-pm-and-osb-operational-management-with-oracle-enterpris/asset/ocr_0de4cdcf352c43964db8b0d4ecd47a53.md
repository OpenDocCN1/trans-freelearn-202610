3. View the differences between the current and the saved configurations:

+   - Click on **Configuration Files**.
+   - Select **config.xml**.
+   - Click on **Compare File Contents**.
+   - Observe the differences marked in red.

![](bbox=[0.1, 0.07, 0.9, 0.24])

![](bbox=[0.11, 0.37, 0.89, 0.57])

```
<!-- XML diff excerpt lines 12-26 -->
12 <sec-role-mapper ...>...</sec-role-mapper>
13 <sec-authorizer ...>...</sec-authorizer>
14 <sec-adjudicator ...>...</sec-adjudicator>
15 <sec-credential-mapper ...>...</sec-credential-mapper>
16 <sec-cert-path-provider ...>...</sec-cert-path-provider>
17 <sec-cert-path-builder>WebLogicCertPathProvider</sec-cert-path-builder>
18 <sec-name>myrealm</sec-name>
19 <realm>...</realm>
20 <default-realm>myrealm</default-realm>
21 <credential-encrypted>{3DES}...</credential-encrypted>
22 <code-manager-username>...</code-manager-username>
23 <node-manager-password-encrypted>{3DES}...</node-manager-password-encrypted>
24 </security-configuration>
25 <server>
26 <name>xbusServer</name>
<!-- differences marked in red appear below line 26 in the right pane -->
```