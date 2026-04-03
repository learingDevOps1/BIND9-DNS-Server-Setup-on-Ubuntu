# BIND9-DNS-Server-Setup-on-Ubuntu

BIND9 DNS Server Setup on Ubuntu

📌 Overview

This project demonstrates how to set up a local DNS server using BIND9 on Ubuntu. It includes forward and reverse DNS configuration, validation, and testing.

🎯 Objectives
Configure a DNS server
Create forward lookup zone
Create reverse lookup zone
Integrate DNS with system resolver
Test DNS functionality
⚙️ Step-by-Step Implementation
1. Install BIND9
sudo apt update
sudo apt install bind9 bind9utils bind9-doc -y
2. Configure Forward Zone
Copy template
sudo cp /etc/bind/db.local /etc/bind/db.ahmed.lab
Edit zone file
sudo nano /etc/bind/db.ahmed.lab
$TTL    604800

@   IN  SOA ns.ahmed.lab. root.ahmed.lab. (
        2
        604800
        86400
        2419200
        604800 )

@       IN  NS      ns.ahmed.lab.

ns      IN  A       192.168.1.10
@       IN  A       192.168.1.10
www     IN  A       192.168.1.10
3. Configure BIND
sudo nano /etc/bind/named.conf.local
zone "ahmed.lab" {
    type master;
    file "/etc/bind/db.ahmed.lab";
};
4. Configure Reverse Zone
sudo cp /etc/bind/db.127 /etc/bind/db.192
Edit reverse file
sudo nano /etc/bind/db.192
$TTL 604800

@   IN  SOA ns.ahmed.lab. root.ahmed.lab. (
        2
        604800
        86400
        2419200
        604800 )

@       IN  NS  ns.ahmed.lab.

10      IN  PTR www.ahmed.lab.
Add reverse zone config
sudo nano /etc/bind/named.conf.local
zone "1.168.192.in-addr.arpa" {
    type master;
    file "/etc/bind/db.192";
};
5. Validate Configuration
sudo named-checkconf
sudo named-checkzone ahmed.lab /etc/bind/db.ahmed.lab
sudo named-checkzone 1.168.192.in-addr.arpa /etc/bind/db.192
6. Restart Service
sudo systemctl restart bind9
7. Configure System Resolver
sudo nano /etc/systemd/resolved.conf

Add:

DNS=127.0.0.1

Restart:

sudo systemctl restart systemd-resolved
8. Testing
Forward DNS
dig www.ahmed.lab
Reverse DNS
dig -x 192.168.1.10
Authority
dig NS ahmed.lab
dig SOA ahmed.lab
🧪 Expected Results
Forward resolution returns correct IP
Reverse resolution returns correct hostname
No errors or warnings
🧠 Conclusion

This project demonstrates a complete DNS setup using BIND9, including forward and reverse resolution, validation, and system integration.
