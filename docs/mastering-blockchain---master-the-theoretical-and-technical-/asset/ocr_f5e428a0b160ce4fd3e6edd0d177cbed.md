Other standard software bugs such as integer overflow and underflow are also quite significant and any use of integer variables should be carefully implemented in solidity. For example, a simple program where uint8 is used to parse through elements of an array with more than 255 elements can result in an endless loop. This occurs because uint8 is limited to 256 numbers.

In the following sections, two examples of contract verification will be shown using Why3 and Oyente respectively.

# Why3 formal verification

Formal verification of solidity code is now available as a feature in the solidity browser. First the code is converted into Why3 language that the verifier can understand. In the example below, a simple solidity code that defines the variable z as maximum limit of uint is shown. When this code runs, it will result in returning 0, because uint z will overrun and start again from 0. This can also be verified using Why3, which is shown below:

![](bbox=[0.09, 0.46, 0.9, 0.66])

Solidity online compiler with formal verification

Once the solidity is compiled and available in the formal verification tab, it can be copied into the Why3 online IDE available at http://why3.lri.fr/try/. The example below shows that it successfully checks and reports integer overflow errors. This tool is under heavy development but is still quite useful. Also, this tool or any other similar tool is not a silver bullet. Even formal verification generally should not be considered a panacea because specifications in the first place should be defined appropriately: