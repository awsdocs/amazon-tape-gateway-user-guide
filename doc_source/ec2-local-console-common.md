--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Amazon FSx File Gateway documentation has been moved to [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

--------

# Performing Tasks on the Amazon EC2 Local Console<a name="ec2-local-console-common"></a>

Some maintenance tasks require that you log in to the local console when running a gateway deployed on an Amazon EC2 instance\. This section describes how to log in to the local console and perform maintenance tasks\.

**Topics**
+ [Logging In to Your Amazon EC2 Gateway Local Console](#EC2_MaintenanceConsoleWindow-common)
+ [Routing your gateway deployed on EC2 through an HTTP proxy](#EC2_MaintenanceRoutingProxy-common)
+ [Testing your gateway's network connectivity](#EC2_MaintenanceTestGatewayConnectivity-common)
+ [Viewing your gateway system resource status](#EC2_system-resource-check-common)
+ [Running Storage Gateway commands on the local console](#EC2_MaintenanceGatewayConsole-common)

## Logging In to Your Amazon EC2 Gateway Local Console<a name="EC2_MaintenanceConsoleWindow-common"></a>

You can connect to your Amazon EC2 instance by using a Secure Shell \(SSH\) client\. For detailed information, see [Connect to Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html) in the *Amazon EC2 User Guide*\. To connect this way, you will need the SSH key pair you specified when you launched the instance\. For information about Amazon EC2 key pairs, see [Amazon EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) in the *Amazon EC2 User Guide\.*<a name="EC2_MaintenanceConsoleWindowMenu-common"></a>

**To log in to the gateway local console**

1. Log in to your local console\. If you are connecting to your EC2 instance from a Windows computer, log in as *admin*\.

1. After you log in, you see the **AWS Storage Gateway \- Configuration** main menu, from which you can perform various tasks\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/tgw/ec2-local-console-common.html)

To shut down the gateway, enter **0**\.

To exit the configuration session, enter **X**\. 

## Routing your gateway deployed on EC2 through an HTTP proxy<a name="EC2_MaintenanceRoutingProxy-common"></a>

Storage Gateway supports the configuration of a Socket Secure version 5 \(SOCKS5\) proxy between your gateway deployed on Amazon EC2 and AWS\.

If your gateway must use a proxy server to communicate to the internet, then you need to configure the HTTP proxy settings for your gateway\. You do this by specifying an IP address and port number for the host running your proxy\. After you do so, Storage Gateway routes all AWS endpoint traffic through your proxy server\. Communications between the gateway and endpoints is encrypted, even when using the HTTP proxy\.

**To route your gateway internet traffic through a local proxy server**

1. Log in to your gateway's local console\. For instructions, see [Logging In to Your Amazon EC2 Gateway Local Console](#EC2_MaintenanceConsoleWindow-common)\.

1. From the **AWS Appliance Activation \- Configuration** main menu, enter the corresponding numeral to select **Configure HTTP Proxy**\.

1. From the **AWS Appliance Activation HTTP Proxy Configuration** menu, enter the corresponding numeral for the task you want to perform:
   + **Configure HTTP proxy** \- You will need to supply a host name and port to complete configuration\.
   + **View current HTTP proxy configuration** \- If an HTTP proxy is not configured, the message HTTP Proxy not configured is displayed\. If an HTTP proxy is configured, the host name and port of the proxy are displayed\.
   + **Remove an HTTP proxy configuration** \- The message HTTP Proxy Configuration Removed is displayed\.

## Testing your gateway's network connectivity<a name="EC2_MaintenanceTestGatewayConnectivity-common"></a>

You can use your gateway's local console to test your network connectivity\. This test can be useful when you are troubleshooting network issues with your gateway\.

**To test your gateway's connectivity**

1. Log in to your gateway's local console\. For instructions, see [Logging In to Your Amazon EC2 Gateway Local Console](#EC2_MaintenanceConsoleWindow-common)\.

1. From the **AWS Appliance Activation \- Configuration** main menu, enter the corresponding numeral to select **Test Network Connectivity**\.

   If your gateway has already been activated, the connectivity test begins immediately\. For gateways that have not yet been activated, you must specify the endpoint type and AWS Region as described in the following steps\.

1. If your gateway is not yet activated, enter the corresponding numeral to select the endpoint type for your gateway\.

1. If you selected the public endpoint type, enter the corresponding numeral to select the AWS Region that you want to test\. For supported AWS Regions and a list of AWS service endpoints you can use with Storage Gateway, see [AWS Storage Gateway endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/sg.html) in the *AWS General Reference*\.

As the test progresses, each endpoint displays either **\[PASSED\]** or **\[FAILED\]**, indicating the status of the connection as follows:


| Message | Description | 
| --- | --- | 
| \[PASSED\] | Storage Gateway has network connectivity\.  | 
| \[FAILED\] | Storage Gateway does not have network connectivity\.  | 

## Viewing your gateway system resource status<a name="EC2_system-resource-check-common"></a>

When your gateway starts, it checks its virtual CPU cores, root volume size, and RAM\. It then determines whether these system resources are sufficient for your gateway to function properly\. You can view the results of this check on the gateway's local console\.

**To view the status of a system resource check**

1. Log in to your gateway's local console\. For instructions, see [Logging In to Your Amazon EC2 Gateway Local Console](#EC2_MaintenanceConsoleWindow-common)\.

1. From the **AWS Appliance Activation \- Configuration** main menu, enter the corresponding numeral to select **View System Resource Check**\.

   Each resource displays **\[OK**\], **\[WARNING\]**, or **\[FAIL\]**, indicating the status of the resource as follows:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/tgw/ec2-local-console-common.html)

   The console also displays the number of errors and warnings next to the resource check menu option\.

## Running Storage Gateway commands on the local console<a name="EC2_MaintenanceGatewayConsole-common"></a>

The AWS Storage Gateway console helps provide a secure environment for configuring and diagnosing issues with your gateway\. Using the console commands, you can perform maintenance tasks such as saving routing tables or connecting to AWS Support\. 

**To run a configuration or diagnostic command**

1. Log in to your gateway's local console\. For instructions, see [Logging In to Your Amazon EC2 Gateway Local Console](#EC2_MaintenanceConsoleWindow-common)\.

1. From the **AWS Appliance Activation \- Configuration** main menu, enter the corresponding numeral to select **Gateway Console**\.

1. From the gateway console command prompt, enter **h**\.

   The console displays the **AVAILABLE COMMANDS** menu, which lists the available commands:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/storagegateway/latest/tgw/ec2-local-console-common.html)

1. From the gateway console command prompt, enter the corresponding command for the function you want to use, and follow the instructions\.

To learn about a command, enter **man** \+ *command name* at the command prompt\.