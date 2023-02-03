--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

--------

# Requirements<a name="Requirements"></a>

Unless otherwise noted, the following requirements are common to all gateway configurations\.

**Topics**
+ [Hardware and storage requirements](#requirements-hardware-storage)
+ [Network and firewall requirements](#networks)
+ [Supported hypervisors and host requirements](#requirements-host)
+ [Supported iSCSI initiators](#requirements-iscsi-initiators)
+ [Supported third\-party backup applications for a Tape Gateway](#requirements-backup-sw-for-vtl)

## Hardware and storage requirements<a name="requirements-hardware-storage"></a>

This section describes the minimum hardware and settings for your gateway and the minimum amount of disk space to allocate for the required storage\.

### Hardware requirements for on\-premises VMs<a name="requirements-hardware"></a>

When deploying your gateway on\-premises, you must make sure that the underlying hardware on which you deploy the gateway VM can dedicate the following minimum resources:
+ Four virtual processors assigned to the VM\.
+ 16 GiB of reserved RAM for File Gateways

  For Tape Gateway, your hardware should dedicate the following amounts of RAM:
  + 16 GiB of reserved RAM for gateways with cache size up to 16 TiB
  + 32 GiB of reserved RAM for gateways with cache size 16 TiB to 32 TiB
  + 48 GiB of reserved RAM for gateways with cache size 32 TiB to 64 TiB
+ 80 GiB of disk space for installation of VM image and system data\.

For more information, see [Optimizing Gateway Performance](Performance.md#Optimizing-common)\. For information about how your hardware affects the performance of the gateway VM, see [AWS Storage Gateway quotas](resource-gateway-limits.md)\. 

### Requirements for Amazon EC2 instance types<a name="requirements-hardware-ec2"></a>

When deploying your gateway on Amazon Elastic Compute Cloud \(Amazon EC2\), the instance size must be at least **xlarge** for your gateway to function\. However, for the compute\-optimized instance family the size must be at least **2xlarge**\. Use one of the following instance types recommended for your gateway type\.

**Recommended for cached volumes and Tape Gateway types**
+ General\-purpose instance family – **m4, m5, or m6** instance type\.
**Note**  
We don't recommend using the **m4\.16xlarge** instance type\.
+ Compute\-optimized instance family – **c4, c5, or c6** instance types\. Choose the **2xlarge** instance size or higher to meet the required RAM requirements\.
+ Memory\-optimized instance family – **r3, r5, or r6** instance types\.
+ Storage\-optimized instance family – **i3 or i4** instance types\.

### <a name="appliance-hardware-requirements"></a>

### Storage requirements<a name="requirements-storage"></a>

In addition to 80 GiB disk space for the VM, you also need additional disks for your gateway\.

The following table recommends sizes for local disk storage for your deployed gateway\. 


| Gateway Type | Cache \(Minimum\) | Cache \(Maximum\) | Upload Buffer \(Minimum\) | Upload Buffer \(Maximum\) | Other Required Local Disks | 
| --- | --- | --- | --- | --- | --- | 
| Tape Gateway | 150 GiB | 64 TiB | 150 GiB | 2 TiB | — | 

**Note**  
You can configure one or more local drives for your cache and upload buffer, up to the maximum capacity\.  
When adding cache or upload buffer to an existing gateway, it's important to create new disks in your host \(hypervisor or Amazon EC2 instance\)\. Don't change the size of existing disks if the disks have been previously allocated as either a cache or upload buffer\.

For information about gateway quotas, see [AWS Storage Gateway quotas](resource-gateway-limits.md)\.

## Network and firewall requirements<a name="networks"></a>

Your gateway requires access to the internet, local networks, Domain Name Service \(DNS\) servers, firewalls, routers, and so on\. Following, you can find information about required ports and how to allow access through firewalls and routers\.

**Note**  
In some cases, you might deploy Storage Gateway on Amazon EC2 or use other types of deployment \(including on\-premises\) with network security policies that restrict AWS IP address ranges\. In these cases, your gateway might experience service connectivity issues when the AWS IP range values changes\. The AWS IP address range values that you need to use are in the Amazon service subset for the AWS Region that you activate your gateway in\. For the current IP range values, see [AWS IP address ranges](https://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html) in the *AWS General Reference*\.

**Note**  
Network bandwidth requirements vary based on the quantity of data that is uploaded and downloaded by the gateway\. A minimum of 100Mbps is required to successfully download, activate, and update the gateway\. Your data transfer patterns will determine the bandwidth necessary to support your workload\. In some cases, you might deploy Storage Gateway on Amazon EC2 or use other types of deployment

**Topics**
+ [Port requirements](#requirements-network)
+ [Networking and firewall requirements for the Storage Gateway Hardware Appliance](#appliance-network-requirements)
+ [Allowing AWS Storage Gateway access through firewalls and routers](#allow-firewall-gateway-access)
+ [Configuring security groups for your Amazon EC2 gateway instance](#EC2GatewayCustomSecurityGroup-common)

### Port requirements<a name="requirements-network"></a>

Storage Gateway requires certain ports to be allowed for its operation\. The following illustrations show the required ports that you must allow for each type of gateway\. Some ports are required by all gateway types, and others are required by specific gateway types\. For more information about port requirements, see [Port Requirements](Resource_Ports.md)\.



**Common ports for all gateway types**

The following ports are common to all gateway types and are required by all gateway types\.


|  Protocol  |  Port  |  Direction  |  Source  |  Destination  |  How Used  | 
| --- | --- | --- | --- | --- | --- | 
|  TCP  |  443 \(HTTPS\)  |  Outbound  |  Storage Gateway  |  AWS  |  For communication from Storage Gateway to the AWS service endpoint\. For information about service endpoints, see [Allowing AWS Storage Gateway access through firewalls and routers](#allow-firewall-gateway-access)\.  | 
|  TCP  |  80 \(HTTP\)  |  Inbound  |  The host from which you connect to the AWS Management Console\.  |  Storage Gateway  |  By local systems to obtain the Storage Gateway activation key\. Port 80 is only used during activation of the Storage Gateway appliance\.  Storage Gateway does not require port 80 to be publicly accessible\. The required level of access to port 80 depends on your network configuration\. If you activate your gateway from the Storage Gateway Management Console, the host from which you connect to the console must have access to your gateway’s port 80\.  | 
|  UDP/UDP  |  53 \(DNS\)  |  Outbound  |  Storage Gateway  |  Domain Name Service \(DNS\) server  |  For communication between Storage Gateway and the DNS server\.  | 
|  TCP  |  22 \(Support channel\)  |  Outbound  |  Storage Gateway  |  AWS Support  |  Allows AWS Support to access your gateway to help you with troubleshooting gateway issues\. You don't need this port open for the normal operation of your gateway, but it is required for troubleshooting\.  | 
|  UDP  |  123 \(NTP\)  |  Outbound  |  NTP client  |  NTP server  |  Used by local systems to synchronize VM time to the host time\.   | 

**Ports for volume and Tape Gateways**

The following illustration shows the ports to open for Tape Gateway\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/tgw/images/SGWNetworkPorts16-volume-tape2.png)

In addition to the common ports, Tape Gateway requires the following port\.


|  Protocol  |  Port  |  Direction  |  Source  |  Destination  |  How Used  | 
| --- | --- | --- | --- | --- | --- | 
|  TCP  |  3260 \(iSCSI\)  |  Inbound  |  iSCSI Initiators  |  Storage Gateway  |  By local systems to connect to iSCSI targets exposed by the gateway\.   | 

For detailed information about port requirements, see [Port Requirements](Resource_Ports.md) in the *Additional Storage Gateway resources* section\.

### Networking and firewall requirements for the Storage Gateway Hardware Appliance<a name="appliance-network-requirements"></a>

Each Storage Gateway Hardware Appliance requires the following network services:
+ **Internet access** – an always\-on network connection to the internet through any network interface on the server\.
+ **DNS services** – DNS services for communication between the hardware appliance and DNS server\.
+ **Time synchronization** – an automatically configured Amazon NTP time service must be reachable\.
+ **IP address** – A DHCP or static IPv4 address assigned\. You cannot assign an IPv6 address\.

There are five physical network ports at the rear of the Dell PowerEdge R640 server\. From left to right \(facing the back of the server\) these ports are as follows:

1. iDRAC

1. `em1`

1. `em2`

1. `em3`

1. `em4`

You can use the iDRAC port for remote server management\.



![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/tgw/images/ApplianceFirewallRules.png)

A hardware appliance requires the following ports to operate\.


|  Protocol  |  Port  |  Direction  |  Source  |  Destination  |  How Used  | 
| --- | --- | --- | --- | --- | --- | 
| SSH |  22  |  Outbound  | Hardware appliance |  `54.201.223.107`  | Support channel | 
| DNS | 53 | Outbound | Hardware appliance | DNS servers | Name resolution | 
| UDP/NTP | 123 | Outbound | Hardware appliance | \*\.amazon\.pool\.ntp\.org | Time synchronization | 
| HTTPS |  443  |  Outbound  | Hardware appliance |  `*.amazonaws.com`  |  Data transfer  | 
| HTTP | 8080 | Inbound | AWS | Hardware appliance | Activation \(only briefly\) | 

To perform as designed, a hardware appliance requires network and firewall settings as follows:
+ Configure all connected network interfaces in the hardware console\.
+ Make sure that each network interface is on a unique subnet\.
+ Provide all connected network interfaces with outbound access to the endpoints listed in the diagram preceding\.
+ Configure at least one network interface to support the hardware appliance\. For more information, see [Configuring network parameters](appliance-configure-network.md)\.

**Note**  
For an illustration showing the back of the server with its ports, see [Rack\-mounting your hardware appliance and connecting it to power](appliance-rack-mount.md)

All IP addresses on the same network interface \(NIC\), whether for a gateway or a host, must be on the same subnet\. The following illustration shows the addressing scheme\.



![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/tgw/images/ApplianceAddressing.png)

For more information on activating and configuring a hardware appliance, see [Using the Storage Gateway Hardware Appliance](hardware-appliance.md)\.

### Allowing AWS Storage Gateway access through firewalls and routers<a name="allow-firewall-gateway-access"></a>

Your gateway requires access to the following service endpoints to communicate with AWS\. If you use a firewall or router to filter or limit network traffic, you must configure your firewall and router to allow these service endpoints for outbound communication to AWS\.

**Note**  
If you configure private VPC endpoints for your Storage Gateway to use for connection and data transfer to and from AWS, your gateway does not require access to the public internet\. For more information, see [Activating a gateway in a virtual private cloud](https://docs.aws.amazon.com/storagegateway/latest/tgw/gateway-private-link.html)\.

**Important**  
Depending on your gateway's AWS Region, replace *region* in the service endpoint with the correct region string\.

The following service endpoint is required by all gateways for head\-bucket operations\.

```
s3.amazonaws.com:443
```

The following service endpoints are required by all gateways for control path \(anon\-cp, client\-cp, proxy\-app\) and data path \(dp\-1\) operations\.

```
anon-cp.storagegateway.region.amazonaws.com:443
client-cp.storagegateway.region.amazonaws.com:443
proxy-app.storagegateway.region.amazonaws.com:443
dp-1.storagegateway.region.amazonaws.com:443
```

The following gateway service endpoint is required to make API calls\.

```
storagegateway.region.amazonaws.com:443
```

The following example is a gateway service endpoint in the US West \(Oregon\) Region \(`us-west-2`\)\.

```
storagegateway.us-west-2.amazonaws.com:443
```

The Amazon S3 service endpoint, shown following, is used by File Gateways only\. A File Gateway requires this endpoint to access the S3 bucket that a file share maps to\.

```
bucketname.s3.region.amazonaws.com
```

The following example is an S3 service endpoint in the US East \(Ohio\) Region \(`us-east-2`\)\.

```
s3.us-east-2.amazonaws.com
```

**Note**  
If your gateway can't determine the AWS Region where your S3 bucket is located, this service endpoint defaults to `s3.us-east-1.amazonaws.com`\. We recommend that you allow access to the US East \(N\. Virginia\) Region \(`us-east-1`\) in addition to AWS Regions where your gateway is activated, and where your S3 bucket is located\.

The following are S3 service endpoints for AWS GovCloud \(US\) Regions\.

```
s3-fips-us-gov-west-1.amazonaws.com (AWS GovCloud (US-West) Region (FIPS))
s3-fips.us-gov-east-1.amazonaws.com (AWS GovCloud (US-East) Region (FIPS))
s3.us-gov-west-1.amazonaws.com (AWS GovCloud (US-West) Region (Standard))
s3.us-gov-east-1.amazonaws.com (AWS GovCloud (US-East) Region (Standard))
```

The following example is a FIPS service endpoint for an S3 bucket in the AWS GovCloud \(US\-West\) Region\.

```
bucket-name.s3-fips-us-gov-west-1.amazonaws.com
```

The Amazon CloudFront endpoint following is required for Storage Gateway to get the list of available AWS Regions\.

```
https://d4kdq0yaxexbo.cloudfront.net/
```

A Storage Gateway VM is configured to use the following NTP servers\.

```
0.amazon.pool.ntp.org
1.amazon.pool.ntp.org
2.amazon.pool.ntp.org
3.amazon.pool.ntp.org
```
+ Storage Gateway—For supported AWS Regions and a list of AWS service endpoints you can use with Storage Gateway, see [AWS Storage Gateway endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/sg.html) in the *AWS General Reference*\.
+ Storage Gateway Hardware Appliance—For supported AWS Regions you can use with the hardware appliance see [Storage Gateway hardware appliance regions](https://docs.aws.amazon.com/general/latest/gr/sg.html#sg-hardware-appliance) in the *AWS General Reference*\.



### Configuring security groups for your Amazon EC2 gateway instance<a name="EC2GatewayCustomSecurityGroup-common"></a>

A security group controls traffic to your Amazon EC2 gateway instance\. When you configure a security group, we recommend the following:
+ The security group should not allow incoming connections from the outside internet\. It should allow only instances within the gateway security group to communicate with the gateway\. If you need to allow instances to connect to the gateway from outside its security group, we recommend that you allow connections only on ports 3260 \(for iSCSI connections\) and 80 \(for activation\)\.
+ If you want to activate your gateway from an Amazon EC2 host outside the gateway security group, allow incoming connections on port 80 from the IP address of that host\. If you cannot determine the activating host's IP address, you can open port 80, activate your gateway, and then close access on port 80 after completing activation\.
+ Allow port 22 access only if you are using AWS Support for troubleshooting purposes\. For more information, see [You want AWS Support to help troubleshoot your EC2 gateway](troubleshooting-EC2-gateway-issues.md#EC2-EnableAWSSupportAccess)\.

In some cases, you might use an Amazon EC2 instance as an initiator \(that is, to connect to iSCSI targets on a gateway that you deployed on Amazon EC2\. In such a case, we recommend a two\-step approach:

1. You should launch the initiator instance in the same security group as your gateway\.

1. You should configure access so the initiator can communicate with your gateway\.

For information about the ports to open for your gateway, see [Port Requirements](Resource_Ports.md)\.

## Supported hypervisors and host requirements<a name="requirements-host"></a>

You can run Storage Gateway on\-premises as either a virtual machine \(VM\) appliance, or a physical hardware appliance, or in AWS as an Amazon EC2 instance\.

**Note**  
When a manufacturer ends general support for a hypervisor version, Storage Gateway also ends support for that hypervisor version\. For detailed information about support for specific versions of a hypervisor, see the manufacturer's documentation\.

Storage Gateway supports the following hypervisor versions and hosts:
+ VMware ESXi Hypervisor \(version 6\.5, 6\.7, or 7\.0\) – A free version of VMware is available on the [VMware website](http://www.vmware.com/products/vsphere-hypervisor/overview.html)\. For this setup, you also need a VMware vSphere client to connect to the host\.
+  Microsoft Hyper\-V Hypervisor \(version 2012 R2, 2016, 2019, or 2022\) – A free, standalone version of Hyper\-V is available at the [Microsoft Download Center](http://www.microsoft.com/en-us/search/Results.aspx?q=hyper-V&form=DLC)\. For this setup, you need a Microsoft Hyper\-V Manager on a Microsoft Windows client computer to connect to the host\.
+ Linux Kernel\-based Virtual Machine \(KVM\) – A free, open\-source virtualization technology\. KVM is included in all versions of Linux version 2\.6\.20 and newer\. Storage Gateway is tested and supported for the CentOS/RHEL 7\.7, Ubuntu 16\.04 LTS, and Ubuntu 18\.04 LTS distributions\. Any other modern Linux distribution may work, but function or performance is not guaranteed\. We recommend this option if you already have a KVM environment up and running and you are already familiar with how KVM works\.
+ Amazon EC2 instance – Storage Gateway provides an Amazon Machine Image \(AMI\) that contains the gateway VM image\. Only file, cached volume, and Tape Gateway types can be deployed on Amazon EC2\. For information about how to deploy a gateway on Amazon EC2, see [Deploying an Amazon EC2 instance to host your Tape Gateway](ec2-gateway-common.md)\.
+ Storage Gateway Hardware Appliance – Storage Gateway provides a physical hardware appliance as a on\-premises deployment option for locations with limited virtual machine infrastructure\.

**Note**  
Storage Gateway doesn’t support recovering a gateway from a VM that was created from a snapshot or clone of another gateway VM or from your Amazon EC2 AMI\. If your gateway VM malfunctions, activate a new gateway and recover your data to that gateway\. For more information, see [Recovering from an unexpected virtual machine shutdown](recover-data-from-gateway.md#recover-from-gateway-shutdown)\.  
Storage Gateway doesn’t support dynamic memory and virtual memory ballooning\.

## Supported iSCSI initiators<a name="requirements-iscsi-initiators"></a>

When you deploy a Tape Gateway, the gateway is preconfigured with one media changer and 10 tape drives\. These tape drives and the media changer are available to your existing client backup applications as iSCSI devices\.

To connect to these iSCSI devices, Storage Gateway supports the following iSCSI initiators:
+ Windows Server 2019
+ Windows Server 2016
+ Windows Server 2012 R2
+ Windows 10
+ Windows 8\.1
+  Red Hat Enterprise Linux 5
+  Red Hat Enterprise Linux 6
+  Red Hat Enterprise Linux 7
+  VMware ESX Initiator, which provides an alternative to using initiators in the guest operating systems of your VMs

**Important**  
Storage Gateway doesn't support Microsoft Multipath I/O \(MPIO\) from Windows clients\.  
Storage Gateway supports connecting multiple hosts to the same volume if the hosts coordinate access by using Windows Server Failover Clustering \(WSFC\)\. However, you can't connect multiple hosts to that same volume \(for example, sharing a nonclustered NTFS/ext4 file system\) without using WSFC\.

## Supported third\-party backup applications for a Tape Gateway<a name="requirements-backup-sw-for-vtl"></a>

You use a backup application to read, write, and manage tapes with a Tape Gateway\. The following third\-party backup applications are supported to work with Tape Gateways\.

The type of medium changer you choose depends on the backup application you plan to use\. The following table lists third\-party backup applications that have been tested and found to be compatible with Tape Gateways\. This table includes the medium changer type recommended for each backup application\.


| Backup Application | Medium Changer Type | 
| --- | --- | 
| Arcserve Backup | AWS\-Gateway\-VTL | 
| Bacula Enterprise V10\.x | AWS\-Gateway\-VTL or STK\-L700 | 
| Commvault V11 | STK\-L700 | 
| Dell EMC NetWorker 19\.5 | AWS\-Gateway\-VTL | 
| IBM Spectrum Protect v8\.1\.10 | IBM\-03584L32\-0402 | 
| Micro Focus \(HPE\) Data Protector 9 or 11\.x | AWS\-Gateway\-VTL | 
| Microsoft System Center 2012 R2 or 2016 Data Protection Manager | STK\-L700 | 
| NovaStor DataCenter/Network 6\.4 or 7\.1 | STK\-L700 | 
| Quest NetVault Backup 12\.4 or 13\.x | STK\-L700 | 
| Veeam Backup & Replication 11A | AWS\-Gateway\-VTL | 
| Veritas Backup Exec 2014 or 15 or 16 or 20 or 22\.x | AWS\-Gateway\-VTL | 
| Veritas Backup Exec 2012 Veritas has ended support for Backup Exec 2012\.  | STK\-L700 | 
| Veritas NetBackup Version 7\.x or 8\.x | AWS\-Gateway\-VTL | 

**Important**  
We highly recommend that you choose the medium changer that's listed for your backup application\. Other medium changers might not function properly\. You can choose a different medium changer after the gateway is activated\. For more information, see [Selecting a Medium Changer After Gateway Activation](https://docs.aws.amazon.com/storagegateway/latest/tgw/resource_vtl-devices.html#change-mediumchanger-vtl)\.