# Relational Database Service (RDS)

* Allows easily setup and scale relational databases
* Automate hard administration tasks (e.g. increase memory)

## Supported DB

* Amazon Aurora
* MariaDB
* Oracle
* MySQL
* PostgreSQL
* Microsoft SQL Server

## RDS and VPC

Client <-> Internet Gateway -> VPC -> RDS

## Setup

### Link

`Amazon RDS -> Instances -> Create new DB instance`

### Select engine

* Oracle -> Enterprise/.. Edition
* Microsoft SQL Server -> Enterprise/..
* MYSQL
* Aurora -> Compatible with MySQL/Compatible with PostgreSQL/..
* etc.

### Choose use case

* Production - Amazon Aurora (MySQL compatible low-price)
* Production - MySQL (better performance + multi-az deployment for reliability)
* Dev/Test - MySQL

### Specify DB details

| Key | Value | Notes
|-----|-------|-----
| License model | GPL |
| DB engine version | mysql 5.6.39 | 
| DB instance class | db.m4.xlarge | simple EC2 instance
| Multi-AZ deployment | yes/no |
| Allocated storage | 20GB | 
| DB Instance name | alex-db | 
| Master username | root | 
| Master password | mypassword |
| Confirm password | mypassword |


### Network & Security

`RDS -> My subnet groups`

| Key | Value | Notes
|-----|-------|-----
| Subnet group | alex-default | (or create new VPC)
| Public accessibility | No | or yes

### Database Options

`Amazon RDS -> My subnet groups`

| Key | Value | Notes
|-----|-------|-----
| Port | 3306 | (or create new VPC)
| Encryption | No | or yes
| Backup | 7 days | 
| Monitoring | | 
| Logging |  | Audit log, etc.
| Maintenance | Enable auto minor version upgrade | 

### Launch DB Instance

Click `Launch DB instance` and go to `Amazon RDS -> Instances -> alex-db`.

Instance actions:
* Create read replica
* Promote read replica
* Take snapshot
* Migrate latest snapshot
* Modify
* Stop
* Reboot
* Delete

## Info

| Key | Value | Notes
|-----|-------|-----
| Endpoint | alex.....eu-west-1.rds.amazon.com | 
| Username | root | 

There is a separate read replica.
It means that if any data is written to master, it will be available to read from read replica.

*RDS is not integrated with Elastic Load Balancer* 

## Replica

```
                 L4 or L7 load balancers
                        |
        ------------------------------------
       |                                    |
Delete/Update/Insert                      Select
       |                         --------------------------
       |                        |           |              |
Write replica             Read replica | Read replica | Read replica
```

*Aurora supports better read replica management*

The issue is that `cluster (write) endpoint` and `reader endpoint` are different.