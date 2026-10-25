object types. Running these tests together results in the following output:

# Tests:(Total Failed : 1)
(Total Passed: 3)

+   • Test:
+   ShapeTests/test_Point
+   (failed: 0) (passed: 2)

+   • Test:
+   ShapeTests/test_IRectangle
+   (failed: 1) (passed: 1)

# Failed

| Test Name | Test Description | Message |
| test_SquareResize | Test resizing of Squares | areIdentical failed when passed expected: {number} "9" and actual:{number} "3" Should return 9 |

# Passed

| Test Name | Test Description | Message |
| test_RectangleResize | Test resizing of Rectangle | OK |

The square test has failed. The interface was implemented so our code was able to compile; however, this has exposed a problem with our assumption about squares. Through our implementation, we forced the behavior of a square onto an IRectangle type. This test exposed the fact that we broke the Liskov substitution principle that we discussed in Chapter 4, Object-oriented Programming with TypeScript. We are not able to safely substitute one IRectangle object for another, which could result in unexpected behavior.