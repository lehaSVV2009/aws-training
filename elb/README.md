# Elastic Load Balancer (ELB)

ELB automatically allocates income traffic between different EC2 instances in different Availability Zones.

```
        |
  Load Balancer
        |
     -------
    |       |
   VPC     VPC
    |       |
   ---     ---
  |   |   |   |
 EC2 EC2 EC2 EC2
```

How it works:

```
                       Load Balancer
                            |
     ----------------------------------------------------
    |                                                    |       
(Rule) Listener                                     (Rule) Listener
    |                                                   ...       
   -----------------------------------------    
  |                                         |   
Target group (health check)     Target group (health check)
  |         |                         |         |         |
Target    Target                    Target    Target    Target
```

## Price

* 0.0225 USD per hour
* It is cheap :smile:

*In a good practice load balancer should be in public net and instances should be in private net*

## Setup

### 1. Create VPC with 2 subnets in 2 availability zones (public)

Key | Value
--- | -----
Network | alex-pub1
VPC | ...
IPv4 CIDR block | 10.0.1.0/24

Key | Value
--- | -----
Network | alex-pub2
VPC | ...
IPv4 CIDR block | 10.0.2.0/24

### 2. Create AMI

*To create your own AMI go to EC2 -> IMAGES -> AMIs -> Actions -> ...*
*It was working practice with building AMI from Jenkins and have separate CI*

### 3. Create EC2 instances with valid security groups


Key | Value | Notes
--- | ----- | -----
HTTP | 80 |
Custom TCP Rule | TCP | 8080

### 4. Create target groups

Load Balancing -> Target Groups -> Create

Key | Value | Notes
--- | ----- | -----
Name | tg1 | 
Health check| `/health | 
Registered targets | instance id-s

### 5. Select load balancer type

* Application Load Balancer (choose)
* Network Load Balancer
* Custom..

### 6. Configure load balancer

Key | Value | Notes
--- | ----- | -----
Name | LB1 | 
Scheme | `internet-facing` | (or internal)
Listeners | `HTTP - 8080` | 
VPC | `vpc-... alex` | 
Availability Zones | `eu-west-1a`, `eu-west-1b` | 

### 7. Configure Route ...

Key | Value | Notes
--- | ----- | -----
existing target group | tg1 | 

### 8. Check Load Balancers

Wait about 3 minutes

Name | State | Notes
--- | ----- | -----
existing target group | active | 

Use ping a couple of times `https://lb1-...eu-west-1.elb.aws-amazon.com`. It will use different IPs :smile:

## Auto Scaling

Auto Scaling - service that helps to have a number of instances

`Min size` - Min number of instances
`Max size` - Max number of instances
`Desired capacity` - preferred number of instances

*Your own AMI is required here*

## Create Launch Configuration

`Create Launch Configuration` -> `Choose Instance Type` -> `Security groups`

Key | Value | Notes
--- | ----- | -----
Name| lc1 |
IAM role | .. | 

Check `EC2` -> `Launch Configuration` -> ..

## Create Auto Scaling Group

### Configure Auto Scaling group details

Choose vpc, subnet

Key | Value | Notes
--- | ----- | -----
Load balancing Receiver traffic from one or more load balancers | true
Network | vpc-.. alex |
... | ... |
Target groups | tg1 | 
Health Check | EC2 | 
Health check Grace Period | 300 |

### Configure Scale Group Size

Use scaling policies to adjust the capacity of the group

Scale between 1 and 10 instances.

Name | Scale Group Size
Metric Type | Application Load Balancer Request Count Per Target
Target value | 2
Instances need | 300

...

### Go to target groups and instances

It will automatically create instance in a special target group


## FAQ

* How to automatically deploy docker image to AWS AMI and update all instances?

