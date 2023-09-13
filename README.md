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
- Step 2: Use Remote Desktop to connect to the Windows 10 VM and install Wireshark on the VM. Perform ping commands and observe ICMP traffic using Wireshark. Configure the Ubuntu VM's Network Security Group to disable incoming ICMP traffic and observe the results in Wireshark.
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
  We will now wait until the Windows 10 VM installation is complete before moving to the next step. 
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
  For Virtual network, select the VM1-vnet network used for the Windows 10 VM. Both our Windows and Ubuntu VMs will occupy the same subnet VM1-vnet. For Public IP, if none has been created, click on Create new and use the name "VM2-ip". Click on Review + create.
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

**Step 2: Use Remote Desktop to connect to the Windows 10 VM and install Wireshark on the VM. Perform ping commands and observe ICMP traffic using Wireshark. Configure the Ubuntu VM's Network Security Group to disable incoming ICMP traffic and observe the results in Wireshark.**
<p>
  Now, we will use Remote Desktop to access our Windows 10 VM. Navigate to the Remote Desktop by searching "Remote Desktop" in the host system's Windows searchbar. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  We will need to enter the public IP address of our Windows 10 VM. To find the IP address, navigate to Virtual machines from the Azure Portal home page and select VM1.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  Copy and paste VM1's public IP address onto Remote Desktop Connection and click on Connect.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  To enter the credential we set up on Azure, click on More choices and select Use a different account. Then enter the username and password we assigned in Azure.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  A window will pop up warning that VM1 remote computer does not have proper certificates. Click on Yes to connect despite these certificate errors. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  Our Windows 10 VM will now begin starting up. When the privacy settings window appears, flip the switches to No on all settings and click Accept. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  We will need to install Wireshark to start inspecting network traffic. Go to the Microsoft Edge browser. When prompted to sign in into Microsoft Edge, click on Start without your data. Click on Continue without this data. Click on Confirm and continue. Finally, click on Confirm and start browsing.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  Search "wireshark download" on the browser search bar and go to the Wireshark - Download link.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  In the Wireshark website download page, click on Windows x64 Installer. Once the Wireshark download executive has downloaded, open the file to begin the setup wizard. 
</p>
<p>
  We will use the default options provided by the setup wizard. Once the installation has finished, navigate to the Wireshark app using the the Windows VM search bar. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  Double-click Ethernet to start observing the VM's network traffic. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  We should begin to see the various network traffic occurring while we are in our remote desktop connection. We will start by filtering for only ICMP traffic by entering "icmp" in the display filter. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  We see that there is currently no ICMP traffic being sent through the virtual network. To prompt ICMP connections, we will use the ping command. Search "powershell" and open Powershell.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  We will start by pinging our Ubuntu VM using its private IP address. To find it, from the Azure Portal home page  , navigate to Virtual machines and select VM2.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  After identifying our Ubuntu VM's private address, perform the ping command on this address using the Windows VM's Powershell. In this tutorial, we will enter the command "ping 10.0.0.5".
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  We see that the our Windows VM was successfully able to ping our Ubuntu VM. Inside Wireshark, we are able to observe the ICMP traffic between the two VMs. We see the four ICMP requests sent by our Windows VM and the four ICMP replies following each ICMP requests coming from our Ubuntu VM. With this, we are able to confirm network connectivity between the two VMs.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  Now, we will try pinging google.com from our Windows VM. Inside Powershell, enter the command "ping www.google.com -4". The "-4" flag will force the command to use google.com's IPv4 address.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  We see the the ping commands were successful. Inside Wireshark, we can observe the the ICMP requests of our Window's VM being sent to google.com's IPv4 address, as well as google.com's ICMP replies directly after each ICMP requests. With this, we can confirm network connectivity between our Windows VM and google.com.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Now, we will configure the Network Security Group of our Ubuntu VM to disable incoming ICMP traffic. We will start by sending continuous pings from our Windows VM to the Ubuntu VM. In Powershell, enter the command "ping 10.0.0.5 -t". The "-t" flag will tell the Windows VM to continuously send pings. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Back in the Azure Portal home page, navigate to Network security groups and select VM2-nsg.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Under Settings, go to Inbound security rules and click on Add to add an inbound security rule.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  For Protocol, select ICMP. For Action, select Deny. For Priority, enter "200" to place this security rule over other existing rules. For Name, we will name this security rule "Deny-Inbound-ICMP". Click Add. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  After the new security rule has been set, return to our Windows VM. We see that the continuous pings are now timing out. In Wireshark, we observe that the ICMP requests are still being sent to our Ubuntu VM but there is no longer any replies. We can confirm that the inbound security rule we set preventing inbound ICMP traffic to our Ubuntu VM is in effect. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  We will now edit the Deny-Inbound-ICMP rule to allow inbound ICMP traffic. Back in Azure, click on Deny-Inbound-ICMP. For Action, select Allow. Click Save. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Back in Powershell inside our Windows VM, we see that the pings are once again successful. In Wireshark, we can observe that our Ubuntu VM is now returning ICMP replies to our Windows VM's ICMP requests. We can confirm that the edit we made on our Ubuntu VM's security rule is in effect. Press Ctrl+C in Powershell to stop the continuous pings. 
</p>
<br/>

**Step 3: From the Windows 10 VM, use the ssh command to connect into the Ubuntu VM. Perform different commands on the ssh linux connection and observe the SSH traffic on Wireshark.**
<p>
  
</p>
