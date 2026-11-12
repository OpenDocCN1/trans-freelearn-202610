There are two common techniques for data obfuscation, namely **pseudonymization**, and **anonymization**. Here is a brief description of the two:

+   - **Anonymization**: This works by changing and scrambling the contents of fields so they become useless. For example, a contact named Rachel Greene could become hA73Hns#d$. An email address such as RachelG@gmail.com could become an unreadable value such as JA7ehK23.
+   - **Pseudonymization**: This converts a field into readable values unrelated to the original value. For example, a contact named Rachel Greene could become Mark Bates. An email address such as RachelG@gmail.com could become MarkBates@SomeDomain.com.

Which technique amongst these two you should choose depends on the degree of risk associated with the masked data and how the data will be processed. Pseudonymous data still allows some sort of reidentification (even if it is remote or indirect), while anonymous data cannot be reidentified. A common way to anonymize data is by scrambling data, a process that can sometimes be reversible; for example, “London” could become “ndooln.” This masking technique allows a part of the data to be hidden with a static or random character. On the other hand, **data blurring** uses an approximation of data values to make it impossible to identify a person or to make the data’s meaning obsolete.

# Data Erasure

**Data erasure** (also referred to as data clearing, data destruction, or data wiping) is a software-based activity where specific data is overwritten with other values to destroy electronic data and make it unrecoverable. This is different from data deletion, even though they sound the same.

**Data deletion** can leave data in a recoverable format (for example, by simply removing the reference to it from an index table while still maintaining it on the storage disk). Data erasure, on the other hand, is permanent and particularly important for highly sensitive data. It is important to understand the difference between these terms so that you can suggest the best strategy to your stakeholders while also taking into consideration the limited control they have over how the data is ultimately stored in Salesforce.

It is worth mentioning that encrypted data can be destroyed/erased permanently by simply destroying the encryption keys.

Another key topic the data governing body needs to cover is data regulatory compliance. With the increased amount of gathered customer and business data, it has become essential to introduce rules that govern the use of that data. As an architect, you must be aware of these regulations to design a fully compliant solution. You will likely need to work with subject matter experts to ensure your solution fulfills all regulatory requirements, but you should still be able to cover a good amount of that by yourself. You also need to be able to explain how your solution is compliant with these regulations to your stakeholders.