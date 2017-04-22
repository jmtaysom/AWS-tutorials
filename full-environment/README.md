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
