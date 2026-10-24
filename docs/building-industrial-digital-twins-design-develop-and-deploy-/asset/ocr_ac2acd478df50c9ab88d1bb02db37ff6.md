Serialization follows a standardized structure that helps improve collaboration and interoperability. The following diagram shows the metamodel of an asset in the AAS structured approach:

![](bbox=[0.209, 0.167, 0.791, 0.426])

Figure 4-9 – Metamodel of an asset in the AAS structure

The following snippet of XML code shows the structure of defining the asset and its component or sub-model hierarchy in a machine-readable format:

```
```
...
<aas:assetAdministrationShells>
    <aas:assetAdministrationShell>
        <aas:idShort>ExampleMotor</aas:idShort>
        <aas:category>CONSTANT</aas:category>
        <aas:identification idType="URI">http://customer.com/
aas/9175_7013_7091_9168</aas:identification>
        <aas:assetRef>
            <aas:keys>
                <aas:key type="Asset" local="true"
idType="URI">http://customer.com/assets/KHBVZJSQKIY
</aas:key>
            </aas:keys>
        </aas:assetRef>
        <aas:submodelRefs>
            <aas:submodelRef>
                <aas:keys>
                    <aas:key type="Submodel" local="true"
```
```