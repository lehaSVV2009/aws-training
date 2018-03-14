# AWS Lambda

AWS Lambda - service allows to execute code without servers.

Supports python, node, java, php, etc.
AWS handles resource allocation (process, memory, etc.)

## How it works

```
Event Source ---------------> Function --------------> Services
     |                           |                         |
Changes in data state           Node                 AWS services
Request to endpoint            Python                   Other
Changes in resource state       Java
                                 C#
                                 Go
```

## Resource allocation

* From 128 MB to 3GB Memory
* CPU and Network resources are automatically allocated according to memory

## Stateless

* Lambda has to be stateless
* It is possible to store state in DB or S3
* No interactions with Lambda infrastructure available (just 500MB in /tmp folder which should not be used)

*Do not use /tmp folder, cause it is not constant, it can be removed*

## Structure

* Handler function - AWS Lambda starter
* Event Object - input parameters
* Context Object - configuration parameters

## Web App with lambda

It is possible to implement web app with lambda only (server-less).

```
                       <-> Authorizer Lambda
Web app -> API Gateway -> Content Service Lambda -> External API
                       -> User Service Lambda -> DynamoDB
```

## Create lambda

Let's do smth in lambda when S3 file is created.

Go to `AWS Lambda` -> `Manage Functions`

## Create function

There are some Lambda examples here.

| Key | Value | Notes
|-----|-------|-----
| Name | lambda-test |
| Runtime | Java 8 |
| Role | Choose an existing role | You can create role with policies (`AWSLambdaBasicExecutionRole`)
| Existing role | lambda_with_s3_access |
| Policy | ... | 

### Configure triggers

`My Lambda -> Configurations -> Click triggers button (e.g. S3)`

| Key | Value | Notes
|-----|-------|-----
| Bucker | my-bucket |

### Function code

`My Lambda -> Configurations -> Click lambda button`

| Key | Value | Notes
|-----|-------|-----
| Code entry type | .zip or .jar |
| Runtime | Java * | 
| Handler | example.HelloWorld::handleRequest |

`com.amazonaws:lambda-java-events`
`com.amazonaws:lambda-java-core`

```
class HelloWorld implement RequestHandler<S3Event, String> {

  AmazonS3 amazonS3;

  HelloWorld {
    this.amazonS3 = AmazonS3ClientBuilder.standard().build();
  }
  
  String handleRequest(S3Event event, Context context) {
    // ..setup
    S3Object object = amazonS3.getObject(bucket, key);

    // Some magic code
    amazonS3.putObject(object.getBucketName(), key + ".zip");
  }
}
```

### Upload JAR file to Lambda

`Click Lambda -> Function Code -> Upload -> Uploading`

### Execute Lambda

`Upload file to S3 -> See lambda monitoring`

### Monitor execution

`My lambda function -> Monitoring`

*Lambda is integrated with CloudWatch, so all the actions can be monitored in logs*

## Settings

* Network - VPC or no VPC
* Memory - e.g. 1024MB
* Timeout - e.g. 15 sec
* Concurrency - e.g. 1000 parallel executions
* Debugging and error handling
* Auditing and compliance

## Test lambda

| Key | Value | Notes
|-----|-------|-----
| Create new event | S3 PUT |

*Be careful with different availability zones, you code might work with a specific zone only*

## Limitations

* Lambda execution time is less than 300 secs
* Supports TCP/IP only
* 1000 parallel executions (can be increased)

## Pricing

???