+   - **Secure Enclave Processor (SEP)**, which runs an Apple-modified version of the L4 microkernel and provides computing power exclusively to Secure Enclave.
+   - A memory protection engine.
+   - A **True Random Number Generator (TRNG)**, which is used to generate random cryptographic keys.
+   - Dedicated **Advanced Encryption Standard (AES)** hardware engines, which communicate directly with the SEP through a secure channel and perform in-line encryption and decryption as files are written or read.
+   - A **unique ID (UID)**, a cryptographic key that uniquely identifies the device. The UID is randomly generated and fused directly into Secure Enclave's hardware during device manufacturing, so it isn't visible outside the device.
+   - A dedicated, secure, nonvolatile storage system that can only be accessed by Secure Enclave. This is where data encryption keys are stored, ensuring that these are never exposed to iOS systems or applications.

The following diagram shows the different components of Secure Enclave: