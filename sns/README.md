# Simple Notification Service (SNS)

Analogue of [Mail chimp](https://mailchimp.com/)

## SNS Dashboard

### Create topic

`SNS -> Topics`

Key | Value | Notes
--- | ----- | -----
Name | My topic | 

### Create subscription

`SNS -> Subscriptions`

Key | Value | Notes
--- | ----- | -----
Protocol | EMAIL | `HTTP`/`EMAIL`/`SMS`/`AWS Lambda`/...
Endpoint  | mymail@gmail.com |

### Confirm subscription

Confirm subscription in your email.

*It requires not user-friendly email confirmation, do not use with customers*

### Publish to topic

`Topics -> Actions -> Publish -> Input text -> Publish`

## S3 with SNS integration

### Allow S3 to send events

`SNS -> Topics -> Update Policy`

```
Condition: {
  "ArnLike": {
    "aws:SourceArn": "arn:aws:s3:*:*:my-s3-bucket"
  }
}
```

### Send event from S3

`S3 -> my-s3-bucket -> Management -> Event`

### Upload file to S3

You will see that topic is processed and 
