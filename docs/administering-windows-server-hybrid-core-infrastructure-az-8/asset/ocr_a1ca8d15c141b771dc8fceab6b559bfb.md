```
$Location = "uksouth"
$PublicIpAddressName = "az800-wac-publiccip"
$Size = "Standard_B2ms_v3"
$Image = "Win2019Datacenter"
$Credential = Get-Credential
```

The following script deploys WAC to a new VM, resource group, and VNet, as set in the preceding variables; it utilizes the MSI from aka.ms/WACDownload and the MSI-generated self-signed certificate:

```
$scriptParams = @{
    ResourceGroupName = $ResourceGroupName
    Name = "az800-wac-vm1"
    Credential = $Credential
    VirtualNetworkName = $VirtualNetworkName
    SubnetName = $SubnetName
    GenerateSslCert = $true
}
./Deploy-WACAzVM.ps1 @scriptParams
```

This deployment method is covered in more detail in the following Microsoft documentation article: https://docs.microsoft.com/en-gb/windows-server/manage/windows-admin-center/azure/deploy-wac-in-azure#deploy-using-script.

# Manual deployment

To manually deploy the WAG gateway to an Azure VM, you first create an Azure VM for use as the WAG gateway. You then need to download the MSI installer directly to the VM or a local machine and copy the installer onto the VM; the installer can be downloaded from https://www.microsoft.com/en-gb/evalcenter/download-windows-admin-center.

Launch the MSI installer on the WAC gateway VM and follow the wizard instructions. You will need a certificate that you can provide or let the MSI generate a self-signed certificate.

For communication, port 443 (HTTPS) is used by default by the installer; you should allow this communication through any network filter, such as a network security group (NSG) or firewall.