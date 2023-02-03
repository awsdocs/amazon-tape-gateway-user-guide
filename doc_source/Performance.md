--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

--------

# Performance<a name="Performance"></a>

This section describes Storage Gateway performance\.

**Topics**
+ [Performance guidance for Tape Gateways](#performance-tgw)
+ [Optimizing Gateway Performance](#Optimizing-common)
+ [Using VMware vSphere High Availability with Storage Gateway](#vmware-ha)

## Performance guidance for Tape Gateways<a name="performance-tgw"></a>

In this section, you can find configuration guidance for provisioning hardware for your Tape Gateway VM\. The Amazon EC2 instance sizes and types that are listed in the table are examples, and are provided for reference\. 


| Configuration | Write Throughput Gbps | Read from Cache Throughput Gbps | Read from Amazon Web Services Cloud Throughput Gbps | 
| --- | --- | --- | --- | 
|  Host Platform: Amazon EC2 instance— c5\.4xlarge  CPU: 16 vCPU \| RAM: 32 GB Root disk: 80 GB, io1 SSD, 4,000 IOPS Cache disk: striped RAID \(2 x 500 GB, io1 EBS SSD, 25000 IOPs\) Upload buffer disk: 450 GB, io1 SSD, 2000 IOPs Network bandwidth to cloud: 10 Gbps  | 2\.3  | 4\.0  | 2\.2  | 
|  Host platform: [Storage Gateway Hardware Appliance](https://www.amazon.com/dp/B079RBVX3M) CPU: 20 cores \| RAM: 128 GB  Cache disk: 2\.5 TB Upload buffer disk: 2 TB  Network bandwidth to cloud: 10 Gbps   | 2\.3  | 8\.8  | 3\.8  | 
|  Host platform: Amazon EC2instance— c5d\.9xlarge  CPU: 36 vCPU \| RAM: 72 GB Root disk: 80 GB, io1 SSD, 4,000 IOPS Cache disk: 900 GB NVMe disk Upload buffer disk: 900 GB NVMe disk  Network bandwidth to cloud: 10 Gbps  | 5\.2  | 11\.6  | 5\.2  | 
|  Host platform: Amazon EC2instance— c5d\.metal  CPU: 96 vCPU \| RAM: 192 GB Root disk: 80 GB, io1 SSD, 4,000 IOPS Cache disk: striped RAID \(2 x 900 GB NVMe disk\) Upload buffer disk: 900 GB NVMe disk  Network bandwidth to cloud: 10 Gbps  | 5\.2  | 11\.6  | 7\.2  | 

**Note**  
This performance was achieved by using a 1 MB block size and ten tape drives simultaneously\.  
The EC2 configurations in the above table are only intended to be representative of the performance you might attain on your own physical servers with similar resources\. For example, the EC2 configurations using a striped RAID were done through a special mechanism that is not generally supported by our gateway on EC2\. To achieve similar performance, you should instead use a hardware RAID controller attached to the on\-premise server running your gateway\.  
Your performance might vary based on your host platform configuration and network bandwidth\.

 To improve write and read throughput performance of your Tape Gateway, see [Optimize iSCSI Settings](#optimize-iSCSI), [Use a Larger Block Size for Tape Drives](#block-size), and [Optimize the Performance of Virtual Tape Drives in the Backup Software](#optimize-virtual-tape-drive)\. 

## Optimizing Gateway Performance<a name="Optimizing-common"></a>

### Recommended Gateway Server Configuration<a name="Recommended-vtl-resources-common"></a>

To obtain the best performance out of your gateway, Storage Gateway recommends the following gateway configuration for your gateway's host server:
+ At least 64 dedicated physical CPU cores
+ For Tape Gateway, your hardware should dedicate the following amounts of RAM:
  + At least 16 GiB of reserved RAM for gateways with cache size up to 16 TiB
  + At least 32 GiB of reserved RAM for gateways with cache size 16 TiB to 32 TiB
  + At least 48 GiB of reserved RAM for gateways with cache size 32 TiB to 64 TiB
**Note**  
For optimal gateway performance, you must provision at least 32 GiB of RAM\.
+ Disk 1, to be used as the gateway cache as follows:
  + Striped RAID \(redundant array of independent disks\) consisting of NVMe SSDs\. 
+ Disk 2, to be used as the gateway upload buffer as follows: 
  + Striped RAID consisting of NVMe SSDs\. 
+ Disk 3, to be used as the gateway upload buffer as follows: 
  + Striped RAID consisting of NVMe SSDs\. 
+ Network adapter 1 configured on VM network 1:
  + Use VM network 1 and add VMXnet3 \(10 Gbps\) to be used for ingestion\.
+ Network adapter 2 configured on VM network 2:
  + Use VM network 2 and add a VMXnet3 \(10 Gbps\) to be used to connect to AWS\.

### Add Resources to Your Gateway<a name="Optimizing-vtl-add-resources-common"></a>

The following bottlenecks can reduce the performance of your Tape Gateway below the theoretical maximum sustained throughput \(your bandwidth to AWS cloud\):
+ CPU core count
+ Cache/Upload buffer disk throughput
+ Total RAM amount
+ Network bandwidth to AWS
+ Network bandwidth from initiator to gateway

This section contains steps you can take in order to optimize the performance of your gateway\. This guidance is based on adding resources to your gateway or your application server\. 

 You can optimize gateway performance by adding resources to your gateway in one or more of the following ways\.

**Use higher\-performance disks**  
Cache and upload buffer disk throughput can limit your gateway's upload and download performance\. If your gateway is exhibiting performance significantly below what is expected, consider improving the cache and upload buffer disk throughput by:   
+ Using a striped RAID such as RAID 10 to improve disk throughput, ideally with a hardware RAID controller\. 
**Note**  
RAID \(redundant array of independent disks\) or specifically disk striped RAID configurations like RAID 10, is the process of dividing a body of data into blocks and spreading the data blocks across multiple storage devices\. The RAID level you use affects the exact speed and fault tolerance you can achieve\. By striping IO workloads out across multiple disks, the overall throughput of the RAID device is much higher than that of any single member disk\. 
+ Using directly attached, high performance disks

  To optimize gateway performance, you can add high\-performance disks such as solid\-state drives \(SSDs\) and a NVMe controller\. You can also attach virtual disks to your VM directly from a storage area network \(SAN\) instead of the Microsoft Hyper\-V NTFS\. Improved disk performance generally results in better throughput and more input/output operations per second \(IOPS\)\. 

  To measure throughput, use the `ReadBytes` and `WriteBytes` metrics with the `Samples` Amazon CloudWatch statistic\. For example, the `Samples` statistic of the `ReadBytes` metric over a sample period of 5 minutes divided by 300 seconds gives you the IOPS\. As a general rule, when you review these metrics for a gateway, look for low throughput and low IOPS trends to indicate disk\-related bottlenecks\. For more information about gateway metrics, see [Measuring Performance Between Your Tape Gateway and AWS](GatewayMetrics-vtl-common.md#PerfGatewayAWS-vtl-common)\. 
**Note**  
CloudWatch metrics are not available for all gateways\. For information about gateway metrics, see [Monitoring Storage Gateway](Main_monitoring-gateways-common.md)\. 

**Add CPU resources to your gateway host**  
The minimum requirement for a gateway host server is four virtual processors\. To optimize gateway performance, confirm that each virtual processor that is assigned to the gateway VM is backed by a dedicated CPU core\. In addition, confirm that you are not oversubscribing the CPUs of the host server\.   
When you add additional CPUs to your gateway host server, you increase the processing capability of the gateway\. Doing this allows your gateway to deal with, in parallel, both storing data from your application to your local storage and uploading this data to Amazon S3\. Additional CPUs also help ensure that your gateway gets enough CPU resources when the host is shared with other VMs\. Providing enough CPU resources has the general effect of improving throughput\.

 **Back gateway virtual disks with separate physical disks**  
When you provision gateway disks, we strongly recommend that you *don't* provision local disks for the upload buffer and cache storage that use the same underlying physical storage disk\. For example, for VMware ESXi, the underlying physical storage resources are represented as a data store\. When you deploy the gateway VM, you choose a data store on which to store the VM files\. When you provision a virtual disk \(for example, as an upload buffer\), you can store the virtual disk in the same data store as the VM or a different data store\.   
If you have more than one data store, then we strongly recommend that you choose one data store for each type of local storage you are creating\. A data store that is backed by only one underlying physical disk can lead to poor performance\. An example is when you use such a disk to back both the cache storage and upload buffer in a gateway setup\. Similarly, a data store that is backed by a less high\-performing RAID configuration such as RAID 1 or RAID 6 can lead to poor performance\.

**Increase bandwidth between your gateway and AWS cloud**  
Increasing your bandwidth to and from AWS will increase the maximum rate of data ingress to your gateway and egress to AWS cloud\. This can improve your gateway performance if network speed is the limiting factor in your gateway configuration, rather than other factors like slow disks or poor gateway\-initiator connection bandwidth\.  
Network bandwidth to and from AWS defines the *theoretical maximum* average performance of your Tape Gateway during sustained workloads\.  
+ The average rate at which you can write data to your Tape Gateway over long intervals will not exceed your upload bandwidth to AWS\.
+ The average rate at which you can read data from your Tape Gateway over long intervals will not exceed your download bandwidth to AWS\.
Your observed gateway performance will likely be lower than your network bandwidth due to other limiting factors listed here, such as cache/upload buffer disk throughput, CPU core count, total RAM amount, or the bandwidth between your initiator and gateway\. Furthermore, your gateway's normal operation involves many actions taken to protect your data, which might cause the observed performance to be less than your network bandwidth\. 

### Optimize iSCSI Settings<a name="optimize-iSCSI"></a>

 You can optimize iSCSI settings on your iSCSI initiator to achieve higher I/O performance\. We recommend choosing 256 KiB for `MaxReceiveDataSegmentLength` and `FirstBurstLength`, and 1 MiB for `MaxBurstLength`\. For more information about configuring iSCSI settings, see [Customizing iSCSI Settings](initiator-connection-common.md#recommendediSCSISettings)\. 

**Note**  
These recommended settings can enable overall better performance\. However, the specific iSCSI settings that are needed to optimize performance vary depending on which backup software you use\. For details, see your backup software documentation\. 

### Use a Larger Block Size for Tape Drives<a name="block-size"></a>

For a Tape Gateway, the default block size for a tape drive is 64 KB\. However, you can increase the block size up to 1 MB to improve I/O performance\. 

The block size that you choose depends on the maximum block size that your backup software supports\. We recommend that you set the block size of the tape drives in your backup software to a size that is as large as possible\. However, this block size must not be greater than the 1 MB maximum size that the gateway supports\. 

Tape Gateways negotiate the block size for virtual tape drives to automatically match what is set on the backup software\. When you increase the block size on the backup software, we recommend that you also check the settings to ensure that the host initiator supports the new block size\. For more information, see the documentation for your backup software\. For more information about specific gateway performance guidance, see [Performance](#Performance)\.

### Optimize the Performance of Virtual Tape Drives in the Backup Software<a name="optimize-virtual-tape-drive"></a>

Your backup software can back up data on up to 10 virtual tape drives on a Tape Gateway at the same time\. We recommend that you configure backup jobs in your backup software to use at least 4 virtual tape drives simultaneously on the Tape Gateway\. You can achieve better write throughput when the backup software is backing up data to more than one virtual tape at the same time\. 

 As a general rule, you can achieve a higher maximum throughput by operating on \(reading or writing from\) more virtual tapes at the same time\. By using more tape drives, you allow your gateway to service more requests concurrently, potentially improving performance\. 

### Add Resources to Your Application Environment<a name="Optimizing-vtl-add-resources-app-common"></a>

**Increase the bandwidth between your application server and your gateway**  
The connection between your iSCSI initiator and gateway can limit your upload and download performance\. If your gateway is exhibiting performance significantly worse than expected and you have already improved your CPU core count and disk throughput, consider:  
+ Upgrading your network cables to have higher bandwidth between your initiator and gateway\.
+ Using as many tape drives concurrently as possible\. iSCSI does not support queueing multiple requests for the same target, meaning that the more tape drives you use, the more requests that your gateway can service concurrently\. This will allow you to more fully utilize the bandwidth between your gateway and initiator, increasing your gateway's apparent throughput\.
To optimize gateway performance, ensure that the network bandwidth between your application and the gateway can sustain your application needs\. You can use the `ReadBytes` and `WriteBytes` metrics of the gateway to measure the total data throughput\. For more information about these metrics, see [Measuring Performance Between Your Tape Gateway and AWS](GatewayMetrics-vtl-common.md#PerfGatewayAWS-vtl-common)\.  
For your application, compare the measured throughput with the desired throughput\. If the measured throughput is less than the desired throughput, then increasing the bandwidth between your application and gateway can improve performance if the network is the bottleneck\. Similarly, you can increase the bandwidth between your VM and your local disks, if they're not direct\-attached\.

**Add CPU resources to your application environment**  
If your application can use additional CPU resources, then adding more CPUs can help your application to scale its I/O load\.

## Using VMware vSphere High Availability with Storage Gateway<a name="vmware-ha"></a>

Storage Gateway provides high availability on VMware through a set of application\-level health checks integrated with VMware vSphere High Availability \(VMware HA\)\. This approach helps protect storage workloads against hardware, hypervisor, or network failures\. It also helps protect against software errors, such as connection timeouts and file share or volume unavailability\.

With this integration, a gateway deployed in a VMware environment on\-premises or in a VMware Cloud on AWS automatically recovers from most service interruptions\. It generally does this in under 60 seconds with no data loss\. 

To use VMware HA with Storage Gateway, take the steps listed following\.

**Topics**
+ [Configure Your vSphere VMware HA Cluster](#vmware-ha-configure-cluster)
+ [Download the \.ova Image from the Storage Gateway console](#vmware-ha-download-image)
+ [Deploy the Gateway](#vmware-ha-deploy-gateway)
+ [\(Optional\) Add Override Options for Other VMs on Your Cluster](#vmware-ha-overrides)
+ [Activate Your Gateway](#vmware-ha-activate-gateway)
+ [Test Your VMware High Availability Configuration](#vmware-ha-test-failover)

### Configure Your vSphere VMware HA Cluster<a name="vmware-ha-configure-cluster"></a>

First, if you haven’t already created a VMware cluster, create one\. For information about how to create a VMware cluster, see [Create a vSphere HA Cluster](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.avail.doc/GUID-4BC60283-B638-472F-B1D2-1E4E57EAD213.html) in the VMware documentation\.

Next, configure your VMware cluster to work with Storage Gateway\.

**To configure your VMware cluster**

1. On the **Edit Cluster Settings** page in VMware vSphere, make sure that VM monitoring is configured for VM and application monitoring\. To do so, set the following options as listed:
   + **Host Failure Response**: **Restart VMs**
   + **Response for Host Isolation**: **Shut down and restart VMs**
   + **Datastore with PDL**: **Disabled**
   + **Datastore with APD**: **Disabled**
   + **VM Monitoring**: **VM and Application Monitoring**

   For an example, see the following screenshot\.   
![\[Editing cluster settings\]](http://docs.aws.amazon.com/storagegateway/latest/tgw/images/edit-cluster-settings.png)

1. Fine\-tune the sensitivity of the cluster by adjusting the following values: 
   + **Failure interval** – After this interval, the VM is restarted if a VM heartbeat isn't received\. 
   + **Minimum uptime** – The cluster waits this long after a VM starts to begin monitoring for VM tools' heartbeats\.
   + **Maximum per\-VM resets** – The cluster restarts the VM a maximum of this many times within the maximum resets time window\.
   + **Maximum resets time window** – The window of time in which to count the maximum resets per\-VM resets\. 

   If you aren't sure what values to set, use these example settings: 
   + **Failure interval**: **30** seconds 
   + **Minimum uptime**: **120** seconds 
   + **Maximum per\-VM resets**: **3**
   + **Maximum resets time window**: **1** hour 

If you have other VMs running on the cluster, you might want to set these values specifically for your VM\. You can't do this until you deploy the VM from the \.ova\. For more information on setting these values, see [\(Optional\) Add Override Options for Other VMs on Your Cluster](#vmware-ha-overrides)\.

### Download the \.ova Image from the Storage Gateway console<a name="vmware-ha-download-image"></a>

**To download the \.ova image for your gateway**
+ On the **Set up gateway** page in the Storage Gateway console, select your gateway type and host platform, then use the link provided in the console to download the \.ova as outlined in [Set up a Tape Gateway](https://docs.aws.amazon.com/storagegateway/latest/tgw/create-gateway-vtl.html)\.

### Deploy the Gateway<a name="vmware-ha-deploy-gateway"></a>

In your configured cluster, deploy the \.ova image to one of the cluster's hosts\.

**To deploy the gateway \.ova image**

1.  Deploy the \.ova image to one of the hosts in the cluster\.

1. Make sure the data stores that you choose for the root disk and the cache are available to all hosts in the cluster\. When deploying the Storage Gateway \.ova file in a VMware or on\-prem environment, the disks are described as paravirtualized SCSI disks\. *Paravirtualization* is a mode where the gateway VM works with the host operating system so the console can identify the virtual disks that you add to your VM\.

   To configure your VM to use paravirtualized controllers

   1. In the VMware vSphere client, open the context \(right\-click\) menu for your gateway VM, and then choose **Edit Settings**\.

   1. In the **Virtual Machine Properties** dialog box, choose the **Hardware** tab, select the **SCSI controller 0**, and then choose **Change Type**\.

   1. In the **Change SCSI Controller Type** dialog box, select the **VMware Paravirtual SCSI** controller type, and then choose **OK**\.

### \(Optional\) Add Override Options for Other VMs on Your Cluster<a name="vmware-ha-overrides"></a>

If you have other VMs running on your cluster, you might want to set the cluster values specifically for each VM\. 

**To add override options for other VMs on your cluster**

1. On the **Summary** page in VMware vSphere, choose your cluster to open the cluster page, and then choose **Configure**\.

1. Choose the **Configuration** tab, and then choose **VM Overrides**\.

1. Add a new VM override option to change each value\. 

   For override options, see the following screenshot\.  
![\[Override cluster settings\]](http://docs.aws.amazon.com/storagegateway/latest/tgw/images/override-cluster-settings.png)

### Activate Your Gateway<a name="vmware-ha-activate-gateway"></a>

After the \.ova for your gateway is deployed, activate your gateway\. The instructions about how are different for each gateway type\.

**To activate your gateway**
+ Follow the procedures outlined in the following topics:

  1. [Connect your Tape Gateway to AWS](https://docs.aws.amazon.com/storagegateway/latest/tgw/create-gateway-vtl.html#connect-to-amazon-tape)

  1. [Review settings and activate your Tape Gateway](https://docs.aws.amazon.com/storagegateway/latest/tgw/create-gateway-vtl.html#review-and-activate-tape)

  1. [Configure your Tape Gateway](https://docs.aws.amazon.com/storagegateway/latest/tgw/create-gateway-vtl.html#configure-gateway-tape)

### Test Your VMware High Availability Configuration<a name="vmware-ha-test-failover"></a>

After you activate your gateway, test your configuration\.

**To test your VMware HA configuration**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. On the navigation pane, choose **Gateways**, and then choose the gateway that you want to test for VMware HA\.

1. For **Actions**, choose **Verify VMware HA**\.

1. In the **Verify VMware High Availability Configuration** box that appears, choose **OK**\.
**Note**  
Testing your VMware HA configuration reboots your gateway VM and interrupts connectivity to your gateway\. The test might take a few minutes to complete\. 

   If the test is successful, the status of **Verified** appears in the details tab of the gateway in the console\.

1. Choose **Exit**\.

You can find information about VMware HA events in the Amazon CloudWatch log groups\. For more information, see [Getting Tape Gateway Health Logs with CloudWatch Log Groups](https://docs.aws.amazon.com/storagegateway/latest/tgw/GatewayMetrics-vtl-common.html#cw-log-groups-tape)\.