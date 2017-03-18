# Peering
Peering allows two VPCs in the same region to communicate with each other as if they were one VPC. At a minimum you need two VPCs with one subnet in each and a resource to be shared like an EC2 instance. Note that the CIDR ranges for the two VPCs cannot overlap. So if the first VPC has a CIDR range of 10.0.0.0/16 the second could be something like 10.1.0.0/24 or 172.0.0.0/24

1. If you do not already have a second VPC create one.
1. Create an internet gateway for that VPC.
1. Create a subnet and attach the internet gateway to the subnet.
1. Ensure both VPCs have an EC2 instance
1. From the VPC page select Peering Connections.
1. Create a new Peering Connection.
   1. Give it a descriptive name that explains what is being connected or why the connection exists
   1. Select the first VPC
   1. Select the second VPC (it can also be from another account)
1. Go to the second VPC and accept the request from the first VPC.
1. Update both route tables with a route to the IP address of the other VPC and set the target as the Peering Connections.
1. Log into one of the EC2 instances and then from there try to log into the other EC2 instance using the private IP address.
