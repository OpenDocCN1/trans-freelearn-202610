To address the issue with JSR 303 support in the auto-generated Flight entity instance by FlightDataOnDemand_Roo_DataOnDemand.aj, we can write custom setNumOfSeats, setOrigin, and setDestination methods in the FlightDataOnDemand.java class, which does the following:

+   Checks that the length of origin and destination fields are within the limits defined by @Size

+   Provides checks for the @DecimalMin and @DecimalMax JSR 303 annotations

It is important to note that if you create setNumOfSeats, setOrigin, and setDestination methods in the FlightDataOnDemand.java class, then Roo removes these methods from the corresponding FlightDataOnDemand_Roo_DataOnDemand.aj AspectJ ITD. This requires that the signature of methods in the FlightDataOnDemand.java class is the same as the signature of methods defined in the corresponding FlightDataOnDemand_Roo_DataOnDemand.aj AspectJ ITD file. Similarly, you can customize any method of FlightDataOnDemand_Roo_DataOnDemand.aj by writing them in the FlightDataOnDemand.java class.

The following figure shows how Roo removes setNumOfSeats, setOrigin, and setDestination methods from the FlightDataOnDemand_Roo_DataOnDemand.aj file:

![](bbox=[0.292, 0.453, 0.71, 0.675])