## Getting Started
1. In IAM follow the security status suggestions for securing your account.
1. Set up new IAM user so that you are not using the root credentials.
    1. Select Create User and enter in the new user ID.
    1. Save the user credentials
    1. Create a user group and add policies like admin or power user.
    1. Add the user to the group.
1. Set up a password policy
1. Go back to the user and set a password.
1. Sign out of the root account and sign back in as the new user.

## Setting up CloudTrail
1. Start logging and select or create an S3 bucket for storing the logs.
1. Optionally turn on SNS notification
1. Next optionally configure the logs.
1. If using SNS, go to SNS and create a subscription.

## Setting up a database
1. Open up RDS
1. Select a database like MySQL community edition.
1. If meant for production select Multi-AZ Deployment which will increase availability/resilency of the application.
1. Choose Instance Class and storage type. General purpose storage gives burstable capabilities of IOPs.
1. Turn off public accessibility unless it needs to be publicly accessible.
1. Set up backup windows and upgrade windows if desired.
1. Deploy the database.

## Setting up EC2
1. Create a IAM role so the EC2 instances can access other needed parts of AWS 
1. Select EC2 role.
    1. Search for S3
    1. Select Read only access
1. Launch a new EC2 instance.
    1. Select an AMI like Ubuntu 14
    1. Select a size (for demo purposes use a micro size)
    1. Ensure an public IP address will be assigned.
    1. Tag the server with a name
    1. Use the default VPC security group (or whichever SG that is set up to work with the RDS)
    1. Create a new key pair if necessary.
    1. Set the permissions of the key with `chmod 600 keypairname.pem`
1. In the default security group add SSH over port 22 from your IP address, and HTTP over port 80 for everywhere.
1. Connect to the EC2 instance.
1. Install the necessary software
    1. `sudo su`
    1. `apt-get update`
    1. `apt-get upgrade`
    1. `apt-get install apache2 php5 php5-mysql php5-curl`
    1. `cd /var/www
    1. `wget https://wordpress.org/latest.zip`
    1. `apt-get install unzip`
    1. `unzip latest.zip`
1. Using a browser go to the url of the EC2 instances to see the default apache page.
1. Create a new load balancer.
    1. Set the ping protocol to TCP.
    1. Select the same security group as earlier.
    1. Add the webserver to the ELB.
    1. Give it a tag
1. In route53 add record sets for the loadbalancer. 
    1. Set the Alias to yes and target the loadbalancer.
    1. Create a record set for both the top level domain and www.
1. Create a new route 53 hosted zone.
    1. Name it internal
    1. Set it to private
    1. Create a record set named mysql.internal and point it to the CNAME of the RDS
1. In the EC2 instance:
    1. Check that it can find the RDS with `nslookup mysql.internal`
    1. `vim wp-config.php`
    1. define the database name, username and password
    1. set the hostname to 'mysql.internal'
    1. save and quit vim
    1. `apt-get install mysql-client`
    1. `mysql -hmysql.internal -uwordpressapp -p`
    1. enter the password to make sure you can connect to the RDS
    1. quit
    1. `cd /etc/apache2/sites-enable`
    1. `vim 000-default.conf`
        1. change the document root to /var/www/wordpress
        1. add `ServerName YourURL.com`
        1. add `ServerAlias www.YourURL.com`
        1. save and quit
    1. `service apache2 reload`
    1. `service apache2 restart`
1. reload the website in the browser to see the changes
1. set up the wordpress site
1. try visiting the route53 name
1. In the settings set up the Website url to yourURL.com

## W3 Total Cache Plugin
1. Google W3 Cache Plugin which helps distribute images and other static files
1. In the EC2 instance:
    1. `cd /var/www/wordpress/wp-content/plugins`
    1. wget the url for the plugin
    1. unzip the download
1. Go to the website -> plugins -> activate the plugin.
1. Manually upload the changes back to the EC2 instance:
    1. cd ~/var/www
    1. `chown -R www-data:www-data`
    1. `cd wordpress`
    1. `vim wp-config.php` and add the lines as directed by the website
1. Setup Https:
    1. `vim .htaccess`
    1. Add the lines as directed by the website.
1. Create a bash script to execute the series of commands as directed:
    1. `vim execute-commands`
    1. Paste in the commands
    1. `chmod +x execute-commands`
    1. `./execute-commands`
    1. `rm execute-commands`
1. `cd ..`
1. `chown www-data:www-data -R wordpress/`
1. In the website -> Performance -> CDN -> change to cloud front
1. In the webstie -> CDN -> it asks for an Access Key and Secret Key. It is okay to place them here because it doesnt end up in the EC2 instance but in the database. 

## Setting up CloudFront
1. Create an S3 bucket to store data for CF
1. Go to CloudFront and start a new distribution.
1. Select Web
1. Select origin Domain name which is the S3 bucket.
1. Add alternate Domain Name -> cdn.domainname.com
1. Enable distribution.
1. Create distribution.
1. In Route53 add a record set for the CDN alias with the alternate domain name.
1. In IAM create a user for wordpressapp with credentials.
1. Add those credentials to the wordpress app.
1. Add policies for S3 Full Access and CloudFront Full Access
1. Add the CloudFront Alternate domain name to Wordpress
1. Add the S3 bucket to Wordpress
1. In prefromance export the files for the CDN
1. Go to the homepage of the wordpress app to ensure that it looks correct still.
1. Ensure in S3 that the bucket all allows everyone to read the files from the bucket.

## Building the AMI
1. Ensure you have a bootstrapping S3 bucket.
1. Temporarily add S3 full access to the IAM Role
1. Connect to EC2 instance.
    1. `sudo apt-get python-pip`
    1. `pip install awscli`
1. In EC2 console -> Actions -> Create Image
1. Set up image name and description.
1. Create image
1. Delete old EC2 instance
1. Create new EC2 instance with IAM Role from AMI
    1. Tag the instance
    1. Use the default security group
1. Connect to the new instance
    1. `sudo su`
    1. `aws configure` set only the region
    1. `aws s3 ls` to make sure you can see your buckets
    1. `cd \var\www`
    1. `aws s3 cp --recursive wordpress/ s3://bucketname
1. Check s3 bucket to make sure the files there.

## Launch Configurations
1. Terminate any existing instances.
1. Go to Launch configurations 
1. Create launch configuration
1. Select your AMI, instance size, and IAM role
1. Set up user data.
   
   `#!/bin/bash`
   
   `aws s3 cp --recursive s3://la-wordpress/ /var/www/wordpress/`
1. Chose default VPC security group
1. Select a keypair for your launch configuration


## Autoscaling group
1. Create Autoscaling group
1. Start with 2 instance.
1. Add all subnets.
1. Use Default VPC
1. In advanced details
    1. Recieve traffic from ELB
    1. Health check type set to EC2
1. In scaling policies
    1. Scale 2 to 5 instances
    1. Create new alarm for avg CPU utilization is >= 80 increase by 1
    1. Create new alarm for <= 60 decrease by 1
1. Tag your group. app:wordpress, env:production
1. Finalize the creation


