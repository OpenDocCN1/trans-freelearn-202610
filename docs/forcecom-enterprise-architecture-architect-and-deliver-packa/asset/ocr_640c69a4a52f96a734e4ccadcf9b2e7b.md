```
// Check the results
System.assertEquals(
    '4.1', e.failures[0].complianceCode);
System.assertEquals(
    'Driver must have a FIA Super License.',
    e.failures[0].failureReason);
}
}
}
```

The namespace used in this sample is fforce. Your namespace will differ. Replace the namespace to get the code sample to compile (you can also find this class in the sample code for this chapter).

Run the test to confirm whether the API is working as expected.

As you have used the same Service layer method as used by the rest of the application, your existing unit and integration tests around this class should suffice in providing you confidence in the robustness of this as an API to your application. If not, improving your testing around your Service layer benefits both external and internal callers equally.

Notice that when you click on the Show Dependencies button in the ApplicationAPITest class, it shows the API and Custom Objects being referenced from the package:

| Show Dependencies |
| :--- |
| Dependency Information for Apex Class ApplicationAPITest |
| << Back to Apex Class: ApplicationAPITest |
| A dependency is created when one component references another component, permission, or preferer below have dependencies. In addition, the object-level operational scope, that is, the data manipulatio also listed. To see field-level detail of the operational scope, click Fields next to name of an object. |
| Object Operational Scope |
| Driver (Installed Package: FormulaForce) | [Fields] |
| Apex Class, Trigger and Page References |
| ComplianceService (Installed Package: FormulaForce) |
| SeasonService (Installed Package: FormulaForce) |