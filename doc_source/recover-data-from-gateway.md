--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

--------

# Best practices for recovering your data<a name="recover-data-from-gateway"></a>

Although it is rare, your gateway might encounter an unrecoverable failure\. Such a failure can occur in your virtual machine \(VM\), the gateway itself, the local storage, or elsewhere\. If a failure occurs, we recommend that you follow the instructions in the appropriate section following to recover your data\.

**Important**  
Storage Gateway doesn’t support recovering a gateway VM from a snapshot that is created by your hypervisor or from your Amazon EC2 Amazon Machine Image \(AMI\)\. If your gateway VM malfunctions, activate a new gateway and recover your data to that gateway using the instructions following\.

**Topics**
+ [Recovering from an unexpected virtual machine shutdown](#recover-from-gateway-shutdown)
+ [Recovering your data from a malfunctioning gateway or VM](#recover-from-gateway)
+ [Recovering your data from an irrecoverable tape](#recover-from-tape)
+ [Recovering your data from a malfunctioning cache disk](#recover-from-cahe-disk)
+ [Recovering your data from an inaccessible data center](#disaster-recovery)

## Recovering from an unexpected virtual machine shutdown<a name="recover-from-gateway-shutdown"></a>

If your VM shuts down unexpectedly, for example during a power outage, your gateway becomes unreachable\. When power and network connectivity are restored, your gateway becomes reachable and starts to function normally\. Following are some steps you can take at that point to help recover your data:
+ If an outage causes network connectivity issues, you can troubleshoot the issue\. For information about how to test network connectivity, see [Testing Your Gateway Connection to the Internet](manage-on-premises-common.md#MaintenanceTestGatewayConnectivity-common)\.
+  For tapes setups, when your gateway becomes reachable, your tapes go into BOOTSTRAPPING status\. This functionality ensures that your locally stored data continues to be synchronized with AWS\. For more information on this status, see [Understanding Tape Status](managing-gateway-vtl.md#understand-tapes-status)\.
+ If your gateway malfunctions and issues occur with your volumes or tapes as a result of an unexpected shutdown, you can recover your data\. For information about how to recover your data, see the sections following that apply to your scenario\.

## Recovering your data from a malfunctioning gateway or VM<a name="recover-from-gateway"></a>

If your Tape Gateway or the hypervisor host encounters an unrecoverable failure, you can use the following steps to recover the tapes from the malfunctioning Tape Gateway to another Tape Gateway:

1. Identify the Tape Gateway that you want to use as the recovery target, or create a new one\.

1. Disable the malfunctioning gateway\.

1. Create recovery tapes for each tape that you want to recover and specify the target Tape Gateway\.

1. Delete the malfunctioning Tape Gateway\.

For detailed information on how to recover the tapes from a malfunctioning Tape Gateway to another Tape Gateway, see [You Need to Recover a Virtual Tape from a Malfunctioning Tape Gateway](Main_TapesIssues-vtl.md#creating-recovery-tape-vtl)\.

## Recovering your data from an irrecoverable tape<a name="recover-from-tape"></a>

If your tape encounters a failure and the status of the tape is IRRECOVERABLE, we recommend you use one of the following options to recover your data or resolve the failure depending on your situation:
+ If you need the data on the irrecoverable tape, you can recover the tape to a new gateway\.
+ If you don't need the data on the tape, and the tape has never been archived, you can simply delete the tape from your Tape Gateway\.

   For detailed information about how to recover your data or resolve the failure if your tape is IRRECOVERABLE, see [Troubleshooting Irrecoverable Tapes](Main_TapesIssues-vtl.md#IrrecoverableTapes)\.

## Recovering your data from a malfunctioning cache disk<a name="recover-from-cahe-disk"></a>

If your cache disk encounters a failure, we recommend you use the following steps to recover your data depending on your situation:
+ If the malfunction occurred because a cache disk was removed from your host, shut down the gateway, re\-add the disk, and restart the gateway\.
+ If the cache disk is corrupted or not accessible, shut down the gateway, reset the cache disk, reconfigure the disk for cache storage, and restart the gateway\.

For detailed information, see [You Need to Recover a Virtual Tape from a Malfunctioning Cache Disk](Main_TapesIssues-vtl.md#recover-from-failed-disk)\.

## Recovering your data from an inaccessible data center<a name="disaster-recovery"></a>

If your gateway or data center becomes inaccessible for some reason, you can recover your data to another gateway in a different data center or recover to a gateway hosted on an Amazon EC2 instance\. If you don't have access to another data center, we recommend creating the gateway on an Amazon EC2 instance\. The steps you follow depends on the gateway type you are covering the data from\.

**To recover data from a Tape Gateway in an inaccessible data center**

1. Create and activate a new Tape Gateway on an Amazon EC2 host\. For more information, see [Deploying an Amazon EC2 instance to host your Tape Gateway](ec2-gateway-common.md)\.

1. Recover the tapes from the source gateway in the data center to the new gateway you created on Amazon EC2 For more information, see [Recovering a Virtual Tape From An Unrecoverable Gateway](Main_TapesIssues-vtl.md#recovery-tapes)\.

   Your tapes should be covered to the new Amazon EC2 gateway\.