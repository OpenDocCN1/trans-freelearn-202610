Here is a screenshot of the command and its output:

![](bbox=[0.171, 0.13, 0.829, 0.304])

13. Download and verify the exfiltrated context file using the following command. Replace the `<blob_container_url>` placeholder with the value of **Exfiltration Storage Location** that you copied from the script’s output:

```
```powershell
Invoke-WebRequest -Uri "<blob_container_url>" -OutFile azureprofile.zip
Get-ChildItem azureprofile.zip
```
```

Here is a screenshot of the command and its output:

![](bbox=[0.17, 0.52, 0.83, 0.763])