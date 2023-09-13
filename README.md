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

**Create a Resource Group. Then, create a Windows 10 Virtual Machine (VM) and a Linux Ubuntu VM using the previously created Resource Group. We will observe the Virtual Network within the Network Watcher.**
<p>
We will start by creating a New Resource Group. From the Azure Portal home page, go to Resource groups. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>Click on Create resource group.</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>Enter a Resource group name. In this tutorial, we will use "RG-Azure-Basics". Click on Review + create.</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>After the validation process has passed, click on Create.</p><p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
<p>
Now that a Resource Group has been created, we will now create a Windows 10 VM. Enter "virtual machines" in the search bar and go to Virtual machines. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
