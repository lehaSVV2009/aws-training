# Other popular AWS services

## Cloud Formation

Allows to store and use templates.
No need to manage AWS services manually.

## Amazon Lightsail

There is an analogue of EC2 with simple configurations

It has nice UI. Prices equal to Digital Ocean machines.

*One month free*

* You can create snapshots
* You can add storage
* You can add load balancers

Note - they can go to S3 or smth like that.

## Terraform (non-AWS)

Infrastructure as code. Can be used with AWS to orchestrate infrastructure.

## Amazon Glacier

Secured and reliable service for data archiving

* 0.004 USD per month for GB
* It is not fast
* Price depends on chosen method (from 5 minutes to several days)

## Elastic Transcode Service

Service for video transcoding.

When the video is uploaded, video format/coding is usually unknown.
ETS allows to customize video params (codec/ratio/sizing/...) to be normally shown.

It has integration with Simple Notification Service.
e.g. `On Error Event -> use SNS topic`

## Amazon MQ

Managed message broker service for Apache ActiveMQ. (ActiveMQ in Cloud).