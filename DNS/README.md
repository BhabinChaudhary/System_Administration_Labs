# 1. Rocky Linux: DNS Bind9 Server Installation and Configuration 

## Step 1: Update the Rocky Linux Server
We need to update to improve security, fix software bugs, enhance performance, add new features, support new hardware, and maintain compatibility with the latest applications. Linux updates install security patches and newer versions of packages, helping keep the system stable, reliable, and protected from vulnerabilities. 
<img width="975" height="271" alt="image" src="https://github.com/user-attachments/assets/a54c6caa-cdc2-4a8e-812f-d342476b9167" />

## Step 2: Install BIND and BIND Utilities
Here we install two packages i.e. **bind** and **bind-utils**. The bind package installs the BIND9(Berkley Internet Name Domain) DNS server software which installs things like **named(DNS Server Daemon), /etc/named.conf, /var/named/, systemd service(named.service)**. Similarly, the bind-utils package installs DNS testing tools in which commands include dig, nslookup, host etc.
<img width="975" height="32" alt="image" src="https://github.com/user-attachments/assets/bdd56a53-963d-430d-9423-3c94dd86aa24" />
<img width="975" height="569" alt="image" src="https://github.com/user-attachments/assets/3c578383-de9f-41bc-8517-eaca7f36bbc5" />

## Step 3: Enable and Start the DNS Service
Here we enable the services. Let’s break the commands. **Systemctl** controls Linux services, **named** is as DNS daemon where daemon means Background services such as Apache, SSH, MariaDB etc and **enable** starts automatically after reboot, 
**--now** means start immediately.. Without enable server will work now and after reboot DNS will disappear. **Systemclt status named** whether the DNS is running, failed or stopped. Administrator should always verify after starting a service.
<img width="975" height="594" alt="image" src="https://github.com/user-attachments/assets/add65678-8909-4b4b-b84f-1b5c594c48bc" />

## Step 4: Locate named.conf
In this step, we are locating named.conf file.
<img width="975" height="67" alt="image" src="https://github.com/user-attachments/assets/6218ba52-e1cf-48ea-9cf3-7c4e5f08aa5f" />

## Step 5: Backup the DNS Configuration File
As a system administrator we always backup important files. In **cp -p /etc/named.conf /etc/named.conf.bak**. **-p** stands for preserve and it’s purpose is to copy the file while preserving its original attributes. The attributes includes: Permissions(read, write, execute), Owner(user), Group, Timestamp(last modified and access times). **“ls -l /etc”** lists everything inside the **/etc** directory with detailed information. As we can see two file original: **named.conf** and copy: **named.conf.bak**. 
<img width="975" height="32" alt="image" src="https://github.com/user-attachments/assets/9c7504a0-7e51-4b3e-9cf3-ca4376521737" />
<img width="975" height="31" alt="image" src="https://github.com/user-attachments/assets/beca8d1e-3e87-4481-a370-90847f59e2b0" />
<img width="975" height="54" alt="image" src="https://github.com/user-attachments/assets/a30487eb-7bfb-4624-9340-13bd0203f4f3" />

## Step 6: Configure named.conf
Now we will configure the DNS server configuration which is the **named.conf** under the etc directory. Here we change **“listen-on port 53 { 127.0.0.1; };”** into **“listen-on port 53 {127.0.0.1; any; };”** means initially DNS only accepts local request but after modification every computer on the network can ask our DNS server. Also do **“allow-query { any; };”** which allows all clients to perform DNS lookups, without these changes only the server itself can use DNS.
<img width="975" height="33" alt="image" src="https://github.com/user-attachments/assets/5dfe2d12-1b19-49aa-a444-aa2d9ec70c6f" />
<img width="975" height="610" alt="image" src="https://github.com/user-attachments/assets/0d8383f3-481d-4d12-9a3e-ae9bc187d61e" />

## Step 7: Create Forward and Reverse Lookup Zones
In the same named.conf file, scroll down and right after zone “.” IN ….., we need to create two zones, one is forward and the other is a reverse look-up zone. Our zones and their configuration file names can have whatever name we choose, although the following naming convention is often recommended:
```
      •	Forward lookup zone Name: Zone “bungkus.org”
           o	This is the domain of your server and client hostname
      •	Reverse lookup zone Name: Zone “200.168.192-addr.arpa.org”
           o	This is the reverse of the initial three octets (numbers) of our network address, for example 192.168.200.5; the initial three will be reversed to 200.168.192.
```
Next we need to identify each zone their relevant configuration file which we need to create later. Same as the zone name we choose the best practice as below:
```
      •	Forward lookup zone configuration file: “fwd.bungkus.org.db”  
           o	Remember bungkus.org is the domain name used in this lab, your domain will be different. 
      •	Reverse lookup zone Name: “rvs.200.168.192.db” 
          o	This is the reverse of the initial three octets (numbers) of our network address, for example, 192.168.200.5; the initial three will be reversed to 200.168.192.
```
<img width="975" height="126" alt="image" src="https://github.com/user-attachments/assets/6cc56dae-c720-4fe5-a3ac-a9eabc7e2322" />
<img width="975" height="278" alt="image" src="https://github.com/user-attachments/assets/c83e2182-94f6-4322-b21b-3dccef1a9f06" />

## Step 8: Verify named.conf Syntax
After we complete doing configuration of the **named.conf** file, we need to check for syntax error, so we use the command **“sudo named-checkconf”**, we shouldnot get any message prompt .
<img width="975" height="33" alt="image" src="https://github.com/user-attachments/assets/5248b2cf-b71c-4c6f-9501-1f0e65f71868" />


## Step 9: Navigate to the DNS Zone Directory
In this step, we changed the directory to **/var/named** because we need to create our forward and reverse zone files that we defined at our **named.conf file**. The **/var/named** directory stores the actual DNS database files example **fwd.bungkus.org.db** and **rvs.200.168.192.db**. 
<img width="975" height="38" alt="image" src="https://github.com/user-attachments/assets/3a2d6265-02e0-4062-9c03-a5219d64d9c5" />
<img width="975" height="302" alt="image" src="https://github.com/user-attachments/assets/1ccf2f57-6bc5-4623-9cbb-176a5c0c57ce" />

## Step 10: Create the Forward Lookup Zone File
In this step we will create forward lookup zone file using a nano text editor nano fwd.bungkus.org.db. We can use templete copy and paste it into the editor. At the Rocky Server (inside the virtual the virtual machine) open the firefox browser. Type the URL (https://access.redhat.com/documentation/enus/red_hat_enterprise_linux/4/html/reference_guide/s2-bind-zone-examples). Copy this template and paste it inside the forward lookup zone configuration file of **fwd.bungkus.org.db**. Here are some of the terms used in the forward lookup zone configuration file:
            • $TTL: It stands for Time to Live. It tells other DNS servers “you may cache this answer for 86400 seconds”. So if someone asks www.bungkus.org their computer remembers the answer for one day.
            • SOA Record: It stands for Start of Authority. Every DNS zone must have one SOA record. Here @ means current zone actually means bungkus.org. 
            • Here **shaserver.bungkus.org.** is the Primary DNS Server. **2001062502** is the serial number which is very important, every time we modify the zone the number should be increased manually so that secondary DNS severs compare this number and know whether any changes were made or not. **21600** is the refresh time which means secondary DNS servers check for update every **21600** seconds and **3600** is the retry which means if refresh fails then try again in 3600. 604800 is the Expiry which means if secondary server cannot primary for 604800 seconds then stop answering DNS requests. 86400 is the Default cache time. 
            • A: Maps hostnames to IPv4 addresses.
            • CNAME: Creates aliases for hostnames.
<img width="975" height="33" alt="image" src="https://github.com/user-attachments/assets/feb0274f-1a37-42f3-9c97-604b2e403778" />
<img width="975" height="582" alt="image" src="https://github.com/user-attachments/assets/b99070a8-5467-4830-9060-9b1f327a4a2a" />

## Step 11: Verify the Forward Zone File
Here we check whether the forward zone file is created in **/var/named** directory. We use command **ls -l** to long list the files and we can see **fwd.bungkus.org.db** which is our forward zone file.
<img width="975" height="270" alt="image" src="https://github.com/user-attachments/assets/209ced33-92ba-497a-82f0-99ab54dbe5a2" />

## Step 12: Create the Reverse Lookup Zone File
In this step we will create reverse zone file using a nano text editor same as we created the forward zone file. We use the command nano **rvs.200.168.192.db** to create the zone file. We will use the template from the documentation (https://access.redhat.com/documentation/enus/red_hat_enterprise_linux/4/html/reference_guide/s2-bind-configuration-zone-reverse). So, we copy the template and paste it into the **rvs.200.168.192.db**. 
<img width="975" height="33" alt="image" src="https://github.com/user-attachments/assets/fd3af992-b1ae-4c55-94e9-cedfa297ce93" />
<img width="975" height="582" alt="image" src="https://github.com/user-attachments/assets/ef660468-cf4c-4977-bcd4-6fea5cc796eb" />

## Step 13: Verify the Reverse Zone File
Here we again check whether our reverse zone file is created or not. We can see **rvs.200.168.192.db** which is the reverse zone file. 
<img width="975" height="296" alt="image" src="https://github.com/user-attachments/assets/75d5a88a-c5c8-4d46-9c77-e3cd91474a32" />

## Step 14: Restart the DNS Service
In this step we restart and check the status of the DNS service. We used commands **systemctl** restart named and **systemctl** status named. Lets breakdown the command: **systemctl** is the command used to manage system services in Linux systems that use **systemd**. This command can start a service, stop a service, restart a service, check service status, enable a service at boot, diable a service. Services such as named, sshd, httpd, NetworkManager. The restart tells linux to stop the service and start it again, it is equivalent to **systemctl** stop named **systemctl** start named. Here named is the serive name of the BIND DNS service, when we installed **BIND:sudo dnf install bind bind-utils**, linux created a service called named. Suppose your forward zone original contains **shaserver IN A 192.168.200.4** then you change it to **shaserver IN A 192.168.200.10**, if you don’t restart the DNS server then the DNS server Still returns **192.168.200.4** because it hasn’t reloaded the updated file. After we retart the DNS server reads the modified zone file ad now returns **192.168.200.10**. And the **systemctl** status named  simply displays information about the named service.
<img width="975" height="501" alt="image" src="https://github.com/user-attachments/assets/36b63b33-cbbc-407b-a529-3656d479ce08" />

## Step 15: Allow DNS Through the Firewall
Now we need to allow the **firewall** to pass port 53 through UDP and TCP. Here we use the commands: **sudo firewall-cmd --permanent --add-port=53/udp** and **sudo firewall-cmd –permanent –add-port=53/tcp**. Lets break down the commands:**firewall-cmd** is the command-line tool used to manage **Firewalld**. **Firewalld** is the firewall service used by Rocky Linux, which allows us to open ports, close ports, allow services, block services, create firewall zones. **---Permanent** save the rule permanently, if we don’t include –permanent then the rule will exists only until the next reboot or firewall reload. For example: we first open port 53 then after we reload then the port 53 closes again. **--add-port=53/upd** means “open a specific network port” **53** is the DNS port as every network service has a standard port. For example: HTTP has port 80, SSH has port 22.upd specifies the protocol, most DNS queries use UPD(User Datagram Protocol) because it is fast, has low overhead and is ideal for smalll requests. We need to use tcp also because the DNS uses both protocols. UDP is used for normal hostname lookups, ping, dig whereas TCP is used when DNS responses.
<img width="975" height="133" alt="image" src="https://github.com/user-attachments/assets/7fdf578a-bc3a-4e82-818e-abbb8e92a824" />

## Step 16: Configure DNS Servers in NetworkManager
Now we put the our Rocky Linux and google’s ip address in DNS.Here we used the command **sudo nano /etc/NetworkManager/system-connections/enp0s3.nmconnection**.Lets break down the command: **/etc/NetworkManager/system-connections/**, here /etc directory stores system configuration files example: `/etc/passwd, /etc/hosts, /etc/resolv.conf, /etc/named.conf` similary NetworkManager is the service responsible for managing network connc	in Linux which controls ip addreses, Gateway, DNS servers, Network interfaces, Wi-Fi connections, ethernet connections. **system-connection** folder stores the configuration files for every network connection, enp0s3.nmconnection is the configuration file for your network interface where enp0s3 is the name of our network interface and .nmconnection is a file that stores settings such as: static IP address, Gateway, DNS server, Search domain, IPv4 method, IPv6 method.
<img width="975" height="47" alt="image" src="https://github.com/user-attachments/assets/efd7966e-79a1-4ae7-ab88-ae1fac47c2e2" />
<img width="975" height="591" alt="image" src="https://github.com/user-attachments/assets/7139353c-3bd0-49e8-bc93-f48105438d2e" />

## Step 17 : Verify DNS Server Order
In this step, we checked our DNS IP addres in resolve.conf file and make sure our DNS server IP(Rocky Server) is the first in line.Our Rocky linux ip is **192.168.200.5**
<img width="975" height="31" alt="image" src="https://github.com/user-attachments/assets/3d9eabcc-7582-4086-829f-4124e3846b4b" />
<img width="975" height="139" alt="image" src="https://github.com/user-attachments/assets/3b200aaf-6a91-4aa7-9de2-642a66812a77" />

## Step 18: Restart the DNS Service Again
<img width="975" height="24" alt="image" src="https://github.com/user-attachments/assets/09b49405-5db8-408f-9a41-b2e9e445ddb9" />

## Step 19: Verify DNS Functionality
Now we check whether the DNS server is working or not, so we use **NSLOOKUP**
<img width="975" height="305" alt="image" src="https://github.com/user-attachments/assets/f24d83b5-aa67-443e-a61f-d826cede9603" />

## Step 20: 
<img width="975" height="254" alt="image" src="https://github.com/user-attachments/assets/2849fb24-37c7-4b1a-9a7c-2dcff60e337f" />

## Step 21: 
<img width="975" height="612" alt="image" src="https://github.com/user-attachments/assets/15182ce0-4b3e-469c-aaf6-fb1bab0d4ce5" />
<img width="975" height="617" alt="image" src="https://github.com/user-attachments/assets/8fdc9966-2634-455c-ae5a-62fc215e2614" />

## Step 22: 
<img width="975" height="269" alt="image" src="https://github.com/user-attachments/assets/18c974da-5bbe-4f06-985d-388571b0e15f" />



























