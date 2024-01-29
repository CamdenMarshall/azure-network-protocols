<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this document, the process to observe various network traffic between Azure Virtual Machines with Wireshark and experiment with Network Security Groups.

<h2>Materials Used</h2>

-  Microsoft Azure (Virtual Machines/Computers/Network)
-  Remote Desktop
-  Command-Line Tools
-  Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
-  Wireshark (Protocol Analyzer)
-  Windows 10 OS (Operating System)
-  Ubuntu Server 20.04

<h2>Overall Process in Steps</h2>

-  Create the associated virtual machines needed
-  Login to the Windows Virtual Machine and Install Wireshark
-  Perform Various tasks to monitor ICMP traffic on the Virtual Network
-  Observe SSH traffic via Wireshark
-  Observe DHCP traffic on the Virtual Network
-  Observe DNS traffic on the Virtual Network
-  Lastly observe RDP traffic on the Virtual Network

<h2>Creating the Virtual Machines</h2>

- Create a Resource Group and name it as you deem necessary
- Create a Virtual Machine with a Windows 10 Operating System within the Resource Group
- Create a Virtual Machine with Ubuntu within the Resource Group and ensure both Virtual Machines are on the same Virtual Network


<h2>Installing Wireshark on Virtual Machine</h2>

- Using Remote Desktop connect to the Windows 10 Virtual Machine
- Install Wireshark by opening up the internet browser and Googleing Wireshark
- Choose the Windows x64 Installer and do a default install
- Once it is installed open up the application and click the blue fin to monitor traffic

![image](https://github.com/CamdenMarshall/azure-network-protocols/assets/153537343/a64af64e-ad2f-41ab-9b30-c85e6a38d8bc)

![image](https://github.com/CamdenMarshall/azure-network-protocols/assets/153537343/08904f99-b60e-436d-82c3-5391abde8da1)


<h2>Observing ICMP Traffic</h2>

- To track ICMP traffic click on the filter search up at the top and enter icmp
- There should not be traffic being displayed
- Find the Private IP address of the Ubuntu Virtual Machine and then return to the Windows Virtual Machine
- Use Windows Powershell to ping the Ubuntu Virtal Machine

![image](https://github.com/CamdenMarshall/azure-network-protocols/assets/153537343/e37090b0-e4ac-46e0-adfb-e48d767f3153)

![image](https://github.com/CamdenMarshall/azure-network-protocols/assets/153537343/473caf63-c979-48e6-abc8-5cab73b38d2c)

- Using Windows Powershell we can ping www.Google.com -4 to use the IPv4 Address

  ![image](https://github.com/CamdenMarshall/azure-network-protocols/assets/153537343/b6e02340-8422-4c9a-8eef-1ef6a0a0a9f6)

- Now to clear the data history on Wireshark click the green fin and continue without saving
- Go back to Powershell and ping the Ubuntu Virtual Machine with a perpetual ping by adding -t to the command
- Once the perpetual ping is going return to the Azure portal to modify the firewall of the Ubuntu Virtual Machine to deny ICMP traffic
- To do this access VM2's nsg and go to inbound Security Rules and click Add
- Change Protocal to ICMP and under the Action section click Deny. Then set the priority to 200

  ![image](https://github.com/CamdenMarshall/azure-network-protocols/assets/153537343/075b3583-a20f-47a3-a894-7c5fbac44461)

- Once that is done you should be seeing this from both Powershell and Wireshark

![image](https://github.com/CamdenMarshall/azure-network-protocols/assets/153537343/3ef632ae-2319-4ca3-aab6-7c179c7db76e)

- Now we can go and enable ICMP traffic again from the Azure portal and end the ping activity once we see the traffic return

<h2>Observing SSH Traffic</h2>

- Beginning here we will clear the traffic in Wireshark and alter our filter search for "ssh" (Secure Shell) traffic
- Next we will use Powershell to ssh into the VM2 by using the command ssh "username"@private IP address
- Example of that command being used is as follows

![image](https://github.com/CamdenMarshall/azure-network-protocols/assets/153537343/a58eebec-b285-4e69-8144-e44bb21b3e9a)

- Once the attempt to connect to the VM2 with SSH there will be traffic generated in wireshark
- Continue with the login process and when prompted to enter the password the cursor will not move as you type in the password
- To generate more traffic to ensure it is working type id in the command line
- Once finished type exit to leave the VM2

<h2>Observing DHCP Traffic</h2>

- To create DHCP traffic we will have to enter ipconfig /renew to generate a new IP address to and generate DHCP traffic
- The reason to do this is because DHCP is how IP addresses are automatically assigned

  ![image](https://github.com/CamdenMarshall/azure-network-protocols/assets/153537343/aa610657-3fb4-44ea-933b-d71f111facaf)


<h2>Observing DNS Traffic</h2>

- Upon changing the filter to search for DNS traffic only there will more than likely already be some traffic there you can clear it if you wish
- Within Powershell enter the command nslookup www.google.com for example to see DNS traffic be generated as the IP addresses for Google are found.
- DNS traffic will also automatically happen in the background usually so if you see traffic generate without input that should be fine

  ![image](https://github.com/CamdenMarshall/azure-network-protocols/assets/153537343/c075dc2f-fa53-4f3b-8d90-9cd207a3f109)


<h2>Observing RDP Traffic</h2>

- RDP traffic is Remote Desktop traffic and to better observe the traffic within Wireshark type into the filter tcp.port==3389
- You should see an immediate stream of traffic with both your IP address and that of your virtual machine

  ![image](https://github.com/CamdenMarshall/azure-network-protocols/assets/153537343/418ef6f6-3c91-4e20-8bc9-f39f44762a33)

- This is the end of the Azure Network Protocols
