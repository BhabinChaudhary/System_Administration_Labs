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















