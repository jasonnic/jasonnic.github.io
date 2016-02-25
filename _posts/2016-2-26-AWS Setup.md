---
layout: post
title: Getting Started QuickSheet 
---

# IAM User Creation

#Getting Started with the CLI

This step starts with you having a credentials with the correct IAM permissions in IoT

##Set up AWS CLI
Personally I see no reason to repeat what AWS has put out for us:
###PIP

[PIP Based Installation](http://docs.aws.amazon.com/cli/latest/userguide/installing.html#install-with-pip)

###If you prefer something other than pip

[Installing AWS CLI](http://docs.aws.amazon.com/cli/latest/userguide/installing.html#install-bundle-other-os)

###Configure

```bash 
aws configure
```

* Enter Access Key - From credentials file
* Secret - From credentials file
* region - Likely us-east-1
* output format - really your preference

_I ususally work with several different profiles, if this is going to be the case I would suggest modifying the credentials file_
```text
[default]
aws_access_key_id = 123456
aws_secret_access_key = 123456

[rdi]
aws_access_key_id = 123456
aws_secret_access_key = 123456
region = us-east-1

[rdi-govcloud]
AWS_ACCESS_KEY_ID = 123456
AWS_SECRET_ACCESS_KEY = 123456
region = us-gov-west-1
```

_Rather than constantly specifying the profile with the_ `--profile enterprise` _I use _ `export AWS_DEFAULT_PROFILE=rdi` _ to set for the duration of my cli usage_

_There have been several modifications to the AWS CLI recently, if you've installed in the past you might need to reinstall in order to use all of the IoT functionality._