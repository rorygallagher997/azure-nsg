<p align="center">
<img src="https://github.com/user-attachments/assets/917c55dd-1c62-4ac1-a9a4-996a6db9f8b4" height="50%" width="50%"/>
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

- Windows 10 (22H2)
- Ubuntu Server 24.04

<h2>High-Level Steps</h2>

- Create Resources
- Observe IMCP Traffic
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic

<h2>Actions and Observations</h2>

<p>
<img src="https://github.com/user-attachments/assets/5b150c39-2247-4d32-988b-eca981c8a6f4" height="50%" width="50%"/>
</p>
<p>
First, we have to create a resource group in Azure.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/e21c786c-ccf4-4efd-81b9-72a4cebac686" height="50%" width="50%"/>
</p>
<p>
Create the first virtual machine, we'll be using Windows 10 Pro (22H2)
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/56951683-5aab-49ed-ac45-d48ca15cc4d0" height="50%" width="50%"/>
</p>
<p>
Create the second virtual machine using Ubuntu Server 24.04
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/fb540787-f3f8-4f22-bbce-be6630e5da70" height="50%" width="50%"/>
</p>
<p>
Go to VM1 (Windows machine we created) in Azure and copy its public IP address. Open Remote Desktop Connection and paste the IP. Log in with the credentials you assigned to it when it was created.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/1b6d6016-3864-44ad-8459-7cd30c774312" height="50%" width="50%"/>
</p>
<p>
In your browser, go to www.wireshark.org and download it.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/f4e9bc0d-0106-4c6b-8041-c03deb72ef9b" height="50%" width="50%"/>
</p>
<p>
Open Wireshark and click on the blue fin button in the top left corner to start capturing packets.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/844e0d7f-8b3a-4dad-a315-058f0a44d4bf" height="50%" width="50%"/>
</p>
<p>
Let's start by filtering traffic by ICMP. Type ICMP in the search bar on Wireshark. Right now there is no traffic.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/c6b4418d-81fe-4b8c-a0b7-141ec64276c3" height="50%" width="50%"/>
</p>
<p>
Go back to the Azure portal and copy the private IP address of VM2 (Linux machine). Open Powershell on VM1 and ping the Linux machine. Notice how we now have ICMP traffic on Wireshark.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/4aea7b3d-9dbb-4c73-8b2b-48384ea6db48" height="50%" width="50%"/>
</p>
<p>
Use -t with ping to have it ping the Linux machine continuously. We are now going to block ICMP traffic on VM2 and see what happens to the requests.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/e0a9b8c9-6249-4e94-bb78-fc9cd4e06608" height="50%" width="50%"/>
</p>
<p>
In the Azure portal, go to Network Security Groups and open VM2's NSG. Click settings and then "inbound security rules". We will now add a rule to block ICMP traffic.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/b23c6371-86a6-4f12-b77e-f684c7604b47" height="50%" width="50%"/>
</p>
<p>
Click "Add" to add a new rule. Select Any source, Any Destination, All(*) source ports and destination ports. Select ICMP as the protocol we want to block and make sure Deny is selected.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/491cb3c2-65f7-408f-9d32-bd7adfbfe502" height="50%" width="50%"/>
</p>
<p>
Notice how we can no longer ping VM2 and requests are timed out. 
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/6149b3dc-3a5c-481a-b82e-9c38454e5550" height="50%" width="50%"/>
</p>
<p>
Now we will examine SSH traffic. Right now there is currently none.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/8382c2b5-d392-4ee4-ba9b-0741750b2970" height="50%" width="50%"/>
</p>
<p>
Using Powershell on VM1, SSH into VM2 by using the command "ssh labuser@10.0.0.5". Labuser is the username we gave the Linux machine and 10.0.05 is its private IP address.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/d20e700e-5dfc-4fcd-8dfb-b539a28875e2" height="50%" width="50%"/>
</p>
<p>
Now that we've successfully logged into VM2 via SSH, Wireshark is capturing the traffic.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/0a227ee5-ff43-46d6-811e-b05b83c8bb2c" height="50%" width="50%"/>
</p>
<p>
Let's view DHCP traffic. Use the command "ipconfig /renew". 
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/fa1938a5-7f5e-4fe2-9799-93baaadf5900" height="50%" width="50%"/>
</p>
<p>
To view DNS traffic, use the command "nslookup www.google.com". We are just using Google as an example.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/fa1938a5-7f5e-4fe2-9799-93baaadf5900" height="50%" width="50%"/>
</p>
<p>
To view DNS traffic, use the command "nslookup www.google.com". We are just using Google as an example.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/104d427f-ad26-4c3f-8a0a-849079cb7af4" height="50%" width="50%"/>
</p>
<p>
To view Remote Desktop Protocol (RDP), search in Wireshark "tcp.port == 3389". Our current RDP connections will be on display.
</p>
<br />
