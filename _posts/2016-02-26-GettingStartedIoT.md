##Create AWS Infrastructure

###Create a Thing
####What is a Thing?

The process of working with AWS IoT begins with Things.  For simplisiticy A Thing is a Device.  It can be **any** device!  A Thing ties in rules, policies and shadows.

#####What is a Rule
A if this then that engine for IoT.  Rules are interpreted with a SQL like language and can be pushed downstream to:
* S3
* DynamoDb
* Kinesis
* Lambda
* SNS
* SQS
* Other Subscribers


#####What is a Shadow
A Shadow is a versionable JSON representation of the state of a device; enables persistent state.  

#####What are Policies
Policies are just like any other IAM policy.  You are setting permissions on resources assigned to this certificate

#####What are Message
Messages are the way Things and IoT communicate.  
AWS IoT supports WebSockets and MQTT.

MQTT follows a pub/sub model:
* When you Publish to a topic (e.g. sensors/hrm/client)
* Any subscriber to that topic will receive that message
  * __+__ is like a wildcard for one level of the topic heiarchy (e.g. sensors/+/jen123 will receive any message for any sensor on the jen123 device
  * __#__ is like a wildcard for everything under the / (e.g. sensors/# matches any sensor of any client) 

```bash
aws iot create-thing --thing-name <something unique>
```

Should get a response like:
```json
{
    "thingArn": "arn:aws:iot:us-east-1:123456:thing/jasons-yun",
    "thingName": "jasons-yun"
}
```

###Provision a Certificate
```bash
aws iot create-keys-and-certificate --set-as-active --certificate-pem-outfile cert.pem --public-key-outfile publicKey.pem --private-key-outfile privateKey.pem
```

This will create the cert, public and private keys.

Addtionally at the very top of the command's output there will be a certificateArn.  Save this, you will need it to associate the policy with the certificate.

```json
{
  "certificateArn": "arn:aws:iot:us-east-1:123456t:cert/ee123456ff121341241212",
    "certificatePem": "-----BEGIN CERTIFICATE-----\n
```


###Create Policy and Attach to Certificate

####Create Policy File
_I usually save this a file *AllowAllIOTPolicy.json*_

```json
{
 "Version": "2012-10-17",
 "Statement": [{
 "Effect": "Allow",
 "Action":["iot:*"],
 "Resource": ["*"]
 }]
}
```

_The policy name might cause collisions if it already exists_ 
```bash
aws iot create-policy --policy-name "PubSubToAnyTopic" --policy-document file://AllowAllIOTPolicy.json
```

Attach to the certificate that you created in prievious  step

```bash
aws iot attach-principal-policy --principal "arn:aws:iot:us-east-1:123456t:cert/ee123456ff121341241212"  --policy-name "PubSubToAnyTopic"
```

[MQTT Setup](quiver:///notes/2E385283-7834-4225-9299-DDF9BE615C17)