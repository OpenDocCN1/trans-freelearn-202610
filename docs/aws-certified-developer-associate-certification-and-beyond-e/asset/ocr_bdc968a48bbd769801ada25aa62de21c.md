# Asymmetric KMS Keys

Asymmetric KMS encryption keys are made of a key pair – a private key and a public key. This encryption option is advantageous when you need to enable external parties to perform encryption on data that later needs to be decrypted by you. Using a public/private key pair maintains security even if you freely distribute the public key outside the AWS environment because only you can decrypt any encrypted data using the private key.

The private key is stored in KMS and never leaves unencrypted. You need to call KMS to perform the decryption operation using this private key.

Deciding which encryption key option depends on your use case. If you need to perform encryption operations outside of AWS and access to KMS is not possible, you can use asymmetric keys. If you need to store and manage all encryption and decryption operations using an AWS service such as Amazon S3, then symmetric encryption keys are a great choice.

As previously mentioned, you may need to create data keys or even data key pairs to encrypt data larger than 4 KB. Let us examine this concept next.

# What Are Data Keys?

AWS KMS keys (AWS-managed or customer-managed) are used to encrypt and decrypt plaintext data up to a maximum size of 4 KB. Because you will mostly need to encrypt much larger datasets, you can use the KMS keys to create and encrypt data keys using the GenerateDataKey API. These data keys are symmetric and used for data encryption because they can encrypt data over 4 KB in size and can be downloaded and used outside of AWS. The operation will return both the plaintext copy of the data key and an encrypted copy.

Let us examine the workflow process for using a data key to encrypt your data, as depicted in the diagram in *Figure 7.7*:

![](bbox=[0.185, 0.65, 0.832, 0.884])

Figure 7.7 – Encrypting your data using data keys