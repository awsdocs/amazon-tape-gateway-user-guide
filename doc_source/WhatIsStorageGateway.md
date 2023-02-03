--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

--------

# What is Tape Gateway?<a name="WhatIsStorageGateway"></a>

AWS Storage Gateway connects an on\-premises software appliance with cloud\-based storage to provide seamless integration with data security features between your on\-premises IT environment and the AWS storage infrastructure\. You can use the service to store data in the Amazon Web Services Cloud for scalable and cost\-effective storage that helps maintain data security\.

AWS Storage Gateway offers file\-based File Gateways \(Amazon S3 File and Amazon FSx File\), volume\-based \(Cached and Stored\), and tape\-based storage solutions\.

**Topics**
+ [Tape Gateway](#tape-gateway)
+ [Are you a first\-time Storage Gateway user?](#are-you-first-time-user)
+ [How Tape Gateway works \(architecture\)](StorageGatewayConcepts.md)
+ [Storage Gateway pricing](#Pricing)
+ [Plan your Storage Gateway deployment](#planning-gateway-deployment)

## Tape Gateway<a name="tape-gateway"></a>

**Tape Gateway** – A Tape Gateway provides cloud\-backed virtual tape storage\.

With a Tape Gateway, you can cost\-effectively and durably archive backup data in S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive\. A Tape Gateway provides a virtual tape infrastructure that scales seamlessly with your business needs and eliminates the operational burden of provisioning, scaling, and maintaining a physical tape infrastructure\.

You can deploy Storage Gateway either on\-premises as a VM appliance running on VMware ESXi, KVM, or Microsoft Hyper\-V hypervisor, as a hardware appliance, or in AWS as an Amazon EC2 instance\. You deploy your gateway on an EC2 instance to provision iSCSI storage volumes in AWS\. You can use gateways hosted on EC2 instances for disaster recovery, data mirroring, and providing storage for applications hosted on Amazon EC2\.

For an architectural overview, see [How Tape Gateway works \(architecture\)](StorageGatewayConcepts.md)\. To see the wide range of use cases that AWS Storage Gateway helps make possible, see [AWS Storage Gateway](http://aws.amazon.com/storagegateway)\.

**Documentation**: For Tape Gateway documentation, see [Creating a Tape Gateway](create-tape-gateway.md)\.

## Are you a first\-time Storage Gateway user?<a name="are-you-first-time-user"></a>

In the following documentation, you can find a Getting Started section that covers setup information common to all gateways and also gateway\-specific setup sections\. The Getting Started section shows you how to deploy, activate, and configure storage for a gateway\. The management section shows you how to manage your gateway and resources:
+ [Creating a Tape Gateway](create-tape-gateway.md) provides instructions on how to create and use a Tape Gateway\. It shows you how to back up data to virtual tapes and archive the tapes\.
+ [Managing Your Gateway](managing-gateway-common.md) describes how to perform management tasks for your gateway and its resources\.

In this guide, you can primarily find how to work with gateway operations by using the AWS Management Console\. If you want to perform these operations programmatically, see the *[AWS Storage Gateway API Reference](https://docs.aws.amazon.com/storagegateway/latest/APIReference/)\.* 

## Storage Gateway pricing<a name="Pricing"></a>

For current information about pricing, see [Pricing](http://aws.amazon.com/storagegateway/pricing) on the AWS Storage Gateway details page\.

## Plan your Storage Gateway deployment<a name="planning-gateway-deployment"></a>

By using the Storage Gateway software appliance, you can connect your existing on\-premises application infrastructure with scalable, cost\-effective AWS cloud storage that provides data security features\.

To deploy Storage Gateway, you first need to decide on the following two things:

1. **Your gateway type** – this guide covers the following gateway type:
   + **Tape Gateway** – If you are looking for a cost\-effective, durable, long\-term, offsite alternative for data archiving, deploy a Tape Gateway\. With its virtual tape library \(VTL\) interface, you can use your existing tape\-based backup software infrastructure to store data on virtual tape cartridges that you create\. For more information, see [Supported third\-party backup applications for a Tape Gateway](Requirements.md#requirements-backup-sw-for-vtl)\. When you archive tapes, you don't worry about managing tapes on your premises and arranging shipments of tapes offsite\. For an architectural overview, see [How Tape Gateway works \(architecture\)](https://docs.aws.amazon.com/storagegateway/latest/tgw/StorageGatewayConcepts.html)\.

1. **Hosting option** – You can run Storage Gateway either on\-premises as a VM appliance or hardware appliance, or in AWS as an Amazon EC2 instance\. For more information, see [Requirements](Requirements.md)\. If your data center goes offline and you don't have an available host, you can deploy a gateway on an EC2 instance\. Storage Gateway provides an Amazon Machine Image \(AMI\) that contains the gateway VM image\.

Additionally, as you configure a host to deploy a gateway software appliance, you need to allocate sufficient storage for the gateway VM\.

Before you continue to the next step, make sure that you have done the following:

1. For a gateway deployed on\-premises, choose the type of VM host and set it up\. Your options are VMware ESXi Hypervisor, Microsoft Hyper\-V, and Linux Kernel\-based Virtual Machine \(KVM\)\. If you deploy the gateway behind a firewall, make sure that ports are accessible to the gateway VM\. For more information, see [Requirements](Requirements.md)\.

1. Install your client backup software\. For more information, see [Supported third\-party backup applications for a Tape Gateway](Requirements.md#requirements-backup-sw-for-vtl)\.