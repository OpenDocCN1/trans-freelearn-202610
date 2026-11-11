In the instance where you want to modify a service's description, you first need to stop the service by leveraging the stop-service cmdlet. You then call set-service cmdlet with -name name of the service, and the -Description parameter with the description that you want to set for the particular service. The description property is unique as it is not made available to the get-service cmdlet. To get around this, you need to leverage the get-wmiobject cmdlet. To view the description, you can use the get-wmiobject cmdlet referencing the win32_service class, with the -filter parameter referencing DisplayName='Display Name'. After executing this script, the description will print to the screen. After setting the description, you can start the service using the start-service cmdlet.

To set the description for the Windows Audio Service, you can do the following:

```
```powershell
$olddesc = (get-wmiobject win32_service -filter "DisplayName='Windows Audio'").description
$olddesc
stop-service -DisplayName "Windows Audio"
Set-service -name "Audiosrv" -Description "My New Windows Audio Description."
(get-wmiobject win32_service -filter "DisplayName='Windows Audio'").description
Set-service -name "Audiosrv" -Description $olddesc
(get-wmiobject win32_service -filter "DisplayName='Windows Audio'").description
start-service -DisplayName "Windows Audio"
```
```

The output of this is shown in the following screenshot:

![](bbox=[0.07, 0.61, 0.9, 0.71])