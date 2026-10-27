```
```hcl
}
provisioner "shell" {
  inline = [
    "sleep 30",
    "sudo apt-get update",
    "sudo apt install unzip -y",
    "sudo apt install default-jre -y",
    "curl -k -O \"https://releases.hashicorp.com/consul/${var.consul_version}/consul_${var.consul_version}_linux_amd64.zip\"",
    "unzip consul_${var.consul_version}_linux_amd64.zip",
    "sudo mv consul /usr/local/bin"
  ]
}
provisioner "shell"{
  inline = [
    "sudo /usr/local/bin/consul -autocomplete-install",
    "sudo useradd --system --home /etc/consul/consul.d --shell /bin/false consul",
    "sudo mkdir /etc/consul /etc/consul/consul.d /etc/consul/logs /var/lib/consul/ /var/run/consul/",
    "sudo chown -R consul:consul /etc/consul /var/lib/consul/ /var/run/consul/",
    "sudo chmod -R a+r /etc/consul/logs/",
    "sudo mv /tmp/consul.service /etc/systemd/system/consul.service",
    "sudo mv /tmp/consul-common.hcl /etc/consul/consul.d/consul-common.hcl"
  ]
}
}
```
```