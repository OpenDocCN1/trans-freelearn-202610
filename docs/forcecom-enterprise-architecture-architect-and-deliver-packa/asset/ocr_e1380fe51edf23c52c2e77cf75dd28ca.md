The following record is defined by you the packager developer to effectively declare that **Award Championship Points** callout exists and can be configured. This should also be included in your package:

| Action | Label * | Callout Name | Namespace Prefix | Interface Type | Protected Component |
|---|---|---|---|---|---|
| Edit : Del | Award Championship Points | AwardChampionshipPoints | c2v17 | ContestantService.IAwardChampionshipPoints | ✓ |

> > Notice how the Custom Metadata Type fields supports Developer X providing a class name that is either fully qualified (perhaps from an extension package, for example, fforceext.SimpleCalc) or a local class (defined in the subscriber org), for example, DeveloperXCustomCalc.

The following code is a Selector method (see CalloutsSelector) class that reads the registered interface implementations. It uses the Type.forName method to construct applicable Apex types and returns a Map of Apex classes implementing each interface:

```
public Map<Type, Type> selectAllCallouts() {
    // Query custom metadata records for callouts
    Map<Type, Type> calloutsByInterfaceType =
        new Map<Type, Type>();;
    for(Callout__mdt record :
        Database.query(newQueryFactory().toSOQL())) {
        if(Callout__mdt.ApexClass__c!=null) {
            // Namespace of the interface is that
            // of the custom metadata type
            Type interfaceType =
                Type.forName(
                    record.NamespacePrefix__c + '.' + record.InterfaceType__c);
            // Implementing class can optionally specify
            // the namespace if needed
            Type implType =
                Type.forName(record.ApexClassNamespacePrefix__c,
                    record.ApexClass__c);
            calloutsByInterfaceType.put(interfaceType, implType);
        }
    }
    return calloutsByInterfaceType;
}
```