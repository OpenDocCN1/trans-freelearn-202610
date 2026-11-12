3. This results in the current year being displayed in the **Year** field as shown in the following screenshot:

![](bbox=[0.075, 0.102, 0.87, 0.302])

The preceding screenshot shows the user interface shown by Salesforce when the user is using Salesforce Classic. Salesforce Classic is the predecessor to their latest user interface technology known as Lightning Experience. Both are options for users and will likely remain so for some time until users and applications are migrated over. This book also includes information on Lightning throughout its chapters, as well as a dedicated Chapter 10, Lightning.

Unfortunately, by default, the Apex code does not apply default values when you construct an SObject in the direct manner via `new Season();` however, if you utilize the `SObjectType.newSObject` method, you can request for defaults to be applied as follows:

```
Season__c season = (Season__c)
Season__c.SObjectType.newSObject(null, true);
System.assertEquals('2016', season.Year__c);
```

# # Encrypted fields

However unlikely you feel data theft might be, there are some markets and government regulations that require encrypted fields. Salesforce offers two ways to help ensure your application data is stored in their data centers in an encrypted form. There are two options to consider for encrypting field values at rest, that is records physically stored on permanent storage.

+   * Classic Encryption: Encrypted fields leverage 128-bit master keys and use the Advanced Encryption Standard (AES) algorithm to store values; they are displayed using a character mask (currently not developer definable). Such fields are packagable, though this only applies to Text fields.