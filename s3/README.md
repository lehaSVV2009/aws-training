# Simple Storage Service (S3)

Object storage. Looks like file storage, but key-value object (filename - file content).

Like Dropbox, Google Drive, etc.

## Payment

* For storage (GBs)
* For read/update/add/copy/.. operations

## Bucket

Isolated container with objects.

Bucket name | Access | Region | 
--- | ----- | -----
sample | public | EU (Ireland)

## Create bucket

## Name and region

Key | Value | Notes
--- | ----- | -----
Name | SampleUser | 
Region | EU (Ireland) | 

### Set properties

* Version
* Server access logging
* Tags
* Object-level logging
* Default encryption

### Set permissions

Key | Value | Notes
--- | ----- | -----
Manage users | alex - READ, WRITE, etc. | 
Manage public permissions | `Grant public access`| or `Do not grant public access`. Public access files are not so secured..

## Upload file

### Select files

Upload file

### Set permissions

Key | Value | Notes
--- | ----- | -----
Storage class | `Standard` | `Standard-IA` vs `Reduced redundancy`. Default storage vs cheap storage + expensive operations vs remove not needed file
Encryption | `None` | `Amazon S3 Master key`

### Metadata

Key | Value | Notes
--- | ----- | -----
Metadata| x-amz-meta-my-custom-http-header - my-header-value | Add request headers that will be stored if file is uploaded
Tag | my_key - my_value | to search, organize and manage access

## Bucket content

If `grant public access` is selected, you will be able to see the file by `https://s3-eu-west-1.amazonaws.com/my-bucket/my-file.jpg` link.
If unselected, you will get HTTP 403 Access Denied.

## Versioning

`Management` -> `Versioning` -> `Enable` -> `Upload bla.txt` -> `Upload bla.txt again` -> `Select Latest version dropdown`

## Create folder

S3 is an object storage, so folder is not a folder :smile: Just part of path.

## Lifecycle management

* Lifecycle
* Replication
* Analytics
* Metrics
* Inventory (Reports to email)

### Name and scope

Key | Value | Notes
--- | ----- | -----
Rule name | sample |
Filter to limit | |

### Transitions

Key | Value | Notes
--- | ----- | -----
Configure transition | `Current version` or `Previous version` | Make different transitions with current or previous version of object
Add transition | list of transitions | e.g. make object `Standard-IA` dependent on time

### Expiration

Key | Value | Notes
--- | ----- | -----
Expiration | `Current version` or `Previous version` |
Expire current version of object after | 30 | days
Permanently delete previous version of object after | 30 | days
Clean up object delete markers and incomplete multipart uploads | `Clean up expired object delete markers` and `Clean up incomplete multipart uploads` |

## Amazon SDK

`com.amazonaws:aws-java-sdk-s3:1.11.283`

```
AmazonS3 amazonS3 = AmazonS3ClientBuilder
  .standard()
  .withRegion(EU_WEST_1)
  .withCredentials(new AWSStaticCredentialsProvider(new BasicAWSCredentials("my access key", "my secret key (IAM -> ... -> Create secret key)")))
  .build();

  amazonS3.listBuckets().forEach(log::info);

```
