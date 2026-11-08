![](bbox=[0.16, 0.12, 0.8, 0.32])

Figure 7.2 – Cloud Build and Terraform (VPC and compute provisioning as separate pipelines)

The majority of examples are the same, except for moving specific resource configurations into their respective vpc and compute folders. In the previous example, it consisted of executing the trio of terraform init, plan, and apply commands once – however, when the resources are separated, the trio of terraform commands will be executed twice, once for each set of resource configurations. Another difference is because they are executed separately, we will need to use another capability available to us in Terraform known as remote state.

remote state allows us to reference provisioned resources from other applied Terraform configurations, so rather than manually leveraging variables to reference a resource, we can retrieve the outputs of a previous Terraform execution to link to created resources. When we create a GCE VM instance, we need access to the subnet that the resource will be associated with.

Note the following output of the gce.tf configuration:

```
resource "google_compute_instance" "packt_instance" {
  name           = var.instance_name
  machine_type   = var.instance_machine_type
  boot_disk {
    initialize_params {
      image = var.instance_image
    }
  }
  network_interface {

```