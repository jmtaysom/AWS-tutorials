How to serve a website from a private subnet in AWS using a load balancer.

- [ ] Create a VPC
- [ ] Use a CIDR block range of 10.0.0.0/16
- [ ] Create a subnet named public with ip of 10.0.0.0/24 in an availability zone
- [ ] Create a subnet named public with ip of 10.0.1.0/24 in a second AZ
- [ ] Create a subnet named private with ip of 10.0.2.0/24 in the first AZ
- [ ] Create an internet gateway
- [ ] Attach it to the VPC
- [ ] In route tables add a route with a destination of 0.0.0.0/0 and target of the new internet gateway

At this point all the subnets are public even though one is named private since there is only one route table and they are all associated with it.

- [ ] Create a new EC2 instance and launch it in the private subnet. Enable auto-assign IP. Create a new security group with a HTTP rule.
- [ ] Create a new load balancer in the VPC. Select both public subnets. Use the same security group. For the Health Check set ping protocol to TCP and port 22. 
- [ ] Connect to the EC2 instance.
- [ ] Run the following commands.
* sudo su
* yum install httpd
* service httpd start
- [ ] Navigate to the public IP address of the EC2 instance to see the webserver test page
- [ ] Navigate to the CNAME of the load balancer to see the same test page 
- [ ] Create a new route table named private-RT.
- [ ] On the private subnet change the route table to the new private-RT
- [ ] Navigate to the public IP address to see that the subnet can no longer be viewed.
- [ ] Navigate to the load balancer CNAME to see that the test page is still viewable.
