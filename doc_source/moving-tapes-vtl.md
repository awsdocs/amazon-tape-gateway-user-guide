--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

--------

# Moving Your Tape from S3 Glacier Flexible Retrieval to S3 Glacier Deep Archive Storage Class<a name="moving-tapes-vtl"></a>



Move your tapes from S3 Glacier Flexible Retrieval to S3 Glacier Deep Archive for long\-term data retention and digital preservation at a very low cost\. You use S3 Glacier Deep Archive for long\-term data retention and digital preservation where the data is accessed once or twice a year\. For detailed information, see [Storage Classes for Archiving Objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier)\.

**To move a tape from S3 Glacier Flexible Retrieval to S3 Glacier Deep Archive**

1. In the navigation pane, choose **Tape Library > Tapes** to see your tapes\. By default, this list displays up to 1,000 tapes at a time, but the searches that you perform apply to all of your tapes\. You can use the search bar to find tapes that match a specific criteria, or to reduce the list to less than 1,000 tapes\. When your list contains 1,000 tapes or fewer, you can then sort your tapes in ascending or descending order by various properties\.

1. Select the check boxes for the tapes you want to move to S3 Glacier Deep Archive\. You can see the pool that each tape is associated with in the **Pool** column\.

1. Choose **Assign to pool**\.

1. In the Assign tape to pool dialog box, verify the barcodes for the tapes you are moving and choose **Assign**\. 
**Note**  
If a tape has been ejected by the backup application and archived in S3 Glacier Deep Archive, you can't move it back to S3 Glacier Flexible Retrieval\. There's a charge for moving your tapes from S3 Glacier Flexible Retrieval to S3 Glacier Deep Archive\. In addition, if you move tapes from S3 Glacier Flexible Retrieval to S3 Glacier Deep Archive prior to 90 days, there is an early deletion fee for S3 Glacier Flexible Retrieval\.

1. After the tape is moved, you can see the updated status in the **Pool** column on the **Tape overview** page\.