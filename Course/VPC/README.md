# CHAPTER 7 | VPC


Day of exam and day before you should build your own VPC from memory for successfull passing!

## VPC

### [What's a VPC](https://aws.amazon.com/vpc/)

Think of a VPC as a virtual data centre in the cloud.

Amazon Virtual Private Cloud (Amazon VPC) lets you provision a logically isolated section of the AWS 
Cloud where you can launch AWS resources in a virtual network that you define. You have complete 
control over your virtual networking environment, including selection of your own IP address range, 
creation of subnets, and configuration of route tables and network gateways.

### What are the components of Amazon VPC

* A Virtual Private Cloud: A logically isolated virtual network in the AWS cloud. You define a VPC’s IP address space from ranges you select.
* Subnet: A segment of a VPC’s IP address range where you can place groups of isolated resources.
* Internet Gateway: The Amazon VPC side of a connection to the public Internet. Single one per VPC!
* NAT Gateway: A highly available, managed Network Address Translation (NAT) service for your resources in a private subnet to access the Internet.
* Virtual private gateway: The  VPC side of a VPN connection!
* Peering Connection: A peering connection enables you to route traffic via private IP addresses between two peered VPCs.
* VPC Endpoints: Enables private connectivity to services hosted in AWS, from within your VPC without using an Internet Gateway, 
VPN, Network Address Translation (NAT) devices, or firewall proxies.
* Egress-only Internet Gateway: A stateful gateway to provide egress only access for **IPv6** traffic from the VPC to the Internet.

![VPC_Diagram](https://www.mattbutton.com/images/2017/vpc-with-private-and-public-subnets.png)
![VPC_Diagram](https://neonta.com/wp-content/uploads/2017/01/vpc-diagram.png)

* You can have multiple VPC in a region (default up to 5).
* You can have 1 internet gateway per VPC.
* Multiple Subnets in 1 AZ (not possible same subnet in 2 AZs)
* Security groups are Stateful, instead, Network ACLs are stateless.
* Security groups do not span multiple VPCs

Ex. your public web-servers in one subnet, but sensitive backend components and databases in another subnet.
Additionally you can create hardware VPN to connect your datacenter to VPC.

### Default VPC

* Amazon provides a default VPC to immediately deploy instances, all Subnets in it have a route out to the internet.

### VPC Peering

* You can peer one VPC to another VPC using private IP subnets.
* You can peer VPC's with others AWS accounts as well as same account.

### How to VPC Peering

* Overlapping CIDR Blocks is not supported: You can't connect two VPC's that have the same CIDR.
* Transitive Peering is not supported (if network A peers to B, and B peers to C, then A not able to peer C 
(but able by creating direct link between A and C)



###  NAT Instances and NAT Gateways

NAT Instance - is a single EC2 instance, but Gateway is highly available.
Allows your private subnets to communicate to internet without becoming public.

NAT Instances: when creating disable Source/Destination checks , NAT instance must be in public subnet,
there must be route from privat subnet to NAT instance.

Nat Gateways: redundant inside of AZ, preffered by enterprise , 5Gbps to 45Gbps, no need to patch, no associated with security groups,
automatically assigned IP address; dont foget to update route tbl to point to Nat Gateway;
Suggestion: use separate Nat Gateway in each AZ (to prevent single AZ failure)






### Build Your Own Custom VPC


* when create VPC a default Route Table , NACL and default security group created
* By default subnets are associated with main route table (so if we put route to internet in 
main root table, then every subnet will be public). That's why main route table should be private. 
And separate route table to be public.
* us-east-1a (and other AZ) can be different from us-east-1a in DIFFERENT AWS account


* [VPC and Subnet Sizing](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html#vpc-subnet-basics) 
The first four IP addresses and the last IP address in each subnet CIDR block are not available for you to use 
(5 addresses total), and cannot be assigned to an instance.

* For Nat Instances you have to disable the [Source/Destination Checks](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_NAT_Instance.html#EIP_Disable_SrcDestCheck).
* Use [Nat Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html#nat-gateway-basics) instead of Nat Instances

    ![nat-gateway](https://docs.aws.amazon.com/vpc/latest/userguide/images/nat-gateway-diagram.png)


### NACL
By default new ACL has only DENY rules.
Rules evaluated in order(ID order)
Network ACLs evaluated before Security Groups


* VPC comes with default ACL and by detault it allows all inbound/outbound traffic!
* With ACL its possible to block specific IPs (but imposible with Security Groups)
* ACL can be associated with multiple Subnets (but Subnet with single ACL)
* NACL have inbound and outbound separate rules, each with Allow or Deny
* ACLs stateless (you need to define inbound/outbound rules)



### [Network Access Control Lists vs Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Security.html)

![vpc&NACL](https://docs.aws.amazon.com/vpc/latest/userguide/images/security-diagram.png)

* NACL cannot be deployed in multiple VPCs.
* NACL cannot be attached to multiple subnets, only one at the time.
* Each subnet must be associated with a network ACL, if you don't, default NACL will be used.
* By default when you create one NACL, everything is denied.
* Rules are applied in numerical order (starting from the lowest), so when you should create the first rule having number 100 and add others on incremental of 100
* NACL are **stateless** (opposite of Security Groups)
* Remember to open [ephemaral ports](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html#nacl-ephemeral-ports) on your outbound rules only.
* If you have to block specific IP addresses, use network ACL not Security Groups

### Custom VPC's and ELB's

* Design tip: Remember that ELB needs at least two public AZ's, so when designing your network, remember to create at least two public subnets in two AZ.

### [VPC Flow Logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html)

VPC Flow Logs is a feature that enables you to capture information about the IP traffic going to and
 from network interfaces in your VPC. Flow log data can be published to Amazon CloudWatch Logs and Amazon S3. 
 After you've created a flow log, you can retrieve and view its data in the chosen destination.
* You cannot enable Flow Logs for peered VPC that's not in your accounts
* Instances with Amazon DNS traffic not monitored
* Windows Activation traffic not monitored
* Traffic to metadata/userdata instance not monitored
* DHCP traffic not monitored
* Traffic to the reserved addresses of VPC router not monitored


### Direct Connect
Connects your DC to AWS (stable,reliable and secure connections (contra regular VPN))


### [VPC Enpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints.html)

A VPC endpoint enables you to privately connect your VPC to supported AWS services and VPC endpoint 
services powered by PrivateLink without requiring an internet gateway, NAT device, VPN connection, or AWS 
Direct Connect connection
