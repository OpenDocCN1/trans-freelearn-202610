
## User Experience and Usability Testing

The objective of this testing is to ensure the following:

- Proper consistency in the user interface
- Adherence to Windows user interface guidelines
- Adherence to Dynamics AX UI guidelines

The following table summarizes the specifications for this testing.

### Specifications for User Experience and Usability Testing

|  |  |
|---|---|
| Technique | Manual verification by test engineer. A generalized test pack (GUI compliance test document) should be used to perform this testing. Issues encountered are reported in the bug tool. |
| Expected Results | Proper consistency is observed along with compliance to Dynamics AX user interface standards. |
| Completion Criteria | All forms in the code under test have been verified as per Microsoft Dynamics AX GUI standards. |
| Special Considerations | This testing must be conducted on an SVGA display with 1024*768 pixels. |
| Responsibility | Tester. |
| IBI Reference | 5.1 |

## Verification of Business Intelligence or Reporting Standard

The objective of this testing is to verify adherence to reporting guidelines prescribed in the Dynamics AX Developers Handbook, for e.g.:

- Use of Dynamics AX report wizard
- Use of SQL server reporting service as per implementation guidelines

The following table summarizes this testing

### Specifications for Verification of Business Intelligence/Reporting Standards

|  |  |
|---|---|
| Technique | Manual verification through code review. |
| Expected Results | Compliance to Reporting guidelines mentioned in the Dynamics AX Developers Guide. |
| Completion Criteria | All reports have been verified in the code under test. Issues encountered are reported in the review and authorization form. |
| Special Considerations | The purpose of the report should be considered. If the report under testing is to be used for some regulatory purpose and should be in the same format as issued by a regulatory authority, an exception can be documented. |
| Responsibility | Lead Developer. |
| IBI Reference | 6.1 |

## Performance Testing

The objective of this testing is to verify that there is no significant performance degradation of core Dynamics AX processes with installation of the application under test. Performance is not expected to degrade more than 5%. The following table summarizes this testing.

### Specifications for Performance Testing

|  |  |
|---|---|
| Technique | Dynamics AX benchmark toolkit should be used for performance testing. The performance test for standard Dynamics AX should be executed to baseline the performance. The same tests should be run again with application under test and the results should be compared against the baseline values. The Dynamics AX Event log should be checked for error and warnings regarding the performance. Test scripts should be written for the business processes to be tested. The business processes that are most affected by customization and are under the main business flow of the application should be selected. Issues found are recorded in the review and authorization form. |
| Expected Results | No significant (>5%) performance degradation when running performance benchmark tool before and after installing this solution onto core Dynamics AX. |
| Completion Criteria | Performance degradation is not greater than 5%. |
| Special Considerations | Application testing demo data must be planned and reviewed before starting Performance Testing. |
| Responsibility | Tester. |
| IBI Reference | 7.1 |

This testing will be applicable only in countries where the benchmark kit for Dynamics AX has been released by Microsoft.

## Security Testing

Any software application cannot be useful until and unless it is secure. As per Microsoft vision, IT applications should be as safe as water and electrical devices in our homes. The following should be included in the objectives of security testing for Dynamics AX implementations:

- To ensure that Application Under Testing (AUT) does not bypass standard security model
- To ensure that the system is robust enough to deal with security threats
- Analysis of threat modeling, security measures, etc.
- Final code review to ensure that no critical security is left unresolved

The following table summarizes this testing.

### Specifications for Security Testing

|  |  |
|---|---|
| Technique | The following verifications should be performed as part of this testing: In **Administration | Setup | Security** menu, AUT must have a node available in the tree view listing of modules. Setting no access to a user group should not allow seeing any part of the functionality, forms, or menu item related to the AUT. A Security test plan should be written to define the scope of testing, defect classification, root-cause classification, etc. A security checklist should be provided to the Tester, which should be filled while testing. Test results should be recorded in an Excel sheet, which will also contain bug classification and root-cause classification. |
| Expected Results | - The access control security works as in the standard Dynamics AX product. - The counter measure is found for every security threat identified in threat modeling. |
| Completion Criteria | - Verification of node and tree structure. - Verification of removal of AUT from security menu. |
| Special Considerations | The accessing permission to various groups should be checked with Dynamics AX unique challenges; e.g. if access permission is set to NO, all menu items, buttons, forms, etc. should be invisible. |
| Responsibility | Tester. |
| IBI Reference | 8.1 |

## Authorization Testing

Authorization testing is a type of security testing. This testing is very important and is required to supplement the security testing to achieve the objective of trustworthiness. This testing ensures that user permissions are enforced as per the following configurations in Dynamics AX:

- User group
- Company
- Configuration codes and security keys
- License codes

The following table summarizes authorization testing.

### Specifications for Authorization Testing

|  |  |
|---|---|
| Technique | The following scenarios should be tested: - A security key set to **View level** allows the user to view records. - A top-level security key is disabled; the user should not have access to that item. - A security key set to **Create level** allows the user to create new records. - A feature is disabled when the configuration key is turned off. - A form control is invisible when its security access is set to **No Access**. - A field inside a table does not appear on a form if it is set to **No Access**. - Users cannot see the forms to which they do not have access. - Users cannot see the reports to which they do not have access. |
| Expected Result | A combination of various means of controlling permissions should behave in the way a standard Dynamics AX does. |
| Completion Criteria | All scenarios are executed. |
| Special Considerations | The following special considerations must be taken care while designing test scenarios: - All conditions (i.e. **No Access**, **Edit**, **Create**, **View**, **Full Control**) must be checked for users with limited permissions. - Users belonging to more than one user group (with conflicting permissions) must be tested. - Users belonging to multiple companies may have different permissions in different companies. This must be a special consideration while designing test scenarios. - The Dynamics AX system has limited license codes, such as a user with administrator privileges may not have access to AR or AP as per the license provided. |
| Responsibility | Business Analysts. |
| IBI Reference | 8.6 |

## Globalization Testing

The objective of this testing includes the verification of adherence to rules to function in any country without translation (i.e. proper handling of date/times, currency, string formats) and the ability to run on different (supported) language versions of Windows. It should also verify if the Dynamics AX layer conventions are followed or not. The following table summarizes this testing.

### Specifications for Globalization Testing

|  |  |
|---|---|
| Technique | Dynamics AX best-practice tool should not show any label file-related errors or any other errors that indicate that date/time, currency, or string formats are hard coded. |
| Expected Results | - Ensure the application can function in any culture/locale. - The system should run on different (supported) language versions of Windows. - Adhere to Dynamics AX globalization conventions (i.e. by layer). |
| Completion Criteria | Dynamics AX best-practice tool does not show any error related to globalization. |
| Special Considerations | The first check point should be covered during the compliance check of Dynamics AX best practices. |
| Responsibility | Tester. |
| IBI Reference | 10.1, 10.2, 10.3 |

## Localization Testing

The following should be the objectives of localization testing for Dynamics AX implementations:

- Configuration keys to enable and disable appropriate country functionality based on country settings
- Manual verification to check that all country-specific localization is in the DIS layer
- Inspecting the Dynamics AX Event log for errors indicating hard-coded strings in the code when executing the Dynamics AX best-practice tool
