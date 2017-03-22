Create a launch configuration
1. Select an AMI - probably a custom AMI for the app
1. Select the instance size
1. Name the launch config (a/b)
1. Select other settings as necessary

Create an Auto Scaling Group
1. Select a minimum size (2 at least for high availability)
1. Select VPC and subnets to be used
1. In the Advanced Details, select recieve traffic from ELB, ELB health check and 300 second grace period.
1. Next configure scaling properties with a min and a max number of instances and rules for increasing and decreasing the application
1. Associate a Route 53 route with the DNS name of the ELB.

