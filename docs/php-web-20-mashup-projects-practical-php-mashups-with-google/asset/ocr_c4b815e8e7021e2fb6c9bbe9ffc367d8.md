+   2. If the data type is an object, it is known as a **complex type** in XSD, and must be declared.
+   3. An **array**, which can be described as a hybrid of the former two.

Let's take a look at some examples of what we will encounter in the `types` element.

# Simple Type

Sometimes, you need to restrict or refine a value of a built-in data type. For example, in a hospital's patient database, it would be ludicrous to have the length of a field called Age to be more than three digits. To add such a restriction in the SOAP world, you would have to define Age here in the types section as a new type.

Simple types must be based on an existing built-in type. They cannot have children or properties like complex types. Generally, a simple type is defined with the `simpleType` element, the name as an attribute, followed by the restriction or definition. If the simple type is a restriction, the built-in data type that it is based on, is defined in the `base` attribute of the `restriction` element.

For example, a restriction for an age can look like this:

```
<xsd:simpleType name="Age">
    <xsd:restriction base="xsd:integer">
        <xsd:totalDigits value="3" />
    </xsd:restriction>
</xsd:simpleType>
```

Children elements of restriction define what is acceptable for the value. totalDigits is used to restrict a value based on the character length. A table of common restrictions follows:

| Restriction | Use | Applicable In |
|---|---|---|
| enumeration | Specifies a list of acceptable values. | All except boolean |
| fractionDigits | Defines the number of decimal places allowed. | Integers |
| length | Defines the exact number of characters allowed. | Strings and all binaries |
| maxExclusive/ maxInclusive | Defines the maximum value allowed. If Exclusive is used, value cannot be equal to the definition. If Inclusive, can be equal to, but not greater than, this definition. | All numeric and dates |
| minLength/ maxLength | Defines the minimum and maximum number of characters or list items allowed. | Strings and all binaries |