--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

--------

# Deleting Custom Tape Pools<a name="deleting-tape-pools-vtl"></a>

You can delete a custom tape pool only if there are no archived tapes in the pool, and there are no automatic tape creation policies attached to the pool\.

**To delete your custom tape pool**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Pools** to see the available pools\.

1. Select one or more tape pools to delete\.

   If the **Tape Count** for the tape pools that you want to delete is **0**, and if there are no automatic tape creation policies that reference the custom tape pool, you can delete the pools\.

1. Choose **Delete**\. The confirmation dialog box appears\.

1. Verify that you want to delete the specified tape pools, then type the word *delete* in the confirmation box and choose **Delete**\.
**Warning**  
This procedure permanently deletes the selected tape pools and can't be undone\.

   After the tape pools are deleted, they disappear from the tape library\.