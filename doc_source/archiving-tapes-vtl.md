--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

--------

# Archiving Virtual Tapes<a name="archiving-tapes-vtl"></a>

You can archive your tapes to S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive\. When you create a tape, you choose the archive pool that you want to use to archive your tape\. 

You choose **Glacier Pool** if you want to archive the tape in S3 Glacier Flexible Retrieval\. When your backup software ejects the tape, it is automatically archived in S3 Glacier Flexible Retrieval\. You use S3 Glacier Flexible Retrieval for more active archives where the data is regularly retrieved and needed in minutes\. For detailed information, see [Storage Classes for Archiving Objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier) 

You choose **Deep Archive Pool** if you want to archive the tape in S3 Glacier Deep Archive\. When your backup software ejects the tape, the tape is automatically archived in S3 Glacier Deep Archive\. You use S3 Glacier Deep Archive for long\-term data retention and digital preservation at a very low cost\. Data in S3 Glacier Deep Archive is not retrieved often or is rarely retrieved\. For detailed information, see [Storage Classes for Archiving Objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier)\.

**Note**  
Any tape created before March 27, 2019, are archived directly in S3 Glacier Flexible Retrieval when your backup software ejects it\.

When your backup software ejects a tape, it is automatically archived in the pool that you chose when you created the tape\. The process for ejecting a tape varies depending on your backup software\. Some backup software requires that you export tapes after they are ejected before archiving can begin\. For information about supported backup software, see [Using Your Backup Software to Test Your Gateway Setup](https://docs.aws.amazon.com/storagegateway/latest/tgw/GettingStartedTestGatewayVTL.html)\.