## EC2

### [What's EC2](https://aws.amazon.com/ec2/)

Amazon Elastic Compute Cloud (Amazon EC2) is a web service that provides secure, resizable compute capacity in the cloud. It is designed to make web-scale cloud computing easier for developers.

### [EC2 Options](https://aws.amazon.com/ec2/pricing/)

* [On demand](https://aws.amazon.com/ec2/pricing/on-demand/): You pay for computing capacity by per hour or per second depending on which instances you run.
* [Reserved Instance (RI)](https://aws.amazon.com/ec2/pricing/reserved-instances/): Provide a significant discount (up to 75%) compared to On-Demand pricing and provide a capacity reservation when used in a specific Availability Zone. You have to enter a contract.
* [Spot](https://aws.amazon.com/ec2/spot/): Amazon EC2 Spot instances allow you to request spare Amazon EC2 computing capacity for up to 90% off the On-Demand price.

  * If you terminate an instance, you will pay for the complete hour.
  * If Amazon terminates the instance, you won't pay for the complete hour.
  * The following are the possible reasons that Amazon EC2 might [interrupt your Spot Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-interruptions.html):
    * Price – The Spot price is greater than your maximum price.
    * Capacity – If there are not enough unused EC2 instances to meet the demand for Spot Instances, Amazon EC2 interrupts Spot Instances. The order in which the instances are interrupted is determined by Amazon EC2.
    * Constraints – If your request includes a constraint such as a launch group or an Availability Zone group, these Spot Instances are terminated as a group when the constraint can no longer be met.

* [Dedicated Hosts](https://aws.amazon.com/ec2/dedicated-hosts/): Is a physical server with EC2 instance capacity fully dedicated to your use(often for goverment regulations).

### Launch an EC2 Instance - Lab

* Termination protection is turned off by default.
* The EBS root volume by default is deleted at termination.
* Default AMI's (provided by Amazon) cannot be encrypted.
* Additional volumes can be encrypted.

## Security groups - Lab

### [What's a security group](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)

A security group acts as a virtual firewall for your instance to control inbound and outbound traffic.

* All Inbound traffic is Blocked by Default.
* All Outbound traffic is Blocked by Default.
* All security groups changes are applied immediately(exam topic)
* Security groups are stateful. For example, if you allow the request to come in, automatically responses can go out even if you don't have anything on the outbound section of your security group.
* You can specify only allow rules, not deny rules.

### [What's EBS (Elastic Block Store)](https://aws.amazon.com/ebs/)

Provides persistent block storage volumes for use with Amazon EC2 instances in the AWS Cloud. EBS is automatically replicated in a specific AZ

Volume types: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumeTypes.html

#### SSD

* General Purpose SSD (gp2): For most workloads. 1GB-16TB, up to 16k IOPS
* Provisioned IOPS SSD (io1): Designed for IO intensive(databases), 4GB-16TB, 16k-64k IOPS

#### HDD (Magnetic)

Are great for sequential access (processing log files, bigdata work flows as an example).

* Throughput Optimized HDD (st1): Magnetic disk, this can't be a root(boot) volume. 500 IOPS
* Cold HDD (sc1): Lower cost storage, like file servers, can't be a boot volume. 250 IOPS
* Magnetic (standard): Lowest cost per gigabyte of all EBS. It's bootable and it's from the previous storage generation. 40-200 IOPS

## EBS Volumes & Encrypt Root Device Volume - Lab

* Instances and Volumes MUST be in the same AZ (exam)
* Volume Snapshots exist in S3.
* Snapshots are incremental (only differences are saved)
* Snapshot of root volumes require the instance to be stopped.
* _You can't delete a snapshot of an EBS volume that is used as the root device of a registered AMI._
* You can change EBS volumes size and type on the fly. 
* To move an EC2 volume from one AZ/Region to another, take a snap then image(AMI) of it, then you can launch AMI in new AZ/copy AMI to the new Region.
* Snapshots of encrypted volumes are encrypted automatically
* Volumes restored from an encrypted snapshot will be encrypted as well.
* You can share snapshots only if they are not encrypted, these snapshots can be made public.

### [AMI Types](https://aws.amazon.com/amazon-linux-ami/instance-type-matrix/)

* [EBS](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html): Amazon EBS provides durable, block-level storage volumes that you can attach to a running instance
  * EBS takes less time to provision.
  * EBS volumes can be kept once the instance is terminated.
* [Instance Store / Ephemeral storage](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html): This storage is located on disks that are physically attached to the host computer

  * Instance Store Volumes can't be stopped, if the host fails, you lose data.
  * You can reboot the instance without losing data.
  * You can not detach Instance Store Volumes.
  * Instance store volumes cannot be kept once the instance is terminated.

### CloudWatch - Lab

* Standard monitoring is 5 min and * Detailed monitoring is 1 min (you will be charged for it)
* Alarms can be set to notify when a specific threshold is hit
* Events can be used to perform actions when state changes happen in your AWS resources.
* Logs can be aggregated in a single place to better troubleshoot. Remember that you need to install an agent on the EC2 instance.
* By default, Matrics on EC2 instances are: CPU related, Disk related, Network related and Status check related.

Remember that:

* CloudWatch: is for monitoring performance
* CloudTrail: is for auditing AWS configurations itself

### IAM Roles with EC2 - Lab

* Roles avoid you to store credentials inside EC2 instances in order to communicate with other AWS services
* Roles are universal, you can use them in any region

### [EC2 Instance Metadata](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html)

Within your local instance run:
 ```http://169.254.169.254/latest/meta-data/```
 ```http://169.254.169.254/latest/user-data/```

If you want to access a specific item, simply add it at the end of your request.
There are lots of useful info , which ex. can be exported and stored in DB on bootstrap

### [Placement Groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/placement-groups.html)

You can launch or start instances in a placement group, which determines how instances are placed on the underlying hardware. When you create a placement group, you specify one of the following strategies for the group:

* Cluster – clusters instances into a low-latency group in a single AZ.
* Spread – spreads instances across underlying hardware and can spread in multiple Availability Zones.
//TODO to be removed
* Partition – spreads instances across logical partitions, ensuring that instances in one partition do not share underlying hardware with instances in other partitions.

Some notes about placement groups:

* The name you specify for a placement group must be unique within your AWS account.
* Only specific types of instances can be launched in a placement group.
* You can't merge placement groups.
* You can't move an existing instance into a placement group 
* If the exam refers to placement groups, without mentioning which type, it's most probably talking about the Cluster ones since those are the old ones.

## [EFS](https://aws.amazon.com/efs/)

Amazon EFS provides scalable file storage for use with Amazon EC2. You can create an EFS file system and configure your instances to mount the file system
Diff from EBS that EFS can be assigned to multiple EC2

* Supports NFSv4
* You only pay for the storage you use
* Scale up to petabytes
* Can support thousands of concurrent connections
* Data is stored across multiple AZ 
* Read after write consistency

* You need to make sure that the EC2 instance that needs to connect with the EFS volume, is associated with the same security group you have on the EFS volume.
* You can assign permissions at the file level and at the folder level.

