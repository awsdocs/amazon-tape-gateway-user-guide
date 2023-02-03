--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

--------

# Performing Tasks on the VM Local Console<a name="manage-on-premises-common"></a>

For a gateway deployed on\-premises, you can perform the following maintenance tasks using the VM host's local console\. These tasks are common to VMware, Hyper\-V, and Linux Kernel\-based Virtual Machine \(KVM\) hosts\.

**Topics**
+ [Logging in to the Local Console Using Default Credentials](#LocalConsole-login-common)
+ [Setting the Local Console Password from the Storage Gateway Console](#set-password)
+ [Routing Your On\-Premises Gateway Through a Proxy](#MaintenanceRoutingProxy-common)
+ [Configuring Your Gateway Network](#MaintenanceConfiguringStaticIP-common)
+ [Testing Your Gateway Connection to the Internet](#MaintenanceTestGatewayConnectivity-common)
+ [Synchronizing Your Gateway VM Time](#MaintenanceTimeSync-common)
+ [Running Storage Gateway Commands on the Local Console](#MaintenanceGatewayConsole-common)
+ [Viewing Your Gateway System Resource Status](#system-resource-check-common)
+ [Configuring Network Adapters for Your Gateway](#NICConfiguring-common)

## Logging in to the Local Console Using Default Credentials<a name="LocalConsole-login-common"></a>

When the VM is ready for you to log in, the login screen is displayed\. If this is your first time logging in to the local console, you use the default sign\-in credentials to log in\. These default login credentials give you access to menus where you can configure gateway network settings and change the password from the local console\. Storage Gateway enables you to set your own password from the AWS Storage Gateway console instead of changing the password from the local console\. You don't need to know the default password to set a new password\. For more information, see [Setting the Local Console Password from the Storage Gateway Console](#set-password)\.<a name="MaintenanceConsoleWindowMenu-common"></a>

**To log in to the gateway's local console**

1. If this is your first time logging in to the local console, log in to the VM with the default credentials\. The default user name is `admin` and the password is `password`\.

   Otherwise, use your credentials to log in\.
**Note**  
We recommend changing the default password by entering the corresponding numeral for **Gateway Console** from the **AWS Appliance Activation \- Configuration** main menu, then running the `passwd` command\. For information about how to run the command, see [Running Storage Gateway Commands on the Local Console](#MaintenanceGatewayConsole-common)\. You can also set your own password from the AWS Storage Gateway console\. For more information, see [Setting the Local Console Password from the Storage Gateway Console](#set-password)\.
**Important**  
For older versions of the volume or Tape Gateway, the user name is `sguser` and the password is `sgpassword`\. If you reset your password and your gateway is updated to a newer version, your the user name will change to admin but the password will be maintained\.

1. After you log in, you see the **AWS Storage Gateway Configuration** main menu, from which you can perform various tasks\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/tgw/manage-on-premises-common.html)

To shut down the gateway, enter **0**\.

To exit the configuration session, enter **X**\. 

## Setting the Local Console Password from the Storage Gateway Console<a name="set-password"></a>

When you log in to the local console for the first time, you log in to the VM with the default credentials— The user name is `admin` and the password is `password`\. We recommend that you always set a new password immediately after you create your new gateway\. You can set this password from the AWS Storage Gateway console rather than the local console if you want\. You don't need to know the default password to set a new password\.

**To set the local console password on the Storage Gateway console**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. On the navigation pane, choose **Gateways** then choose the gateway for which you want to set a new password\.

1. For **Actions**, choose **Set Local Console Password**\.

1. In the **Set Local Console Password** dialog box, type a new password, confirm the password and then choose **Save**\. Your new password replaces the default password\. Storage Gateway does not save the password but rather safely transmits it to the VM\.
**Note**  
The password can consist of any character on the keyboard and can be 1 to 512 characters long\.

## Routing Your On\-Premises Gateway Through a Proxy<a name="MaintenanceRoutingProxy-common"></a>

Volume Gateways and Tape Gateways support configuration of a Socket Secure version 5 \(SOCKS5\) proxy between your on\-premises gateway and AWS\. 

**Note**  
The only supported proxy configuration is SOCKS5\.

If your gateway must use a proxy server to communicate to the internet, then you need to configure the SOCKS proxy settings for your gateway\. You do this by specifying an IP address and port number for the host running your proxy\. After you do so, Storage Gateway routes all traffic through your proxy server\. For information about network requirements for your gateway, see [Network and firewall requirements](Requirements.md#networks)\.

The following procedure shows you how to configure SOCKS proxy for Volume Gateway and Tape Gateway\.<a name="socks-proxy"></a>

**To configure a SOCKS5 proxy for volume and Tape Gateways**

1. Log in to your gateway's local console\.
   + VMware ESXi – for more information, see [Accessing the Gateway Local Console with VMware ESXi](accessing-local-console.md#MaintenanceConsoleWindowVMware-common)\.
   + Microsoft Hyper\-V – for more information, see [Access the Gateway Local Console with Microsoft Hyper\-V](accessing-local-console.md#MaintenanceConsoleWindowHyperV-common)\.
   + KVM – for more information, see [Accessing the Gateway Local Console with Linux KVM](accessing-local-console.md#MaintenanceConsoleWindowKVM-common)\.

1. From the **AWS Storage Gateway \- Configuration** main menu, enter the corresponding numeral to select **SOCKS Proxy Configuration**\.

1. From the **AWS Storage Gateway SOCKS Proxy Configuration** menu, enter the corresponding numeral to perform one of the following tasks:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/tgw/manage-on-premises-common.html)

1. Restart your VM to apply your HTTP configuration\.

## Configuring Your Gateway Network<a name="MaintenanceConfiguringStaticIP-common"></a>

The default network configuration for the gateway is Dynamic Host Configuration Protocol \(DHCP\)\. With DHCP, your gateway is automatically assigned an IP address\. In some cases, you might need to manually assign your gateway's IP as a static IP address, as described following\.

**To configure your gateway to use static IP addresses**

1. Log in to your gateway's local console\.
   + VMware ESXi – for more information, see [Accessing the Gateway Local Console with VMware ESXi](accessing-local-console.md#MaintenanceConsoleWindowVMware-common)\.
   + Microsoft Hyper\-V – for more information, see [Access the Gateway Local Console with Microsoft Hyper\-V](accessing-local-console.md#MaintenanceConsoleWindowHyperV-common)\.
   + KVM – for more information, see [Accessing the Gateway Local Console with Linux KVM](accessing-local-console.md#MaintenanceConsoleWindowKVM-common)\.

1. From the **AWS Storage Gateway \- Configuration** main menu, enter the corresponding numeral to select **Network Configuration**\.

1. From the **AWS Storage Gateway Network Configuration** menu, perform one of the following tasks:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/tgw/manage-on-premises-common.html)

## Testing Your Gateway Connection to the Internet<a name="MaintenanceTestGatewayConnectivity-common"></a>

You can use your gateway's local console to test your internet connection\. This test can be useful when you are troubleshooting network issues with your gateway\.

**To test your gateway's connection to the internet**

1. Log in to your gateway's local console\.
   + VMware ESXi – for more information, see [Accessing the Gateway Local Console with VMware ESXi](accessing-local-console.md#MaintenanceConsoleWindowVMware-common)\.
   + Microsoft Hyper\-V – for more information, see [Access the Gateway Local Console with Microsoft Hyper\-V](accessing-local-console.md#MaintenanceConsoleWindowHyperV-common)\.
   + KVM – for more information, see [Accessing the Gateway Local Console with Linux KVM](accessing-local-console.md#MaintenanceConsoleWindowKVM-common)\.

1. From the **AWS Storage Gateway \- Configuration** main menu, enter the corresponding numeral to select **Test Network Connectivity**\.

   If your gateway has already been activated, the connectivity test begins immediately\. For gateways that have not yet been activated, you must specify the endpoint type and AWS Region as described in the following steps\.

1. If your gateway is not yet activated, enter the corresponding numeral to select the endpoint type for your gateway\.

1. If you selected the public endpoint type, enter the corresponding numeral to select the AWS Region that you want to test\. For supported AWS Regions and a list of AWS service endpoints you can use with Storage Gateway, see [AWS Storage Gateway endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/sg.html) in the *AWS General Reference*\.

As the test progresses, each endpoint displays either **\[PASSED\]** or **\[FAILED\]**, indicating the status of the connection as follows:


| Message | Description | 
| --- | --- | 
| \[PASSED\] | Storage Gateway has network connectivity\.  | 
| \[FAILED\] | Storage Gateway does not have network connectivity\.  | 

## Synchronizing Your Gateway VM Time<a name="MaintenanceTimeSync-common"></a>

After your gateway is deployed and running, in some scenarios the gateway VM's time can drift\. For example, if there is a prolonged network outage and your hypervisor host and gateway do not get time updates, then the gateway VM's time will be different from the true time\. When there is a time drift, a discrepancy occurs between the stated times when operations such as snapshots occur and the actual times that the operations occur\.

For a gateway deployed on VMware ESXi, setting the hypervisor host time and synchronizing the VM time to the host is sufficient to avoid time drift\. For more information, see [Synchronizing VM Time with Host Time](configure-vmware.md#GettingStartedSyncVMTime-common)\. 

For a gateway deployed on Microsoft Hyper\-V, you should periodically check your VM's time\. For more information, see [Synchronizing Your Gateway VM Time](MaintenanceTimeSync-hyperv.md)\.

## Running Storage Gateway Commands on the Local Console<a name="MaintenanceGatewayConsole-common"></a>

The VM local console in Storage Gateway helps provide a secure environment for configuring and diagnosing issues with your gateway\. Using the local console commands, you can perform maintenance tasks such as saving routing tables, connecting to AWS Support, and so on\.

**To run a configuration or diagnostic command**

1. Log in to your gateway's local console:
   + For more information on logging in to the VMware ESXi local console, see [Accessing the Gateway Local Console with VMware ESXi](accessing-local-console.md#MaintenanceConsoleWindowVMware-common)\.
   + For more information on logging in to the Microsoft Hyper\-V local console, see [Access the Gateway Local Console with Microsoft Hyper\-V](accessing-local-console.md#MaintenanceConsoleWindowHyperV-common)\.
   + For more information on logging in to the KVM local console, see [Accessing the Gateway Local Console with Linux KVM](accessing-local-console.md#MaintenanceConsoleWindowKVM-common)\.

1. From the **AWS Appliance Activation \- Configuration** main menu, enter the corresponding numeral to select **Gateway Console**\.

1. From the gateway console command prompt, enter **h**\.

   The console displays the **AVAILABLE COMMANDS** menu, which lists the available commands:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/tgw/manage-on-premises-common.html)

1. From the gateway console command prompt, enter the corresponding command for the function you want to use, and follow the instructions\.

To learn about a command, enter **man** \+ *command name* at the command prompt\.

## Viewing Your Gateway System Resource Status<a name="system-resource-check-common"></a>

When your gateway starts, it checks its virtual CPU cores, root volume size, and RAM\. It then determines whether these system resources are sufficient for your gateway to function properly\. You can view the results of this check on the gateway's local console\.

**To view the status of a system resource check**

1. Log in to your gateway's local console:
   + For more information on logging in to the VMware ESXi console, see [Accessing the Gateway Local Console with VMware ESXi](accessing-local-console.md#MaintenanceConsoleWindowVMware-common)\.
   + For more information on logging in to the Microsoft Hyper\-V local console, see [Access the Gateway Local Console with Microsoft Hyper\-V](accessing-local-console.md#MaintenanceConsoleWindowHyperV-common)\.
   + For more information on logging in to the KVM local console, see [Accessing the Gateway Local Console with Linux KVM](accessing-local-console.md#MaintenanceConsoleWindowKVM-common)\.

1. From the **AWS Appliance Activation \- Configuration** main menu, enter the corresponding numeral to select **View System Resource Check**\.

   Each resource displays **\[OK**\], **\[WARNING\]**, or **\[FAIL\]**, indicating the status of the resource as follows:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/tgw/manage-on-premises-common.html)

   The console also displays the number of errors and warnings next to the resource check menu option\.

## Configuring Network Adapters for Your Gateway<a name="NICConfiguring-common"></a>

By default, Storage Gateway is configured to use the E1000 network adapter type, but you can reconfigure your gateway to use the VMXNET3 \(10 GbE\) network adapter\. You can also configure Storage Gateway so it can be accessed by more than one IP address\. You do this by configuring your gateway to use more than one network adapter\.

**Topics**
+ [Configuring Your Gateway to Use the VMXNET3 Network Adapter](#NICChanging-common)
+ [Configuring Your Gateway for Multiple NICs](#MaintenanceMultiNIC-common)

### Configuring Your Gateway to Use the VMXNET3 Network Adapter<a name="NICChanging-common"></a>

Storage Gateway supports the E1000 network adapter type in both VMware ESXi and Microsoft Hyper\-V hypervisor hosts\. However, the VMXNET3 \(10 GbE\) network adapter type is supported in VMware ESXi hypervisor only\. If your gateway is hosted on a VMware ESXi hypervisor, you can reconfigure your gateway to use the VMXNET3 \(10 GbE\) adapter type\. For more information on this adapter, see the [VMware website](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1001805)\.

**Important**  
To select VMXNET3, your guest operating system type must be **Other Linux64**\. 

Following are the steps you take to configure your gateway to use the VMXNET3 adapter: 

1. Remove the default E1000 adapter\.

1. Add the VMXNET3 adapter\.

1. Restart your gateway\.

1. Configure the adapter for the network\.

Details on how to perform each step follow\.

**To remove the default E1000 adapter and configure your gateway to use the VMXNET3 adapter**

1. In VMware, open the context \(right\-click\) menu for your gateway and choose **Edit Settings**\.

1. In the **Virtual Machine Properties** window, choose the **Hardware** tab\.

1. For **Hardware**, choose **Network adapter**\. Notice that the current adapter is E1000 in the **Adapter Type** section\. You will replace this adapter with the VMXNET3 adapter\.

      
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/storagegateway/latest/tgw/images/VerifyAdapterType.png)

1. Choose the E1000 network adapter, and then choose **Remove**\. In this example, the E1000 network adapter is **Network adapter 1**\. 
**Note**  
Although you can run the E1000 and VMXNET3 network adapters in your gateway at the same time, we don't recommend doing so because it can cause network problems\.

1. Choose **Add** to open the Add Hardware wizard\.

1. Choose **Ethernet Adapter**, and then choose **Next**\.

1. In the Network Type wizard, select **VMXNET3** for **Adapter Type**, and then choose **Next**\.

1. In the Virtual Machine properties wizard, verify in the **Adapter Type** section that **Current Adapter** is set to **VMXNET3**, and then choose **OK**\.

1. In the VMware VSphere client, shut down your gateway\.

1. In the VMware VSphere client, restart your gateway\.

After your gateway restarts, reconfigure the adapter you just added to make sure that network connectivity to the internet is established\. 

**To configure the adapter for the network**

1. In the VSphere client, choose the **Console** tab to start the local console\. Use the default login credentials to log in to the gateway's local console for this configuration task\. For information about how to log in using the default credentials, see [Logging in to the Local Console Using Default Credentials](https://docs.aws.amazon.com/storagegateway/latest/tgw/manage-on-premises-common.html#LocalConsole-login-common)\.

1. At the prompt, enter the corresponding numeral to select **Network Configuration**\.

1. At the prompt, enter the corresponding numeral to select **Reset all to DHCP**, and then enter **y** \(for yes\) at the prompt to set all adapters to use Dynamic Host Configuration Protocol \(DHCP\)\. All available adapters are set to use DHCP\.

   If your gateway is already activated, you must shut it down and restart it from the Storage Gateway Management Console\. After the gateway restarts, you must test network connectivity to the internet\. For information about how to test network connectivity, see [Testing Your Gateway Connection to the Internet](https://docs.aws.amazon.com/storagegateway/latest/tgw/manage-on-premises-common.html#MaintenanceTestGatewayConnectivity-common)\.

### Configuring Your Gateway for Multiple NICs<a name="MaintenanceMultiNIC-common"></a>

If you configure your gateway to use multiple network adapters \(NICs\), it can be accessed by more than one IP address\. You might want to do this in the following situations:

 
+ ****Maximizing throughput**** – You might want to maximize throughput to a gateway when network adapters are a bottleneck\.
+ ****Application separation**** – You might need to separate how your applications write to a gateway's volumes\. For example, you might choose to have a critical storage application exclusively use one particular adapter defined for your gateway\.
+ ****Network constraints**** – Your application environment might require that you keep your iSCSI targets and the initiators that connect to them in an isolated network that is different from the network by which the gateway communicates with AWS\.

In a typical multiple\-adapter use case, one adapter is configured as the route by which the gateway communicates with AWS \(that is, as the default gateway\)\. Except for this one adapter, initiators must be in the same subnet as the adapter that contains the iSCSI targets to which they connect\. Otherwise, communication with the intended targets might not be possible\. If a target is configured on the same adapter that is used for communication with AWS, then iSCSI traffic for that target and AWS traffic will flow through the same adapter\.

 When you configure one adapter to connect to the Storage Gateway console and then add a second adapter, Storage Gateway automatically configures the route table to use the second adapter as the preferred route\. For instructions on how to configure multiple\-adapters, see the following sections\. 
+ [Configuring Your Gateway for Multiple NICs in a VMware ESXi Host](configure-multi-nic.md#MaintenanceMultiNIC-vmaware)
+ [Configuring Your Gateway for Multiple NICs in Microsoft Hyper\-V Host](configure-multi-nic.md#MaintenanceMultiNIC-hyperv)