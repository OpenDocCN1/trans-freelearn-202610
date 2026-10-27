In the preceding code, we created the `paint` operation to paint the passenger cars and the `paintTruck` operation to paint trucks. Later, we created the `passengerCarAssembly` line and added a new passenger car to the line via the `add` method. We can run the `paint` operation on the passenger car by calling the `make` method of the `passengerCarAssembly` line. Next, we intentionally made a mistake and tried to paint the truck on the assembly line for passenger cars, which resulted in the following runtime exception:

```
Make paint on Passenger Car
Unhandled exception:
type 'PassengerCar' is not a subtype of type 'Truck' of 'car'.
#0 Operation.call (../generics_operation.dart:10:10)
#1 Assembly.make.<anonymous
closure>(../generics_assembly.dart:16:15)
#2 List.forEach (dart:core-patch/growable_array.dart:240)
#3 Assembly.make (../generics_assembly.dart:15:18)
#4 main (../generics_assembly_and_operation_room.dart:20:28)
...
```

This trick with the `call` method of the `Function` type helps you make all the aspects of your assembly line generic. We've seen how to make a class generic and function to make the code of our application safer and cleaner.

> The documentation generator automatically adds information about generics in the generated documentation pages.

To understand the differences between errors and exceptions, let's move on to the next topic.

# Errors versus exceptions

Runtime faults can and do occur during the execution of a Dart program.
We can split all faults into two types:

+   - Errors
+   - Exceptions