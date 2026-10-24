# CorDapp

The core model of Corda consists of state objects, transactions and transaction protocols, which when combined with contract code, APIs, wallet plugins, and user interface components results in constructing a Corda distributed application (CorDapp).

Smart contracts in Corda are written using Kotlin or Java. The code is targeted for JVM. JVM has been modified slightly in order to achieve deterministic results of execution of JVM bytecode. There are three main components in a Corda smart contract as follows:

+   1. Executable code that defines the validation logic to validate changes to the state objects.
+   2. State objects represent the current state of a contract and either can be consumed by a transaction or produced (created) by a transaction.
+   3. Commands are used to describe the operational and verification data that defines how a transaction can be verified.

# Development environment

The development environment for Corda can be set up easily using the following steps.

Required software includes the following:

+   1. JDK 8 which is available at http://www.oracle.com/technetwork/java/javase/downloads/index.html.
+   2. IntelliJ IDEA community edition which is free and available at https://www.jetbrains.com/idea/download.
+   3. H2 database platform independent zip, and is available at http://www.h2database.com/html/download.html.
+   4. Git, available at https://git-scm.com/downloads.
+   5. Kotlin language, which is available for IntelliJ, and more information can be found at https://kotlinlang.org/.
+   6. Gradle is another component that is used to build Corda.

Once all these tools are installed, smart contract development can be started. CorDapps can be developed by utilizing an example template available at https://github.com/corda/cordapp-template. Detailed documentation on how to develop contract code is available at https://docs.corda.net/.