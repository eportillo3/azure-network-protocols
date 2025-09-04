<p align="center">
<img src="https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/WIRESHARKimg.png" height="40%" width="70%"alt="Wireshark Logo"/>


<h1>Network Security Groups (NSGs) and Inspecting Network Protocols</h1>
In this walkthrough, we will set up two virtual machines (VMs) in Microsoft Azure. Our objective is to monitor and analyze the network traffic between the two VMs using Wireshark. In addition to capturing direct communication, we will also observe background traffic and other types of network activity that occur within the virtual environment.<br />

<h2>Languages</h2>

- PowerShell 

<h2>Environments Used</h2>

- Microsoft Azure
- macOS (Host Machine)
- Windows 10
- Linux Ubuntu
- Windows App(Remote Desktop)
- PowerShell


<h2>Technology/Applications/Services </h2>

- Azure Virtual Machines
- Wireshark
- PowerShell


<h2>Walkthrough</h2>

</br>
<h3 align="center">
  Setting up the Virtual Machines
</h3>
</br>

First, a resource group needs to be created so our VM’s can be put in it.
  
<img width="1086" height="696" alt="image" src="https://github.com/user-attachments/assets/b277390d-a8d7-4d0c-a8e7-4bfd897904cf" />
</p>
<br />
<p>
Create a Windows 10 Virtual Machine (VM)</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Same resource group, new Vnet, & new Subnet

  


<img width="1100" height="1108" alt="image" src="https://github.com/user-attachments/assets/63866a97-eedd-450d-9846-c7c2eaccd224" />
</p>
<br />
<p>
Create a Linux (Ubuntu) VM</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Same resource group, Vnet & Subnet as the Windows VM

</p>



<img width="1208" height="1206" alt="image" src="https://github.com/user-attachments/assets/d4b51962-8908-4efe-87d3-534db1b3bfe9" />
</p>
</br>
<h3 align="center">
  Observing ICMP Traffic
</h3>
</br>

Use Windows App(Remote Desktop), to log in to the Windows VM.



<img width="520" height="172" alt="image" src="https://github.com/user-attachments/assets/9fc1717b-e384-47b2-a397-d6dacad14774" />
</p>
<br />
<p>
Install Wireshark in the Windows VM
</p>



<img width="2482" height="796" alt="image" src="https://github.com/user-attachments/assets/05db86e5-ac96-4105-be95-0d390107e7eb" />
</p>
<br />
<p>
Open Wireshark and start packet capture, by selecting an interface and then clicking start capturing packets button.
</p>



<img width="1072" height="700" alt="image" src="https://github.com/user-attachments/assets/1b8def8d-fd8c-49a5-82ba-8ee661a9fdd8" />
</p>
<br />
<p>
Within Wireshark, filter for ICMP traffic only

<img width="1194" height="772" alt="image" src="https://github.com/user-attachments/assets/f4491693-e13a-42bc-b631-35f9934c5ad2" />
 
</p>
</br>
Get the Linux VM’s private IP address(10.0.0.5) and from within the Windows VM, use PowerShell to ping the Linux VM</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - type “ping 10.0.0.5”</br>
*As a result, ICMP traffic is displayed in purple on Wireshark





<img width="1206" height="688" alt="image" src="https://github.com/user-attachments/assets/8022658c-b03e-442d-98ea-0a8b70baa233" />
</p>
</br>
<h3 align="center">
  Configuring a Firewall [Network Security Group]
</h3>
</br>

Initiate a perpetual/non-stop ping from the Windows VM to the Ubuntu VM</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - type “ping 10.0.0.5 -t”




<img width="1068" height="510" alt="image" src="https://github.com/user-attachments/assets/8b0f7ae0-8115-413b-8709-9fc8ff2ab5cf" />
</p>
<br />


Open the Network Security Group the Ubuntu VM is using and disable incoming (inbound) ICMP traffic</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Azure Portal -> Linux VM(that we created) -> Networking -> Network Settings -> Click nsg link under Network Security Group -> Settings -> Inbound Security Rules -> Add and set up based on the following picture(Deny ICMPv4)

<img width="2138" height="1282" alt="image" src="https://github.com/user-attachments/assets/de914c63-14fa-4c08-91ac-c03b8e916acd" />
</p>
<br />
<p>
Back in the Windows VM, observe the ICMP traffic in WireShark and the command line Ping activity</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Once our rule is active, we will stop getting a reply from the Linux VM(10.0.0.5).

</p>



<img width="988" height="290" alt="image" src="https://github.com/user-attachments/assets/c2262933-0c8b-49a6-98a9-e938dd06d2da" />
</p>
<br />
<p>
Let's say we simply delete the rule that we made: as a result, we will start getting replies from the Linux VM again.

</p>




<img width="1088" height="332" alt="image" src="https://github.com/user-attachments/assets/68303233-cbce-4388-8630-3b13adc2767b" />
</p>
</br>
<h3 align="center">
  Observe SSH Traffic
</h3>
</br>

On Windows VM, start packet capture on Wireshark and filter for SSH traffic only



<img width="862" height="430" alt="image" src="https://github.com/user-attachments/assets/9f0bebdf-8eca-4ef3-b3d5-92bab70f0d88" />
</p>
<br />
<p>
From the Windows VM we will establish an SSH connection with the Linux VM</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Open PowerShell and type: ssh <username>@<private IP address>. In our case, private IP address for Linux VM is 10.0.0.5


</p>



<img width="1044" height="514" alt="image" src="https://github.com/user-attachments/assets/3bb830e9-7558-421d-b130-15d91a75ae35" />
</p>
<br />
<p>
Type commands (username, pwd, etc) into the linux SSH connection and observe SSH(tcp.port 22) traffic spam in WireShark
</p>

<img width="1038" height="526" alt="image" src="https://github.com/user-attachments/assets/3915ca76-a93e-4cf4-a7b0-4692bfe8edd4" />
</p>
<br />
<p>
We can exit the SSH connection by typing ‘exit’ and pressing [Enter] in PowerShell, and on Wireshark we can see a RST, ACK packet that indicates the connection was ended. 

</p>



<img width="656" height="280" alt="image" src="https://github.com/user-attachments/assets/38bc7c19-4cd1-41a4-b2e8-5aa501da52a0" />
</p>
</br>
<h3 align="center">
  Observe DHCP Traffic
</h3>
</br>

On Windows VM, start packet capture on Wireshark and filter for DHCP traffic only



<img width="842" height="416" alt="image" src="https://github.com/user-attachments/assets/c9c0b364-1615-488e-b8ae-f0b2ddaae2f3" />
</p>
</br>
We will attempt to issue our Windows VM a new IP address and see the DHCP traffic in the background.</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Open PowerShell as admin and run: ipconfig /renew


<img width="1056" height="572" alt="image" src="https://github.com/user-attachments/assets/70a8aa38-c624-4b06-8028-da605c1e5d3c" />
</p>
<br />


Since we are on a Virtual Machine, above isn’t normal behavior when running ipconfig /renew. Usually, the renew command is run along with ipconfig /release(first release then renew). If we run the release command on its own, then we will disconnect from our VM.

Something we can do to avoid this problem is to run a script so the release and renew command can be run, such that the renew occurs right after the release with no time in between.



<img width="668" height="100" alt="image" src="https://github.com/user-attachments/assets/62c5cec6-79e3-4869-9e10-b45393d84f9d" />
</p>
<br />
<p>
Then the private IP address will be renewed properly and Discover, Offer, Request, and Acknowledge packets can be seen.
</p>



<img width="952" height="322" alt="image" src="https://github.com/user-attachments/assets/f415f4a0-f14a-4f64-9427-73485aeb2076" />
</p>
</br>
<h3 align="center">
  Observe DNS Traffic
</h3>
</br>

On Windows VM, start packet capture on Wireshark and filter for DNS traffic only





<img width="1726" height="856" alt="image" src="https://github.com/user-attachments/assets/82d1a7ec-cb34-4488-b81f-c3ef3d7b4329" />
</p>
</br>
We will make DNS traffic(tcp.port 53) by going to PowerShell and looking up the IP address of a website.</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Open PowerShell and type: nslookup disney.com



<img width="650" height="206" alt="image" src="https://github.com/user-attachments/assets/8b284c09-e962-4db6-90f2-464d6f81cb39" />

<br />

Here is the traffic generated on Wireshark as a result.





<img width="930" height="466" alt="image" src="https://github.com/user-attachments/assets/1b0dd7cf-7dec-4ea9-a4bc-4254fc753eba" />
</p>
</br>
<h3 align="center">
  Observe RDP Traffic
</h3>
</br>

On Windows VM, start packet capture on Wireshark and filter for RDP traffic only</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Right after entering tcp.port==3389 filter there is constant traffic. This is because we are on a Virtual Machine and RDP traffic is constantly occurring to stream the VM on our host machine.





<img width="864" height="456" alt="image" src="https://github.com/user-attachments/assets/35df0ccd-5e40-4ba6-ab62-dd263f77c273" />
