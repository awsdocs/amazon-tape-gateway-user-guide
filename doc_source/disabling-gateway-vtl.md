--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

--------

# Disabling Your Tape Gateway<a name="disabling-gateway-vtl"></a>

You disable a Tape Gateway if the Tape Gateway has failed and you want to recover the tapes from the failed gateway to another gateway\. 

To recover the tapes, you must first disable the failed gateway\. Disabling a Tape Gateway locks down the virtual tapes in that gateway\. That is, any data that you might write to these tapes after disabling the gateway isn't sent to AWS\. You can only disable a gateway on the Storage Gateway console if the gateway is no longer connected to AWS\. If the gateway is connected to AWS, you can't disable the Tape Gateway\. 

You disable a Tape Gateway as part of data recovery\. For more information about recovering tapes, see [You Need to Recover a Virtual Tape from a Malfunctioning Tape Gateway](Main_TapesIssues-vtl.md#creating-recovery-tape-vtl)\. 

**To disable your gateway**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Gateways**, and then choose the failed gateway\.

1. Choose the **Details** tab for the gateway to display the disable gateway message\.

1. Choose **Create recovery tapes**\.

1. Choose **Disable gateway**\.