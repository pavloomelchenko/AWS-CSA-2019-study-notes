## [What's IAM](https://aws.amazon.com/iam/)

Allows you to manage users and their level of access to the AWS console.

## IAM Consist of the following

* Users.
* Groups: Collection of users.
* Policies: Are basically the Permissions. Are made up of Documents, formatted in JSON.
* Roles: Is a way to allow one part of AWS to another part.

## Features

* Centralized control of your AWS account.
* Shared Access to your AWS account.
* Gives you granular Permission.
* Does Identity Federation (Active directory, Facebook, etc)
* MultiFactor Authentication.
* Provides temporary access for users/devices etc.
* Allows you to set up your own password rotation policy.
* Supports PCI DSS compliance.
* integrated with a lot of AWS services.

## [From Console](https://aws.amazon.com/console/)

IAM users sign-in link:
You can customize the URL used by users to sign-in

## New Users and account set up

* IAM is universal, the region is on a global level.
* Root account: Is the user you used to register on AWS, no one should use it to log-in.
* Always enable MFA on your Root account.
* New Users have No permissions once created.
* Access keys ID & Secret are not the same as password.
* You can create and customize your password rotation policies.

## [Billing Alarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/monitor_estimated_charges_with_cloudwatch.html)

You can set a billing alarm


## S3

_[!!! Read the S3 FAQs before the exam !!!](https://aws.amazon.com/s3/faqs/)_

### [What's S3](https://aws.amazon.com/s3/)

* Object-based storage: you can save only object, you can't, for example, install an OS (In this case you need block-based storage).
* Files can save from 0 Bytes to 5 TB.
* No storage Limits.
* Files are stored in Buckets (a folder in a cloud).
* S3 buckets in a universal namespace, the name must be unique globally. So you **cannot** have the same name as someone else.
* Sample of an S3 URL: ```https://s3-eu-west-1.amazonaws.com/yourbucket```.
* When you upload an object in S3 you get an HTTP 200 OK code back.

### Data Consistency for S3

* It's consistent in reads after a write on new objects(PUT).
* It's eventually consistent for overwriting and deletes(PUTS and DELETES) (this means it can take some time to propagate)
* S3 is spread across multiple AZ's

### Components

* S3 Objects consist of:
  * Key (name of object)
  * Value (binary data)
  * Version ID
  * Metadata
  * Subresources:
    * Access Control List
    * Torrent (Not an exam topic)

### Basics

* S3 SLA: 99.9% availability
* S3 is built for 99.99%
* S3 guarantees 11x9s (99.999999999) durability for objects.
* Tiered Storage (classes) available
* Lifecycle management (move between Tiers or e.g. Glacier)
* Versioning
* Supports [multi-part upload](https://docs.aws.amazon.com/AmazonS3/latest/dev/mpuoverview.html)
* Encryption
* Access control (permissions on single files) and bucket policies (permissions on buckets)
* MFA option (multi-factor auth) delete from S3

### [S3 Storage Tiers](https://aws.amazon.com/s3/storage-classes/)

* S3 standard: 99.99% availability 11x9s durability (it sustains the loss of 2 facilities concurrently)
* S3 IA: (Infrequently Accessed): For data that is accessed less frequently, but needs rapid access. You are charged a retrieval fee
* S3 One Zone IA: Like S3 IA but data is stored only in one AZ
* Glacier: Most cheap, used for archival only.
  * Expedited: few minutes for retrieval
  * Standard: 3-5 hours for retrieval
  * Bulk: 5-12 hours for retrieval (Deep Archive)
  * It encrypts data by default
  * Regionally availability
  * Designed with 11x9s durability, like S3

### [Charges](https://aws.amazon.com/s3/pricing/)

S3 is charged for:

* Storage
* Requests
* Storage management pricing (Tiers)
* Data Transfer Pricing
* Transfer acceleration (it's using CloudFront the AWS CDN) using edge locations
* Cross Region Replication Pricing

### [Server side Encryption and ACL](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingServerSideEncryption.html)

* AES-256 Use Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3)
* AWS-KMS Use Server-Side Encryption with AWS KMS-Managed Keys (SSE-KMS)
* Server-Side Encryption with Customer-Provided Keys (SSE-C)

* Control access to the bucket using bucket ACL or Bucket policies
* By default all buckets and objects within are private

### [S3 Version Control](https://docs.aws.amazon.com/AmazonS3/latest/dev/Versioning.html)

* Once you enable versioning on bucket, you can't disable it, you can only suspend it
* Every time you update an object, it will become private by default
* It integrates with Lifecycle rules.
* You pay for each version you have
* Delete an object:

    Once you delete a file inside a versioned bucket, you don't delete the file, you simply add a Delete Marker (this basically creates a new version of the object)
    If you delete the version with the Detele Marker you will basically restore the object.

    If you want to permanently delete the object, you have to delete all the Versions of the object.

    You can optionally add another layer of security by configuring a bucket to enable MFA Delete

    More info about versioning:
    [ObjectVersioning](https://docs.aws.amazon.com/AmazonS3/latest/dev/ObjectVersioning.html)
    and
    [Versioning](https://docs.aws.amazon.com/AmazonS3/latest/dev/Versioning.html)

### [S3 Cross region replication](https://docs.aws.amazon.com/AmazonS3/latest/dev/crr.html)

* Regions must be unique
* Cross region replication doesn't replicate existing object by default, only new ones (after the replicate is set) will be replicated automatically.
* In order to replicate the existing objects, you need to do a `cp` using the aws cli:

    `aws s3 cp --recursive s3://alessio-casco-versioning s3://alessio-casco-versioning-replica-sydney`
* If you delete an object in the primary bucket, the delete action and markers won't be done or replicated in your remote bucket, this is a security function.
Only creations and modifications are replicated to the bucket in the other regions, NOT the delete
* You can't replicate over multiple buckets, the maps are always 1-to-1

## [CloudFront](https://aws.amazon.com/cloudfront/)

* A content delivery network or content distribution network is a geographically distributed network of proxy servers and their data centres

* Key terminology about CloudFront:
  * Edge Location: Is the location where the content is cached (separate from AWS AZ's or regions)
    Be aware that you can also write on edge locations, is not read only
  * Invalidating (erasing) the CDN cache costs money (exam topic)
  * Origin: Is the source of the files the CDN will distribute. An origin can be an EC2 instance, an S3 bucket, an Elastic Load Balancer or Route53, 
  you can also have your own origin, it not mandatory that is within AWS.
  * Distribution: Is the name AWS calls CDN's.
    * You can Have two types: Web that is for generic web contents and RTMP that is for video streaming
    * TTL: time to live of the cached object.

### [S3 Security & Encryption](https://aws.amazon.com/blogs/aws/new-amazon-s3-encryption-security-features/)

* You can configure S3 to create access logs for requests made to the S3 bucket
* Access control for buckets:
  * Bucket policies: Permission bucket wide
  * Access control list: Permissions that can be applied to the single object

* Encryption:
  * In transit: from to your bucket, HTTPS for example
  * At rest:
    * Server-side encryption:
      * S3 Managed Keys: SSE-S3 (Keys are managed by S3)
      * Key Management Service: SS3-KMS the customer manages the keys
      * Server-side encryption: Here you manage the keys, and Amazon manage the writes
  * Client-side Encryption: You encrypt the data and you upload it encrypted to S3

## [Amazon Storage](https://aws.amazon.com/products/storage/)

### [Amazon Storage Gateway](https://aws.amazon.com/storagegateway/)

Storage Gateway: AWS Storage Gateway connects an on-premises software appliance with cloud-based storage to provide seamless integration with data security features between your on-premises IT environment and the AWS storage infrastructure.

* File Gateway: For flat files, stored directly in S3. You can NFS Mount points
* Volume gateway (iSCSI): Block-based(ex. virtual HDD ) storage
  * Store volume (you keep all your data on prem and asynchronously backed up to S3)
  * Cached Volumes (you keep only the most recent data on prem, but Entire Dataset on S3)
Tape Gateway (VTL): Virtual tapes

### [Snowball](https://aws.amazon.com/snowball/)

Import Export is still available and was the first version of snowball, you used to ship your drives to AWS

Snowball is (an appliance) a petabyte-scale data transport solution that uses devices designed to be secure to transfer large amounts of data INTO and OUT of the AWS Cloud

Snowball edge: is a 100TB data transfer device with onboard storage-computer capabilities. It's like an AWS DC in a box

Snowmobile: AWS Snowmobile is an Exabyte-scale data transfer service used to move extremely large amounts of data to AWS. A truck full of disks basically.

## [S3 Transfer Acceleration](https://docs.aws.amazon.com/AmazonS3/latest/dev/transfer-acceleration.html)

Instead of uploading files directly to your S3 bucket, you can use the AWS edge network.
Using a specific URL, you upload the file to your local edge and then the file will be uploaded to S3
an example or URL:  alessio-casco-accelerate.s3-accelerate.amazonaws.com

## [Static Website Using S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/HowDoIWebsiteConfiguration.html)

* You can use bucket policies to make entire S3 buckets public
* You can use S3 to host only static websites.
* S3 Scales automatically to meet demand

[Permissions Required for Website Access](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteAccessPermissionsReqd.html)

On console: Amazon S3 => Your_Bucket => Permissions => Bucket Policy

```json
{
  "Version":"2012-10-17",
  "Statement":[{
    "Sid":"PublicReadGetObject",
        "Effect":"Allow",
      "Principal": "*",
      "Action":["s3:GetObject"],
      "Resource":["arn:aws:s3:::example-bucket/*"
      ]
    }
  ]
}
```

