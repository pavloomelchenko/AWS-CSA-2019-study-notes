
###ELBs:
Application (Lvl 7 HTTP/S) - for intelligent Routing

Network (Lvl 4 TCP ) - for fastest performance 

Classic ELB - cheapest one, wihtout intelligence
  If you need IPv4 of user in Classic ELB - look for X-Forwarded-For header 

504 Error (Gateway timed out) - application not responding within Idle Timeout period

Instances monitored by ELB either status: InService or OutService.
Instance states are base on Health Checks .
Load Balancers have DNS name, you never given IP address


Do read ELB AWS FAQ  (10 questions in exam on ELB)



###Advanced ELB Theory
Sticky sessions  - enable your users to stick to the same EC2
instance . Can be useful if you are storing information locally to 
that instance 

Cross-Zone load balancing enables you to use ELB across multiple AZ.

Path Patterns allow you to direct traffic to different EC2 instances based 
URL of request

###Autoscaling Groups
YOu create Launch Configuration and then Autoscaling Groups. You can pick
min instance number, max instance number, parameter(like CPU utilization)
to increase number of instances, etc. 

###HA Architecture
Good example of HA Architecture: 2 regions, 2 AZ in each region, AutoScaling Group in each region, 2 subnets
in each AZ (Public and Private ), 1 ELB(cross zone) in each region , Health
Check on Route53

Avail parameters for autoscaling: Average CPU Utilization, Application
Load Balancer requests per target, Average Network In, Average Network Out.

You should know the difference between Multi-AZ and Read Replicas in RDS.
Should know difference between scaling out and scaling up.
Scaling out - use Autoscaling groups(Horizontal scaling ),
Scaling up - increase resources of EC2(Vertical scaling).
Read questions carefully and always consider cost element.
Should know different S3 storage classes(S3 Standard ,S3 Infrequent Access - 
highly available, Reduced Redundancy Storage or S3 Single AZ - not 
highly available)



###CloudFormation - just need to understand what it is
Way to script everything (create templates, use AMIs, create complete much complex setups) 
###Elastic Beanstalk - just need to understand what it is 

 


