# S3 Security
May 2018

## Overview
Having seen the US Army S3 spillage of September 2017 it’s clear that misconfiguration of S3 can lead to very serious data spillage. This document seeks to provide guidance on how best to design your applications to make best use of S3 buckets and  prevent malingering head trauma.

## Golden Rules
* Amazon are not responsible for the security of S3 buckets. We are.
* S3 buckets MUST NOT be web enabled unless explicitly authorized. Don't publish the bucket as a web page. Ever.
* S3 Buckets MUST NOT be made publicly available unless this is explicitly designed for and authorized.
* S3 buckets MUST be accessible only by appropriate/authorized members of staff or third parties.
* Buckets MUST be encrypted (it doesn't seem add any overhead)
* Only keep online/in the bucket such files/data as needed. Day/week/month’s worth of data in an folder or bucket. Strategy: Limit duration data is stored in bucket
* [Enable auditing of access to buckets]( https://docs.aws.amazon.com/AmazonS3/latest/dev/ServerLogs.html) if necessary.
* Enable versioning of S3 objects. May be useful for comparing the state of objects at different times.
* Versioning. Consider enabling versioning of objects in S3.

## Detect misconfiguration?
https://aws.amazon.com/blogs/aws/new-amazon-s3-encryption-security-features/

## Vendor Specific Features
Depending on sensitivity of information consider setting default encryption on the bucket using either S3 Managed Keys (SSE-S3) or AWS KMS Managed Keys (SSE-KMS). SSE-KMS is available at extra cost. Optionally, encrypt the data yourself prior to writing to S3.

### Benchmark: Does AWS SSE-S3 encryption have a performance cost versus non encrypted?
In this simple benchmark there is no difference in performance between no encryption and SSE-S3 encryption on the bucket.
[S3 Benchmark Encrypted Bucket](https://gist.github.com/gm3dmo/b3f7b18fccc3921be3fdbf304133537d
)
[S3 Benchmark Not Encrypted](https://gist.github.com/gm3dmo/4335c81546f92a1b840de3f30efadb37)

#### Results: Encrypted vs Unencrypted download upload to S3
```
                          count   mean       std   min     25%    50%
Function
downloadFromS3              10.0  3.751  1.581191  2.46  2.6950  2.890
downloadFromS3Encrypted     10.0  3.751  1.776191  2.43  2.4900  2.720
upload2S3                   10.0  4.052  2.768541  2.50  2.9825  3.380
upload2S3encrypted          10.0  3.235  0.648712  2.43  2.6800  3.385
```
Conclusion: If storing personal/sensitive data then encrypt the data outside aws and store encrypted streams on the S3 bucket. There seems little point in encrypting the data twice over but even then there is little disadvantage to that.
Events
Tags


### Tools
#### Prowler
[Prowler](https://gist.github.com/gm3dmo/b6b3e495a752553052bf9fb3937bf0f7) is a good starting point for auditing S3 bucket configuration.

Detect Config Change
http://docs.aws.amazon.com/config/latest/developerguide/gs-cli-subscribe.html

#### Security Monkey from Netflix
worth thinking of security monkey - policy based and acceptance]  https://github.com/Netflix/security_monkey
[also we should put AWS Config in place - it would be good to get the UCFS standard policies to apply to our accounts]

### Expertise
Who is expert in this?
Chris Vickery is a security type with expertise in uncovering S3 problems.

https://threatpost.com/chris-vickery-on-amazon-s3-data-leaks/128120/

Listen to the [S3 Security Podcast](https://arstechnica.com/information-technology/2017/05/defense-contractor-stored-intelligence-data-in-amazon-cloud-unprotected/)
