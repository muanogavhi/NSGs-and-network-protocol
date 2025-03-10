
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

Step 1: Create Our Resources

 Step 2: Observe ICMP Traffic

 Step 3: Observe SSH Traffic

 Step 4: Observe DHCP Traffic

Step 5: Observe DNS Traffic

Step 6: Observe RDP Traffic

Step 7: Resource Cleanup

<h2>Actions and Observations</h2>

<p>
<img width="763" alt="Screen Shot 2024-07-03 at 10 25 00 AM" src="https://github.com/Bpeduru/azure-network-protocols/assets/171273980/9bfa3c9d-ba9a-4d45-a9ca-2176deb61b7f">
</p>
<br />
<img width="789" alt="Screen Shot 2024-07-03 at 10 38 08 AM" src="https://github.com/Bpeduru/azure-network-protocols/assets/171273980/bceaa25d-e2b1-4186-a1db-49e520a501ac">
</p>
<br />

<img width="722" alt="Screen Shot 2024-07-03 at 10 37 20 AM" src="https://github.com/Bpeduru/azure-network-protocols/assets/171273980/545f7b01-9339-44e4-be23-96a8344b8431">
</p>
<p>
Start by creating a Resource Group. Next, set up a Windows 10 Virtual Machine (VM), ensuring to select the newly created Resource Group and allowing it to create a new Virtual Network (Vnet) and Subnet during the process. Then, create a Linux (Ubuntu) VM, also selecting the previously created Resource Group and Vnet.
</p>
<br />
<img width="1440" alt="Screen Shot 2024-07-03 at 11 02 47 AM" src="https://github.com/Bpeduru/azure-network-protocols/assets/171273980/56df9764-3c22-41f2-bb4b-d701610ba517">
</p>
<br />
<img width="516" alt="Screen Shot 2024-07-03 at 11 04 42 AM" src="https://github.com/Bpeduru/azure-network-protocols/assets/171273980/39636a05-6ba1-4248-aa1d-97600139edc7">
</p>
<br />
<p>
Use Remote Desktop to connect to the Windows virtual machine, and within it install Wireshark. To do this simply google Wireshark and click the first link thats comes up, then download the Windows x64 Installer. Click open file and follow the required steps to install the program. 
</p>
<p>
<img width="1365" alt="Screen Shot 2024-07-03 at 11 08 36 AM" src="https://github.com/Bpeduru/azure-network-protocols/assets/171273980/ba5264e6-7ff2-4bad-851d-95bda83b38ef">

</p>

Within Wireshark, filter for ICMP traffic only. To do this, type ICMP onto the bar at the top and hit enter. If you entered it correctly it should turtn green. Then attempt to Ping the Ubuntu virtual machine you perviously created. Observe what happens within wireshark. Then attempt to ping  a public website, I used amazon.com in the example above. Observe the traffic. (ICMP is the protocol that the ping command uses) 
<p>
<p>
<img width="599" alt="Screen Shot 2024-07-03 at 11 15 55 AM" src="https://github.com/Bpeduru/azure-network-protocols/assets/171273980/a93f9179-3103-4fcc-8991-096a5c26ddb8">

</p>
<p>
<img width="1438" alt="Screen Shot 2024-07-03 at 11 17 23 AM" src="https://github.com/Bpeduru/azure-network-protocols/assets/171273980/7ce2ac24-f888-47c5-b120-e3d2c548bde2">
</p>
<br />
  
Start a perpertual ping to the Ubuntu VM with the ping -t command. Then add an inbound secrutiy rule within Azure that denies all ICMP traffic. Obsersve that the VM stops responding when the rule is in effect. Why is this? Hit ctrl-c to stop the ping. 
</p>
<br />
<img width="1440" alt="Screen Shot 2024-07-03 at 11 25 45 AM" src="https://github.com/Bpeduru/azure-network-protocols/assets/171273980/f8183b86-fd9b-4747-a3e1-35a2c6ab6b13">

</p>
<br />

<img width="563" alt="Screen Shot 2024-07-03 at 11 28 41 AM" src="https://github.com/Bpeduru/azure-network-protocols/assets/171273980/ec2fe629-dd3d-409c-ae2d-4dbc15921521">
</p>
<br />  
Now Filter for ssh traffic within Wireshark. Use the ssh command to connect to the virtual machine so you can initiate commands within its command line. To do this enter ssh (yourubunutusername)@(YourUbuntuipaddress). For my example it would be ssh labuser@10.1.0.5. Observe the traffic within Wireshark. Now enter different Ubuntu commands (Username,pwd,id, etc.)Observe the traffic within wireshark as you enter each command. To exit the connection, type exit and hit enter. 
</p>
<br />
<img width="1325" alt="Screen Shot 2024-07-09 at 1 54 13 PM" src="https://github.com/Bpeduru/azure-network-protocols/assets/171273980/5dfc4499-bf1e-40a4-8d61-09a13247b7ac">

</p>
<br />  
Now filter for DHCP traffic. Enter the command ipconfig /renew and observe the traffic
</p>
<br />
<img width="1440" alt="Screen Shot 2024-07-03 at 11 38 03 AM" src="https://github.com/Bpeduru/azure-network-protocols/assets/171273980/af875795-4c34-4ce4-929f-421a4e049169">
</p>
<br />
Now filter for DNS traffic. Use the command nslookup (Domain name) to lookup the IP address of different websites. I used Google.com, Amazon.com and Disney.com. Observe the traffic in Wireshark
</p>
<br />
<img width="1440" alt="Screen Shot 2024-07-03 at 11 36 32 AM" src="https://github.com/Bpeduru/azure-network-protocols/assets/171273980/66ac63a7-ecb6-49b9-b707-3ae226e7ae7a">
</p>
<br />
Now filter to RDP traffic, or TCP port 3389 (TCP port == 3389). Observe that there is a constant flow of traffic. This is because you are usign the Remote desktop protocol right this moment to connect to the virtual machine in the first place! You will see your own computers IP address communicating with the virtual machine. When you are done cleanup your resources in Azure. 
