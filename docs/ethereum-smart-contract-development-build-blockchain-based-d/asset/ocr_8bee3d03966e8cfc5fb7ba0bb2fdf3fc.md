# # Factory pattern

This design pattern is a kind of creational design pattern. Using this pattern, objects can be created without specifying the exact class of creation.

*Figure 11.4* is a simple hub (`Barrel`) that deploys contracts (`Whisky`) from a template and keeps track of the contracts created.

`Whisky` is part of the source file so `Barrel` can "see it" during compilation. `Whisky`'s bytecode will become part of `Barrel` so the `newWhisky()` invocation knows what to do.

It is not necessary to deploy the template (`Whisky`), once the hub (`Barrel`) has been deployed.

We can create as many of the latter as needed by calling a function in the former: