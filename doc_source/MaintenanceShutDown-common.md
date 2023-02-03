--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

--------

# Shutting Down Your Gateway VM<a name="MaintenanceShutDown-common"></a>

You might need to shutdown or reboot your VM for maintenance, such as when applying a patch to your hypervisor\. Before you shutdown the VM, you must first stop the gateway\. For File Gateway, you just shutdown your VM\. Although this section focuses on starting and stopping your gateway using the Storage Gateway Management Console, you can also and stop your gateway by using your VM local console or Storage Gateway API\. When you power on your VM, remember to restart your gateway\. 

**Important**  
If you stop and start an Amazon EC2 gateway that uses ephemeral storage, the gateway will be permanently offline\. This happens because the physical storage disk is replaced\. There is no work\-around for this issue\. The only resolution is to delete the gateway and activate a new one on a new EC2 instance\.

**Note**  
If you stop your gateway while your backup software is writing or reading from a tape, the write or read task might not succeed\. Before you stop your gateway, you should check your backup software and the backup schedule for any tasks in progress\.
+ Gateway VM local console—see [Logging in to the Local Console Using Default Credentials](manage-on-premises-common.md#LocalConsole-login-common)\.
+ Storage Gateway API\-—see [ShutdownGateway](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_ShutdownGateway.html) 

For File Gateway, you simply shutdown your VM\. You don't shutdown the gateway\.

## Starting and Stopping a Tape Gateway<a name="start-stop-classic"></a><a name="PoweringOffGatewayConsole-common"></a>

**To stop a Tape Gateway**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Gateways**, and then choose the gateway to stop\. The status of the gateway is **Running**\.

1. For **Actions**, choose **Stop gateway** and verify the id of the gateway from the dialog box, and then choose **Stop gateway**\.

   While the gateway is stopping, you might see a message that indicates the status of the gateway\. When the gateway shuts down, a message and a **Start gateway** button appears in the **Details** tab\.

When you stop your gateway, the storage resources will not be accessible until you start your storage\. If the gateway was uploading data when it was stopped, the upload will resume when you start the gateway\.<a name="PoweringOnGatewayConsole-common"></a>

**To start a Tape Gateway**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Gateways** and then choose the gateway to start\. The status of the gateway is **Shutdown**\.

1. Choose **Details**\. and then choose **Start gateway**\.