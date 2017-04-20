The purpose of this is to demonstrate how to set up an application to fall back gracefully in case the app fails.

Steps:
1. Launch a web server EC2 instance
  1. yum install httpd
  1. apachectl start
1. set up an ELB that will serve traffic from the web server
1. Set up a cloud front distrobution with an alternate domain name pointing to the domain of your application. Select the origin to be the s3 bucket that contains your fall back web page.
1. In Route 53 create a record set (or modify an existing one) to point the URL to the alias target of the ELB. Switch the routing policy to failover. The ELB is primary and evaluate health must be active.
1. Create a second record set to point to the same url. Point to the alias of the Cloudfront alias. Select Failover and set it to secondary. It should not have a health check.
1. Test both the domain, ELB and CF urls to make sure everything is working.
1. Delete the ELB to make the health check fail and failover the website. Check the domain to make sure it fails over (check it in Incognito if it was cached).
