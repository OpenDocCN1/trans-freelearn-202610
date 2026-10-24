# # Formatting and validating the configuration

After learning how to destroy resources with Terraform, it is also important to emphasize
the importance of having well-formatted code that meets Terraform’s style rules and to
validate that the code does not contain syntax or variable errors.

# # Formatting the code

Terraform has a command that allows the code to be properly aligned with Terraform’s
styles and conventions.

The following command automatically formats the code:

```
```
terraform fmt
```
```

The following is a screenshot of a Terraform-arranged file:

![](bbox=[0.1348, 0.5594, 0.8652, 0.6352])

The command reformats the code and indicates the list of arranged files.

> > **Important Note**
> > For more information regarding the Terraform style guide, please refer
> > to https://www.terraform.io/docs/configuration/
> > style.html. Additionally, for information about the terraform fmt
> > command line, please refer to https://www.terraform.io/docs/
> > commands/fmt.html.