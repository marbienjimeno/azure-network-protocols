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
- Step 4: Issue a new IP address for the Windows 10 VM. Observe the resulting DHCP traffic in Wireshark. 
- Step 5: Perform the nslookup command to get the IP addresses of google.com and disney.com from a DNS server. Observe the dns traffic in Wireshark.
- Step 6: Filter for RDP traffic only. Observe and evaluate the RDP traffic in Wireshark. 

<h2>Actions and Observations</h2>

**Step 1: Create a Resource Group. Then, create a Windows 10 Virtual Machine (VM) and a Linux Ubuntu VM using the previously created Resource Group. We will observe the Virtual Network within the Network Watcher.**
<p>
We will start by creating a New Resource Group. From the Azure Portal home page, go to Resource groups. 
</p>

![go_to_resource_group](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/876d559a-49d9-4737-9aa9-46895ffa83ad)

<p>
  Click on Create resource group.
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/379195e6-d62d-4873-a41f-60c021d9b2ee)

<p>
  Enter a Resource group name. In this tutorial, we will use "RG-Azure-Basics". For Region, select the region that most applies. In this tutorial, we will select (US) West US 3. Click on Review + create.
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/9a36f04c-d1ad-4fae-80fa-6c557b788050)
<br/>
![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/239037ad-82eb-4682-a9f9-72c02e95b5a7)

<p>
  After the validation process has passed, click on Create.
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/ab087d88-27d3-484f-a95b-d743c863d20b)
<br/>
![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/ad59cff9-25be-4d1b-918e-59a165e16a5b)

<br />
<p>
Now that a Resource Group has been created, we will now create a Windows 10 VM. Enter "virtual machines" in the search bar and go to Virtual machines. 
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/1ae8aac9-6bbd-4e43-85e3-2ea082effc5a)

<p>
  Click on Create dropdown button and select Azure virtual machine. 
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/2625a52b-7313-4460-b4a7-85924b12be68)

<p>
For Resource group, select the RG-Azure-Basics resource group. For Virtual machine name, we will use "VM1". For Region, select the appropriate region. In this tutorial we will select "(US) West US 3". For Image, select the Windows 10 Pro version.
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/c1a1b5bb-8792-4957-ab48-3ddd89e22935)

<p>
For Size, select "Standard_E2s_v3 - 2 vcpus, 16 GiB memory ($91.98)" which is recommended by the image publisher. If the option does not appear on the bdropdown menu, click on See all sizes to find and select the proper option. Under Administrator account, enter a username. In this tutorial, we will use "labuser". Then enter a password that satisfies the requirements. Under Licensing, check the box saying "I confirm I have an eligible Windows 10/11 license with multi-tenant hosting rights." Click on Next: Disks >. 
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/541962b5-bab7-40bc-9717-a42325ccf4f2)

<p>
  We will keep the default settings for Disks. Click on Next : Networking >
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/bbfbd5fa-f66a-494e-b31c-47fdc69e72a1)

<p>
For Virtual network, a new VM1-vnet has been created. For Subnet, the default 10.0.0.0/24 subnet has been assigned. For Public IP, VM1-ip has been configured by default. Click on Review + create. 
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/b1e5af17-54d9-45e5-8364-0a30f5dd0740)
<br />
![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/3e3efa47-aac1-4ff0-bbfa-8df191590408)

<p>
After the final validation process has passed, click on Create. 
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/a11d1a31-94f5-49ea-b53e-7f97a56f9cf2)
<br />
![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/11c9e56c-054c-4846-8b75-917545af5e75)

<p>
  We will now wait until the Windows 10 VM installation is complete before moving to the next step. 
</p>

 ![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/727e04c9-f401-4588-a249-8533ee234af2)

<br/>
<p>
  After the Windows 10 VM has completed its installation, we will now install an Ubuntu VM. Click on Create another VM. 
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/fca009fa-de52-4b3b-9c52-6aa90010d6c0)

<p>
  For Resource group, select the previously created RG-Azure-Basics resource group. For Virtual machine name, we will use "VM2". For Region, select the same region used for the Windows 10 VM. In this tutorial, we used "(US) West US 3". For Image, select "Ubuntu Server 20.04 LTS - x64 Gen2".
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/2721265f-7a87-4bbc-8d41-e41a4844aa16)

<p>
  For Size, select "Standard_E2s_v3 - 2 vcpus, 16 GiB memory ($91.98)". For Authentication type, select Password. In this tutorial, we will use the same username and password as the one used in the Windows 10 VM. Click on Next : Disks >.
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/f4f6e6eb-e644-45e7-b638-41abb434f050)

<p>
  We will use the default Disks configuration for this VM. Click on Next : Networking >.
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/9569250e-3d65-430b-b4c8-28b94ac7070e)

<p>
  For Virtual network, select the VM1-vnet network used for the Windows 10 VM. Both our Windows and Ubuntu VMs will occupy the same subnet VM1-vnet. For Public IP, if none has been created, click on Create new and use the name "VM2-ip". Click on Review + create.
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/d1ecbe47-ee2b-406a-a128-8f9579182a84)
<br />
![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/527c5890-8f94-4e9a-ad78-cfff9b8a4af6)

<p>
  After the final validation process has passed, click on Create. 
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/ab1a3a6e-5c70-4a80-b5f2-e02388e0de1f)
<br />
![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/5ff9b16d-ad66-42db-b06a-6ae56e687ce2)

<p>
  Now we'll wait for our newly created Ubuntu VM to be deployed.
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/62ec77d2-d52a-484e-ba56-42f5fd4efa45)

<p>
  After the Ubuntu VM has been deployed, nagivate to Virtual Machines using the search bar.
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/a492fc1b-320c-4fdc-b702-3f9650409a6b)

<p>
  We have successfully created our Windows 10 and Ubuntu VMs. 
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/6d2bb951-0f5e-42ce-a122-00a112f4ccc4)

<p>
  In Resource groups, we see that a NetworkWatcherRG resource group has been automatically created. We will use this resource group to observe the Virtual Network made up of our VMs. 
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/460f81e5-964e-4d08-9907-0e8d1f3f3b2d)

<br/>

**Step 2: Use Remote Desktop to connect to the Windows 10 VM and install Wireshark on the VM. Perform ping commands and observe ICMP traffic using Wireshark. Configure the Ubuntu VM's Network Security Group to disable incoming ICMP traffic and observe the results in Wireshark.**
<p>
  Now, we will use Remote Desktop to access our Windows 10 VM. Navigate to the Remote Desktop by searching "Remote Desktop" in the host system's Windows searchbar. 
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/a6fc5ab5-29b5-4e74-bf7d-c286503930e1)

<p>
  We will need to enter the public IP address of our Windows 10 VM. To find the IP address, hover over Virtual machines from the Azure Portal home page and select VM1.
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/7770de2a-fa46-4bd4-b4e8-52e5819b8bd7)

<p>
  Copy and paste VM1's public IP address onto Remote Desktop Connection and click on Connect.
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/712f1f6d-d181-4f67-9d31-6d95575c1079)

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/87cd9af2-e646-474e-85fc-aded11ab51ef)

<p>
  To enter the credential we set up on Azure, click on More choices and select Use a different account. Then enter the username and password we assigned in Azure. Click OK.
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/9460fde7-1808-4dec-8728-12a7f85463ed)

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/5515df4a-d48d-4d9a-a587-3e0c4acc0243)

<p>
  A window will pop up warning that VM1 remote computer does not have proper certificates. Click on Yes to connect despite these certificate errors. 
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/80c5dd90-fa88-496f-950f-7bb1c6c3fc55)

<p>
  Our Windows 10 VM will now begin starting up. When the privacy settings window appears, flip the switches to No on all settings and click Accept. 
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/c88d3a50-9f85-4cb1-a926-4c7599b98358)

<p>
  We will need to install Wireshark to start inspecting network traffic. Go to the Microsoft Edge browser. When prompted to sign in into Microsoft Edge, click on Start without your data. Click on Continue without this data. Click on Confirm and continue. Finally, click on Confirm and start browsing.
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/1c4106f9-d2b9-4890-946b-bbdc630b0cbe)

<p>
  Search "wireshark download" on the browser search bar and go to the Wireshark - Download link.
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/ef6ec579-806c-4858-97ec-d05b4ec89b84)

<p>
  In the Wireshark website download page, click on Windows x64 Installer. Once the Wireshark download executive has downloaded, open the file to begin the setup wizard. 
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/b20933ba-ddca-4cc2-af10-770e8b8a9a0e)

<p>
  We will use the default options provided by the setup wizard. Once the installation has finished, navigate to the Wireshark app using the the Windows VM search bar. 
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/f86ee9ec-887b-4e16-82ff-b5571dd700af)

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/d1fcebb0-7739-4bcf-ac71-cf564963be5a)

<p>
  In Wireshark, double-click Ethernet to start observing the VM's network traffic. 
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/9f0a832c-ff11-4ba2-9319-45bb45b3daa4)

<p>
  We should begin to see the various network traffic occurring while we are in our remote desktop connection. We will start by filtering for only ICMP traffic by entering "icmp" in the display filter. 
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/3bd29b79-796d-4514-9f9a-65208e6ad5d0)

<p>
  We see that there is currently no ICMP traffic being sent through the virtual network. To prompt ICMP connections, we will use the ping command. Search "powershell" and open Powershell.
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/94478d26-c5a7-431b-8a73-7325eaf6ee11)

<p>
  We will start by pinging our Ubuntu VM using its private IP address. To find it, from the Azure Portal home page  , navigate to Virtual machines and select VM2. Identify our Ubuntu VM's private IP address.
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/86f1c451-62aa-43dd-a689-3f1f407e2254)

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/4ba278e8-eb68-4deb-8a78-028e76bbd364)

<p>
  After identifying our Ubuntu VM's private address, perform the ping command on this address using the Windows VM's Powershell. In this tutorial, we will enter the command "ping 10.0.0.5".
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/4db03148-fb91-445c-a14f-92170f1eb364)

<p>
  We see that the our Windows VM was successfully able to ping our Ubuntu VM. Inside Wireshark, we are able to observe the ICMP traffic between the two VMs. We see the four ICMP requests sent by our Windows VM and the four ICMP replies following each ICMP requests coming from our Ubuntu VM. With this, we are able to confirm network connectivity between the two VMs.
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/9bf04c03-60c8-45e5-9e80-fe80f93cb7d2)

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/18a3c511-60a2-4bad-9702-157dfc510075)

<p>
  Now, we will try pinging google.com from our Windows VM. Inside Powershell, enter the command "ping www.google.com -4". The "-4" flag will force the command to use google.com's IPv4 address.
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/b35a784d-9e98-4ce7-9c3c-06f33e0bd359)

<p>
  We see the the ping commands were successful. Inside Wireshark, we can observe the the ICMP requests of our Window's VM being sent to google.com's IPv4 address, as well as google.com's ICMP replies directly after each ICMP requests. With this, we can confirm network connectivity between our Windows VM and google.com.
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/18f4b54f-1be5-49db-8759-9149a5eb9510)

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/ecaf54e2-8130-42b3-a815-34fc3aa8ced5)

<p>
  Now, we will configure the Network Security Group of our Ubuntu VM to disable incoming ICMP traffic. We will start by sending continuous pings from our Windows VM to the Ubuntu VM. In Powershell, enter the command "ping 10.0.0.5 -t". The "-t" flag will tell the Windows VM to continuously send pings. 
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/fc976132-c49d-4d9f-8a0b-ce4ed391b5cb)

<p>
  Back in the Azure Portal home page, navigate to Network security groups and select VM2-nsg.
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/5f9586c2-8656-45d4-abe5-c8359e957fd3)

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/4782df30-bbe8-4182-b30f-95b2e383e24d)

<p>
  Under Settings, go to Inbound security rules and click on Add to add an inbound security rule.
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/6d4eee99-cfd6-4c63-8e2d-2454ebb81a84)

<p>
  For Protocol, select ICMP. For Action, select Deny. For Priority, enter "200" to place this security rule over other existing rules. For Name, we will name this security rule "Deny-Inbound-ICMP". Click Add. 
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/2c38f4ad-e3e5-4c17-8066-a33f1ad76369)

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/bc0b38df-6f55-46cf-a612-8db252072b5a)

<p>
  After the new security rule has been set, return to our Windows VM. We see that the continuous pings are now timing out. In Wireshark, we observe that the ICMP requests are still being sent to our Ubuntu VM but there is no longer any replies. We can confirm that the inbound security rule we set preventing inbound ICMP traffic to our Ubuntu VM is in effect. 
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/2b8a809c-87c1-4f1b-b730-fa1e44307bd6)

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/258aaccd-c6d7-4454-a289-bef1dbf50a0a)

<p>
  We will now edit the Deny-Inbound-ICMP rule to allow inbound ICMP traffic. Back in Azure, click on Deny-Inbound-ICMP. For Action, select Allow. Click Save. 
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/29ad6c79-2be8-47c7-86ac-e7bd0c60809d)

<p>
  Back in Powershell inside our Windows VM, we see that the pings are once again successful. In Wireshark, we can observe that our Ubuntu VM is now returning ICMP replies to our Windows VM's ICMP requests. We can confirm that the edit we made on our Ubuntu VM's security rule is in effect. Press Ctrl+C in Powershell to stop the continuous pings. 
</p>

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/ae26d927-df34-4936-9199-182d82eed3df)

![image](https://github.com/marbienjimeno/azure-network-protocols/assets/29347863/ee2f6241-fcda-4937-ba1e-9189595dd187)

<br/>

**Step 3: From the Windows 10 VM, use the ssh command to connect into the Ubuntu VM. Perform different commands on the ssh linux connection and observe the SSH traffic on Wireshark.**
<p>
  We will now observe SSH traffic between our VMs in Wireshark. To begin, enter "ssh" into the Wireshark display filter. We will then ssh into our Ubuntu VM from our Windows VM. In Powershell inside our Windows VM, enter "ssh labuser@10.0.0.5" and enter your password. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  In Wireshark, we begin to see SSH traffic betweem our VMs as we attempt to ssh into our Ubuntu VM. In Powershell, a warning might appear regarding the authenticity of the Ubuntu host. Enter "yes" to continue. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  We see that our hostname has changed to labuser@VM2, indicating that we have successfully connected into our Ubuntu VM. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Now, we will try some Linux commands through our SSH connection. Enter the following commands one by one: "id", "uname -a", and "ls -lasth". We can observe SSH traffic activity in Wireshark as we enter the commands. once finished, we can terminate the ssh connection by entering "exit" in the Powershell command line.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br/>

**Step 4: Issue a new IP address for the Windows 10 VM. Observe the resulting DHCP traffic in Wireshark.** 
<p>
  We will now observe for DCHP traffic. We will invoke DHCP traffic by attempting to issue a new IP address for our Windows WM. DHCP stands for "Domain Host Configuration Protocol" and is in charge of assigning IP addresses for all machines in our network. Enter "dhcp" into the Wireshark display filter. In Powershell, enter the command "ipconfig /renew" to prompt the DHCP to assign our Windows VM a new IP address. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  After running the command, we can observe network activity between our Windows VM and th DCHP server.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br/>

**Step 5: Perform the nslookup command to get the IP addresses of google.com and disney.com from a DNS server. Observe the dns traffic in Wireshark.**
<p>
  Now we will observe for DNS traffic. Enter "dns" into the Wireshark display filter. We see that Wireshark has already detected DNS traffic in our network. Clear the captured packets and Continue without Saving.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  The DNS server is in charge of storing the IP addresses of domain names. In Powershell, enter "nslookup www.google.com" to get the IP addresses of google.com from the DNS server. Also perform "nslookup www.disney.com" to get the IP adddress of disney.com. We successfully get the IP addresses of google.com and disney.com. In Wireshark, we can also observe the DNS traffic between the Windows VM and the DNS server.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br/>

**Step 6: Filter for RDP traffic only. Observe and evaluate the RDP traffic in Wireshark.**
<p>
  For our last step, we will observe for RDP traffic. Enter "tcp.port == 3389" into the Wireshark display filter to display RDP traffic. We see that the RDP traffic in our virtual network is active. This makes sense because we are employing RDP as we connect to our Windows 10 VM using Remote Desktop. 
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

**Conclusion**
<p>
  This concludes this tutorial on how to observe Azure network protocols. In this tutorial, we created two Azure VMs and observed various types of virtual network traffic using Wireshark. 
</p>
