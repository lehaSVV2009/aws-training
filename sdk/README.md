# Amazon SDK (Java)

Separate gradle dependency per service.
E.g. `compile com.amazonaws:aws-java-sdk-s3:1.11.283` for S3

S3 example:

```
AmazonS3 amazonS3 = AmazonS3ClientBuilder
  .standard()
  .withRegion(EU_WEST_1)
  .withCredentials(new AWSStaticCredentialsProvider(new BasicAWSCredentials("my access key", "my secret key (IAM -> ... -> Create secret key)")))
  .build();

  amazonS3.listBuckets().forEach(log::info);
```

## Credentials

*Do not put credentials in code!!!*

AWS searches credentials from folder, from env variables, etc.

```
$ cd ~/.aws/
$ nano config

[default]
output=....
```

AWS production server credentials solution:

* No need `~/.aws/config` folder.
* No need any environment variables.
* Credentials are auto generated for every start up of service instance (e.g. ec2).
* They are stored inside a service (e.g. inside EC2) and no one has access to it.