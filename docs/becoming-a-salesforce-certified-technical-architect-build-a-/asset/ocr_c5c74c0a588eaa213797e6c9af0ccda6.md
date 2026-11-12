# ## Identity

This is used to recognize a particular individual or thing. Usually, you normally use names to identify yourself. However, you might come across more than one person with the same name, especially when you deal with a huge number of individuals. In systems, a unique alphanumeric identifier is usually utilized to recognize someone, such as a national ID, social security number, or even an employee ID. Other systems utilize a unique identifier in a specific format, such as the email address format.

Salesforce is a good example of such a system. Once the identity of a particular user is recognized, all operations would run under its context. Salesforce can apply various security policies and run the appropriate monitoring activities based on the given identity. To recognize a particular identity and start using it, you need to ensure that the person claiming it is authentic. This is where authentication comes into the picture.

# ## Authentication

This is a term given to the process or series of processes used to identify a particular individual or thing uniquely, securely, and confidently. This step is used often in today’s applications, where the user is expected to prove their identity using one or more techniques, and once that is done, the application would operate based on the recognized identity (sometimes referred to as the _logged-in user_ or _authenticated user_).

Authentication normally takes place by requesting one or more of the following inputs from the individual/thing: something that is known only to that individual (such as a password or unique key); something that the individual/thing has (such as a physical ID card, dongle, or an embedded chip); or something related to the physical structure of the identified object (such as a fingerprint or iris scan). Once the individual/thing is authenticated, there will be a need to determine _what_ they are allowed to access within the current system. This is where authorization comes into the picture.

# ## Authorization

This process takes place after authenticating a particular user to grant the right privileges to see, do, and have things within a particular system. In software applications, it is common to have a **super user** (also known as an **administrator**) who defines the allowed policies and permissions within a particular system. The administrator would likely also be responsible for defining/creating other users.

Enterprises should aim to apply the principle of least privilege while designing their authorization strategy. This requires that users, applications, and devices are granted the minimal sufficient privileges required to operate normally and deliver their desired functionality. You should keep that in mind while designing your overall security architecture and particularly avoid granting service users (also known as integration users) more privileges than necessary.