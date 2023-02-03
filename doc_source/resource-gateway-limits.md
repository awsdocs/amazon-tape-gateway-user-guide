--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

--------

# AWS Storage Gateway quotas<a name="resource-gateway-limits"></a>

In this topic, you can find information about volume and tape quotas, configuration, and performance limits for Storage Gateway\.

**Topics**
+ [Quotas for tapes](#resource-tape-limits)
+ [Recommended local disk sizes for your gateway](#disk-sizes)

## Quotas for tapes<a name="resource-tape-limits"></a>

The following table lists quotas for tapes\.


| Description | Tape Gateway | 
| --- | --- | 
| Minimum size of a virtual tape | 100 GiB | 
| Maximum size of a virtual tape | 15 TiB | 
| Maximum number of virtual tapes for a virtual tape library \(VTL\) | 1,500  | 
| Total size of all tapes in a virtual tape library \(VTL\) | 1 PiB | 
| Maximum number of virtual tapes in archive | No limit | 
| Total size of all tapes in a archive | No limit | 

## Recommended local disk sizes for your gateway<a name="disk-sizes"></a>

The following table recommends sizes for local disk storage for your deployed gateway\. 


| Gateway Type | Cache \(Minimum\) | Cache \(Maximum\) | Upload Buffer \(Minimum\) | Upload Buffer \(Maximum\) | Other Required Local Disks | 
| --- | --- | --- | --- | --- | --- | 
| Tape gateway | 150 GiB | 64 TiB | 150 GiB | 2 TiB | — | 

**Note**  
You can configure one or more local drives for your cache and upload buffer, up to the maximum capacity\.  
When adding cache or upload buffer to an existing gateway, it's important to create new disks in your host \(hypervisor or Amazon EC2 instance\)\. Don't change the size of existing disks if the disks have been previously allocated as either a cache or upload buffer\.