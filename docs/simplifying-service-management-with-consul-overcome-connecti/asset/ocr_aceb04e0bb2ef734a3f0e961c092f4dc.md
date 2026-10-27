> **Important note**
> 
> Up until this point in the chapter, most of our discussion has been around creating and sharing configuration parameters throughout our services. The Consul integration with Terraform drastically extends that capability and therefore does warrant additional scrutiny as part of the change control process. Although our work here focuses on the open source versions of Consul and Terraform, an enterprise integration enables the employment of fine-grained policies to specify what automation can change, what it cannot, and what requires an additional level of approval. Furthermore, Terraform Enterprise records state changes throughout the process, providing a history of any automated or approved changes.

Much like the **consul-template**, HashiCorp offers an additional standalone binary application called **Consul-Terraform-Sync**. For the purpose of simplicity, I'll be referring to Consul-Terraform-Sync as CTS moving forward. Utilizing CTS, Consul monitors for changes in the service catalog, and based on those changes, issues a request to Terraform to adjust the infrastructure accordingly:

![](bbox=[0.123, 0.438, 0.845, 0.699])

Figure 7.2 – CTS managed infrastructure

As part of the current deployment, we've created a machine specifically for CTS, with the IP address provided in `terraform output` by the variable `CTS_IP`. If you can't find the output from your Terraform run, no problem. Running `terraform output` will provide you with all of the necessary data again. So, let's grab that IP address and SSH into the CTS machine:

```
```bash
$ ssh -i <username>-consul-key.pem ubuntu@<CTS_IP>
```
```