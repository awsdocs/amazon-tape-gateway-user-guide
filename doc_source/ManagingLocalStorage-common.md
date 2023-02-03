--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

--------

# Managing local disks for your Storage Gateway<a name="ManagingLocalStorage-common"></a>

The gateway virtual machine \(VM\) uses the local disks that you allocate on\-premises for buffering and storage\. Gateways created on Amazon EC2 instances use Amazon EBS volumes as local disks\. 

**Topics**
+ [Deciding the amount of local disk storage](#decide-local-disks-and-sizes)
+ [Optimizing Gateway Performance](#OptimizingGatewayPerformance)
+ [Determining the size of upload buffer to allocate](#CachedLocalDiskUploadBufferSizing-common)
+ [Determining the size of cache storage to allocate](#CachedLocalDiskCacheSizing-common)
+ [Configuring additional upload buffer or cache storage](#ConfiguringLocalDiskStorage)

## Deciding the amount of local disk storage<a name="decide-local-disks-and-sizes"></a>

The number and size of disks that you want to allocate for your gateway is up to you\. Depending on the storage solution you deploy \(see [Plan your Storage Gateway deployment](WhatIsStorageGateway.md#planning-gateway-deployment)\), the gateway requires the following additional storage:
+ Tape Gateways require at least two disks\. One to use as a cache, and one to use as an upload buffer\.

The following table recommends sizes for local disk storage for your deployed gateway\. You can add more local storage later after you set up the gateway, and as your workload demands increase\.


| Local storage | Description | 
| --- | --- | 
| Upload buffer | The upload buffer provides a staging area for the data before the gateway uploads the data to Amazon S3\. Your gateway uploads this buffer data over an encrypted Secure Sockets Layer \(SSL\) connection to AWS\. | 
| Cache storage | The cache storage acts as the on\-premises durable store for data that is pending upload to Amazon S3 from the upload buffer\. When your application performs I/O on a volume or tape, the gateway saves the data to the cache storage for low\-latency access\. When your application requests data from a volume or tape, the gateway first checks the cache storage for the data before downloading the data from AWS\. | 

**Note**  
When you provision disks, we strongly recommend that you do not provision local disks for the upload buffer and cache storage if they use the same physical resource \(the same disk\)\. Underlying physical storage resources are represented as a data store in VMware\. When you deploy the gateway VM, you choose a data store on which to store the VM files\. When you provision a local disk \(for example, to use as cache storage or upload buffer\), you have the option to store the virtual disk in the same data store as the VM or a different data store\.  
If you have more than one data store, we strongly recommend that you choose one data store for the cache storage and another for the upload buffer\. A data store that is backed by only one underlying physical disk can lead to poor performance in some situations when it is used to back both the cache storage and upload buffer\. This is also true if the backup is a less\-performant RAID configuration such as RAID1\.

After the initial configuration and deployment of your gateway, you can adjust the local storage by adding or removing disks for an upload buffer\. You can also add disks for cache storage\.

## Optimizing Gateway Performance<a name="OptimizingGatewayPerformance"></a>

To achieve optimal performance use high throughput SSD disks for both cache and upload buffer
+ Use different disks for cache and upload buffer\. If using RAID, ensure that cache and upload buffer d isks use separate RAID controllers at the hardware level\.
+ Add at least 2 different upload buffer disks\.
+ Use a RAID 0 striped raid configuration for cache \+ upload buffer devices to improve throughput\. This is especially critical for the cache disk\.

## Determining the size of upload buffer to allocate<a name="CachedLocalDiskUploadBufferSizing-common"></a>

You can determine the size of your upload buffer to allocate by using an upload buffer formula\. We strongly recommend that you allocate at least 150 GiB of upload buffer\. If the formula returns a value less than 150 GiB, use 150 GiB as the amount you allocate to the upload buffer\. You can configure up to 2 TiB of upload buffer capacity for each gateway\.

**Note**  
For Tape Gateways, when the upload buffer reaches its capacity, your applications can continue to read from and write data to your storage volumes\. However, the Tape Gateway does not write any of your volume data to its upload buffer and does not upload any of this data to AWS until Storage Gateway synchronizes the data stored locally with the copy of the data stored in AWS\. This synchronization occurs when the volumes are in BOOTSTRAPPING status\.

To estimate the amount of upload buffer to allocate, you can determine the expected incoming and outgoing data rates and plug them into the following formula\.

**Rate of incoming data**  
This rate refers to the application throughput, the rate at which your on\-premises applications write data to your gateway over some period of time\.

**Rate of outgoing data**  
This rate refers to the network throughput, the rate at which your gateway is able to upload data to AWS\. This rate depends on your network speed, utilization, and whether you've enabled bandwidth throttling\. This rate should be adjusted for compression\. When uploading data to AWS, the gateway applies data compression where possible\. For example, if your application data is text\-only, you might get an effective compression ratio of about 2:1\. However, if you are writing videos, the gateway might not be able to achieve any data compression and might require more upload buffer for the gateway\.

We strongly recommend that you allocate at least 150 GiB of upload buffer space if either of the following is true:
+ Your incoming rate is higher than the outgoing rate\.
+ The formula returns a value less than 150 GiB\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/tgw/images/WorkingStorageFormula-diagram.png)

For example, assume that your business applications write text data to your gateway at a rate of 40 MB per second for 12 hours per day and your network throughput is 12 MB per second\. Assuming a compression factor of 2:1 for the text data, you would allocate approximately 690 GiB of space for the upload buffer\.

**Example**  

```
1. ((40 MB/sec) - (12 MB/sec * 2)) * (12 hours * 3600 seconds/hour) = 691200 megabytes
```

You can initially use this approximation to determine the disk size that you want to allocate to the gateway as upload buffer space\. Add more upload buffer space as needed using the Storage Gateway console\. Also, you can use the Amazon CloudWatch operational metrics to monitor upload buffer usage and determine additional storage requirements\. For information on metrics and setting the alarms, see [Monitoring the upload buffer](Main_monitoring-gateways-common.md#PerfUploadBuffer-common)\.

## Determining the size of cache storage to allocate<a name="CachedLocalDiskCacheSizing-common"></a>

Your gateway uses its cache storage to provide low\-latency access to your recently accessed data\. The cache storage acts as the on\-premises durable store for data that is pending upload to Amazon S3 from the upload buffer\. Generally speaking, you size the cache storage at 1\.1 times the upload buffer size\. For more information about how to estimate your cache storage size, see [Determining the size of upload buffer to allocate](#CachedLocalDiskUploadBufferSizing-common)\.

You can initially use this approximation to provision disks for the cache storage\. You can then use Amazon CloudWatch operational metrics to monitor the cache storage usage and provision more storage as needed using the console\. For information on using the metrics and setting up alarms, see [Monitoring cache storage](Main_monitoring-gateways-common.md#PerfCache-common)\.

## Configuring additional upload buffer or cache storage<a name="ConfiguringLocalDiskStorage"></a>

As your application needs change, you can increase the gateway's upload buffer or cache storage capacity\. You can add storage capacity to your gateway without interrupting functionality or causing downtime\. When you add more storage, you do so with the gateway VM turned on\.

**Important**  
When adding cache or upload buffer to an existing gateway, you must create new disks on the gateway host hypervisor or Amazon EC2 instance\. Do not remove or change the size of existing disks that have already been allocated as cache or upload buffer\.<a name="GatewayWorkingStorageCachedTaskBuffer"></a>

**To configure additional upload buffer or cache storage for your gateway**

1. Provision one or more new disks on your gateway host hypervisor or Amazon EC2 instance\. For information about how to provision a disk on a hypervisor, see your hypervisor's documentation\. For information about provisioning Amazon EBS volumes for an Amazon EC2 instance, see [Amazon EBS volumes](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volumes.html) in the *Amazon Elastic Compute Cloud User Guide for Linux Instances*\. In the following steps, you will configure this disk as upload buffer or cache storage\.

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Gateways**\.

1. Search for your gateway and select it from the list\.

1. From the **Actions** menu, choose **Configure storage**\.

1. In the **Configure storage** section, identify the disks you provisioned\. If you don't see your disks, choose the refresh icon to refresh the list\. For each disk, choose either **UPLOAD BUFFER** or **CACHE STORAGE** from the **Allocated to** drop\-down menu\.

1. Choose **Save changes** to save your configuration settings\.