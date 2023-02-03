--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

--------

# Deleting Tapes<a name="deleting-tapes-vtl"></a>

You can delete virtual tapes from your Tape Gateway by using the Storage Gateway console\.

**Note**  
If the tape you want to delete from your Tape Gateway has a status of RETRIEVED, you must first eject the tape using your backup application before deleting the tape\. For instructions on how to eject a tape using the Symantec NetBackup software, see [Archiving the Tape](https://docs.aws.amazon.com/storagegateway/latest/tgw/backup_netbackup-vtl.html#GettingStarted-archiving-tapes-vtl)\. After the tape is ejected, the tape status changes back to ARCHIVED\. You can then delete the tape\. 

Make copies of your data before you delete your tapes\. After you delete a tape, you can't get it back\.

**To delete a virtual tape**
**Warning**  
This procedure permanently deletes the selected virtual tape\.

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Tape Library > Tapes** to see your tapes\. By default, this list displays up to 1,000 tapes at a time, but the searches that you perform apply to all of your tapes\. You can use the search bar to find tapes that match a specific criteria, or to reduce the list to less than 1,000 tapes\. When your list contains 1,000 tapes or fewer, you can then sort your tapes in ascending or descending order by various properties\.

1. Select one or more tapes to delete\.

1. For **Actions** choose **Delete tape**\. The confirmation dialog box appears\.

1. Verify that you want to delete the specified tapes, then type the word *delete* in the confirmation box and choose **Delete**\.

After the tape is deleted, it disappears from the Tape Gateway\.