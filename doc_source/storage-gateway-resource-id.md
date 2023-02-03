--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

--------

# Understanding Storage Gateway Resources and Resource IDs<a name="storage-gateway-resource-id"></a>

In Storage Gateway, the primary resource is a *gateway* but other resource types include: *volume*, * virtual tape*, *iSCSI target*, and *vtl device*\. These are referred to as *subresources* and they don't exist unless they are associated with a gateway\.

These resources and subresources have unique Amazon Resource Names \(ARNs\) associated with them as shown in the following table\.


| Resource Type | ARN Format | 
| --- | --- | 
|  Gateway ARN  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id`  | 
| Tape ARN |  `arn:aws:storagegateway:region:account-id:tape/tapebarcode`  | 
| Target ARN \( iSCSI target\) |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id/target/iSCSItarget`  | 
|  VTL Device ARN  |  `arn:aws:storagegateway:region:account-id:gateway/gateway-id/device/vtldevice`  | 

Storage Gateway also supports the use of EC2 instances and EBS volumes and snapshots\. These resources are Amazon EC2 resources that are used in Storage Gateway\.

## Working with Resource IDs<a name="working-with-id"></a>

When you create a resource, Storage Gateway assigns the resource a unique resource ID\. This resource ID is part of the resource ARN\. A resource ID takes the form of a resource identifier, followed by a hyphen, and a unique combination of eight letters and numbers\. For example, a gateway ID is of the form `sgw-12A3456B` where `sgw` is the resource identifier for gateways\. A volume ID takes the form `vol-3344CCDD` where `vol` is the resource identifier for volumes\.

For virtual tapes, you can prepend a up to a four character prefix to the barcode ID to help you organize your tapes\.

Storage Gateway resource IDs are in uppercase\. However, when you use these resource IDs with the Amazon EC2 API, Amazon EC2 expects resource IDs in lowercase\. You must change your resource ID to lowercase to use it with the EC2 API\. For example, in Storage Gateway the ID for a volume might be `vol-1122AABB`\. When you use this ID with the EC2 API, you must change it to `vol-1122aabb`\. Otherwise, the EC2 API might not behave as expected\.