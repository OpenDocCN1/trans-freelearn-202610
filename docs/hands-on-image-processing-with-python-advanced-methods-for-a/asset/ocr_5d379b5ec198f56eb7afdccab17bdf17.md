In image contrast enhancement, a pixel’s intensity may not be strictly dark or bright; it may be partially both. Fuzzy logic accommodates this by assigning degrees of membership to different classes, allowing more nuanced processing.

# Contrast enhancement using fuzzy logic with the Mamdani system

In this section, let us explore contrast enhancement using fuzzy logic based on a Mamdani Fuzzy Inference System (FIS). Fuzzy logic simulates human reasoning by enabling partial truth values between 0 and 1. In image enhancement, pixel intensities can be interpreted through fuzzy sets like dark, medium, and bright. Fuzzy rules then map these input sets to output intensity corrections such as darker, enhanced, or brighter.

# Mamdani FIS

The Mamdani system is a widely used fuzzy inference mechanism that uses min-max operations for rule evaluation and aggregation, and centroid defuzzification to obtain crisp output values. The Mamdani FIS consists of four steps:

+   - **Fuzzification:** Convert input (pixel intensity) into degrees of membership. Given a pixel intensity value, fuzzification maps it to degrees of membership based on membership functions (MFs, e.g., triangular or Gaussian):
+   
+     μ_dark(I),    μ_medium(I),    μ_bright(I)
+   - **Rule evaluation:** Use fuzzy rules and logical operations (usually min for AND) to evaluate the outcome. Apply min operation to infer the output fuzzy set from the antecedent:
+   
+     μ_output(z) = max_i [ min( μ_input(I), μ_rule_i(z) ) ]
+   
+     The firing strength of each rule is determined using the min operator, and the final aggregated fuzzy output is computed using the max operator. Let us define the following Fuzzy rules:
+     - R₁: IF intensity is dark THEN output is darker
+     - R₂: IF intensity is medium THEN output is enhanced
+     - R₃: IF intensity is bright THEN output is brighter
+   - **Aggregation:** Combine the outputs of all rules (typically using max)