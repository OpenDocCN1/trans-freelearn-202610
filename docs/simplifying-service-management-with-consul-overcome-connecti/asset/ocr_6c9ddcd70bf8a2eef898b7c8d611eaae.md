```
}
}
resource aws_internet_gateway "consul-demo" {
  vpc_id = aws_vpc.consul-demo.id

  tags = {
    Name = "${var.owner}-internet-gateway"
  }
}
resource aws_route_table "consul-demo" {
  vpc_id = aws_vpc.consul-demo.id
  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.consul-demo.id
  }
}
resource aws_route_table_association "consul-demo" {
  subnet_id     = aws_subnet.consul-demo.id
  route_table_id = aws_route_table.consul-demo.id
}
```

OK—this part of the networking is not something to gloss over. AWS security groups identify which ports are open between which devices. Here is a list of the defined ports and their purpose:

+   - Port 22—SSH access.
+   - Ports 8300-8302—These cover all consensus, RPC, and gossip communication between the nodes.
+   - Port 8500—This is used for the Consul web user interface (UI).
+   - Port 8600—Consul utilizes port 8600 for Domain Name System (DNS) queries as part of the Service Discovery function.

These ports have all been addressed and configured within the security group configuration. Here's the code illustrating this:

```
resource aws_security_group "consul-demo" {
  name   = "${var.owner}-security-group"
  vpc_id = aws_vpc.consul-demo.id
```