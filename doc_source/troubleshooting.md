# Troubleshooting<a name="troubleshooting"></a>

## File System Mount Hangs and Then Fails with Timeout Error<a name="mount-hangs-fails-timeout"></a>

The file system mount command hangs for a minute or two, and then fails with a timeout error\. 

The following code shows an example\.

```
sudo mount -t lustre file_system_dns_name@tcp:/fsx /mnt/fsx

[2+ minute wait here]
Connection timed out
```

This error can occur because the security groups for the Amazon EC2 instance or the file system aren't configured properly\.

**Action to Take**

Make sure that your security groups for the file system have the inbound rules specified in [Amazon VPC Security Groups](limit-access-security-groups.md#fsx-vpc-security-groups)\. 

## Automatic Mounting Fails and the Instance Is Unresponsive<a name="lustre-automount-fails"></a>

In some cases, automatic mounting might fail for a file system and your Amazon EC2 instance might stop responding\.

This issue can occur if the `_netdev` option wasn't declared\. If `_netdev` is missing, your Amazon EC2 instance can stop responding\. This result is because network file systems need to be initialized after the compute instance starts its networking\.

**Action to Take**  
If this issue occurs, contact AWS Support\.

## File System Mount Using DNS Name Fails<a name="mount-fails-dns-name"></a>

A file system mount that is using a Domain Name Service \(DNS\) name fails\. The following code shows an example\.

```
sudo mount -t lustre file_system_dns_name@tcp:/fsx /mnt/fsx
mount.lustre: Can't parse NID 
'file_system_dns_name@tcp:/fsx'
```

**Action to Take**

Check your virtual private cloud \(VPC\) configuration\. If you are using a custom VPC, make sure that DNS settings are enabled\. For more information, see [Using DNS with Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html) in the *Amazon VPC User Guide*\. 

To specify a DNS name in the `mount` command, do the following:
+ Ensure that the Amazon EC2 instance is in the same VPC as your Amazon FSx for Lustre file system\.
+ Connect your Amazon EC2 instance inside a VPC configured to use the DNS server provided by Amazon\. For more information, see [DHCP Options Sets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_DHCP_Options.html) in the *Amazon VPC User Guide*\.
+ Ensure that the Amazon VPC of the connecting Amazon EC2 instance has DNS host names enabled\. For more information, see [Updating DNS Support for Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html#vpc-dns-updating) in the *Amazon VPC User Guide*\.

## Creating a File System with Data Repository Fails<a name="slr-permissions-fails"></a>

You can't create a file system linked to a data repository in Amazon S3 bucket, and encounter an error like the following\.

```
User: arn:aws:iam::012345678901:user/username is not authorized to perform: iam:PutRolePolicy on resource: resource ARN
```

This error can happen if you try to create a file system linked to a data repository in an Amazon S3 bucket without the necessary IAM permissions\. The required IAM permissions support the Amazon FSx for Lustre service\-linked role that is used to access the specified Amazon S3 bucket on your behalf\.

**Action to Take**

Ensure that your IAM entity \(user, group, or role\) has the appropriate permissions to create file systems\. Doing this includes adding the permissions policy that supports the Amazon FSx for Lustre service\-linked role\. For more information, see [Adding Permissions to Use Data Repositories in Amazon S3](setting-up.md#fsx-adding-permissions-s3)\.

For more information about service\-linked roles, see [Using Service\-Linked Roles for Amazon FSx for Lustre](using-service-linked-roles.md)\.