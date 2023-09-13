<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Step 1: Create a Resource Group. Then, create a Windows 10 Virtual Machine (VM) and a Linux Ubuntu VM using the previously created Resource Group. We will observe the Virtual Network within the Network Watcher. 
- Step 2: Use Remote Desktop to connect to the Windows 10 VM and install Wireshark on the VM. Perform ping commands and observe ICMP traffic using Wireshark.
- Step 3: From the Windows 10 VM, use the ssh command to connect into the Ubuntu VM. Perform different commands on the ssh linux connection and observe the SSH traffic on Wireshark.
- Step 4: Perform the nslookup command to get the IP addresses of google.com and disney.com from a DNS server. Observe the dns traffic in Wireshark.
- Step 5: Filter for RDP traffic only. Observe and evaluate the RDP traffic in Wireshark. 

<h2>Actions and Observations</h2>

**Step 1: Create a Resource Group. Then, create a Windows 10 Virtual Machine (VM) and a Linux Ubuntu VM using the previously created Resource Group. We will observe the Virtual Network within the Network Watcher.**
<p>
We will start by creating a New Resource Group. From the Azure Portal home page, go to Resource groups. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Click on Create resource group.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>Enter a Resource group name. In this tutorial, we will use "RG-Azure-Basics". Click on Review + create.</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  After the validation process has passed, click on Create.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
<p>
Now that a Resource Group has been created, we will now create a Windows 10 VM. Enter "virtual machines" in the search bar and go to Virtual machines. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Click on Create dropdown button and select Azure virtual machine. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
For Resource group, select the RG-Azure-Basics resource group. For Virtual machine name, we will use "VM1". For Region, select the appropriate region. In this tutorial we will select "(US) West US 3". For Image, select the Windows 10 Pro version.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
For Size, select "Standard_E2s_v3 - 2 vcpus, 16 GiB memory ($91.98)" which is recommended by the image publisher. If the option does not appear on the bdropdown menu, click on See all sizes to find and select the proper option. Under Administrator account, enter a username. In this tutorial, we will use "labuser". Then enter a password that satisfies the requirements. Under Licensing, check the box saying "I confirm I have an eligible Windows 10/11 license with multi-tenant hosting rights." Click on Next: Disks >. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  We will keep the default settings for Disks. Click on Next : Networking >
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
For Virtual network, a new VM1-vnet has been created. For Subnet, the default 10.0.0.0/24 subnet has been assigned. For Public IP, VM1-ip has been configured by default. Click on Review + create. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
After the final validation process has passed, click on Create. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  We will now ait until the Windows 10 VM installation is complete before moving to the next step. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<br/>
<p>
  After the Windows 10 VM has completed its installation, we will now install an Ubuntu VM. Click on Create another VM. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  For Resource group, select the previously created RG-Azure-Basics resource group. For Virtual machine name, we will use "VM2". For Region, select the same region used for the Windows 10 VM. In this tutorial, we used "(US) West US 3". For Image, select "Ubuntu Server 20.04 LTS - x64 Gen2".
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  For Authentication type, select Password. In this tutorial, we will use the same username and password as the one used in the Windows 10 VM. Click on Next : Disks >.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  We will use the default Disks configuration for this VM. Click on Next : Networking >.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  For Virtual network, select the VM1-vnet network used for the Windows 10 VM. Both our Windows and Ubuntu VMs will occupy the same subnet. For Public IP, if none has been created, click on Create new and use the name "VM2-ip". Click on Review + create.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  After the final validation process has passed, click on Create. 
</p>
<p>
  Now we'll wait for our newly created Ubuntu VM to be deployed.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  After the Ubuntu VM has been deployed, nagivate to Virtual Machines using the search bar.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  We have successfully created our Windows 10 and Ubuntu VMs. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  We see that a NetworkWatcherRG resource group has been automatically created. We will use this resource group to observe the Virtual Network made up of our VMs. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<br/>

**Step 2: Use Remote Desktop to connect to the Windows 10 VM and install Wireshark on the VM. Perform ping commands and observe ICMP traffic using Wireshark.**

