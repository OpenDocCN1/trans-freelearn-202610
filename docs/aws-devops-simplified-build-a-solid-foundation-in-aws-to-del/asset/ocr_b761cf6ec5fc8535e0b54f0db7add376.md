implementation using the **AWS User Notifications** service, which can be used to send out notifications through commonly used communication platforms and channels.

# Implementing security-in-depth principles

Security should be applied at multiple layers, starting from the edge of the network to the perimeter of VPCs and subnets, and moving down to the instance level, securing the application and operating system itself. It’s a good practice to implement security controls, both preventive and detective, to allow healthy communication patterns.

With a combination of **AWS service control policies (SCPs)**, AWS Config Rules, and networking controls such as subnet **network access control lists (NACLs)**, security groups, and so on, appropriate measures can be taken to implement security in depth.

# Protecting data in transit and at rest

Many AWS services offer out-of-the-box data encryption at rest. At the bare minimum, this provides security coverage for your data, but the ownership and management of the encryption key resides with AWS. Customers operating in more sensitive domains such as financial services might use their own encryption keys that can be managed using AWS **Key Management Service (KMS)**.

# Automating security measures as code

Utilize automated software-based security mechanisms to enhance scalability while ensuring cost-effectiveness. Develop secure architectures with version-controlled templates defining and managing security controls as code.

# Reducing direct human data access patterns

Utilize mechanisms and tools that minimize or eliminate the need for direct access or manual handling of data, reducing the risk of mishandling and human errors involving sensitive data.

# Preparing for security incidents

Every organization operates in the cloud in a slightly different way when it comes to incident management. It is therefore necessary to have a well-defined incident management procedure that is tested and tailored to your unique needs and team structures.

Up next, let’s look into a common security monitoring requirement for break-glass user access, which we discussed a while ago.

# *Security monitoring for break-glass user access*

In order to trigger alerts when credentials of break-glass users are used, organizations can implement a solution that is detailed in *Figure 12.2:*