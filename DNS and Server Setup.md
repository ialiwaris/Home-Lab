# DNS and Server Setup...
### This repository provides a complete guide to setting up a server and configuring DNS for the domain example.com. The repository includes all necessary commands and steps for professionals looking to deploy and manage a reliable server environment.

## **Features**
- Step-by-step server setup on Ubuntu Server.
- DNS configuration for custom domains.
- Forward and reverse zone configuration.
- Secure and scalable environment.

---

## **Prerequisites**
- A server with Ubuntu installed.
- Root or sudo access to the server.
- A registered domain (`example.com`).
- Basic knowledge of Linux commands.

---

# **Steps to Set Up the Server**
## *Replace {192.168.0} with your {SERVER IP}
## *Replace {example.com} with your {Registered Domain}
### 1. **Update the System**
```bash
sudo apt update && sudo apt upgrade -y
```
### 2. Install Required Packages
```bash
sudo apt install -y bind9 bind9utils bind9-doc net-tools curl
```
### 3. Configure DNS
#### Forward DNS Zone Configuration
##### 1-Open the BIND configuration file:
```bash
sudo nano /etc/bind/named.conf.local
```
##### 2-Add the following zone for the forward zone:
```bash
zone "example.com" {
    type master;
    file "/etc/bind/db.example.com";
};
```
##### 3-Create the forward zone file:
```bash
sudo cp /etc/bind/db.local /etc/bind/db.example.com
```
##### 4-Edit the forward zone file:
```bash
sudo nano /etc/bind/db.example.com
```
##### 5-Replace the content with the following:
```bash
$TTL    604800
@       IN      SOA     ns1.example.com. admin.example.com. (
                        2         ; Serial
                   604800         ; Refresh
                    86400         ; Retry
                  2419200         ; Expire
                   604800 )       ; Negative Cache TTL
;
@       IN      NS      ns1.example.com.
@       IN      A       {SERVER IP}
ns1     IN      A       {SERVER IP}
www     IN      A       {SERVER IP}
mail    IN      A       {SERVER IP}
@       IN      MX 10   mail.example.com.
```
#### Reverse DNS Zone Configuration
##### 1-Open the BIND configuration file:
```bash
sudo nano /etc/bind/named.conf.local
```
##### 2-Add the reverse zone configuration in the same file (named.conf.local):
```bash
zone "0.168.192.in-addr.arpa" {
    type master;
    file "/etc/bind/db.192.168.0";
};
```
##### 3-Create the reverse zone file:
```bash
sudo cp /etc/bind/db.127 /etc/bind/db.192.168.0
```
##### 4-Edit the reverse zone file:
```bash
sudo nano /etc/bind/db.192.168.0
```
##### 5-Replace the content with the following:
```bash
$TTL    604800
@       IN      SOA     ns1.example.com. admin.example.com. (
                        2         ; Serial
                   604800         ; Refresh
                    86400         ; Retry
                  2419200         ; Expire
                   604800 )       ; Negative Cache TTL
;
@       IN      NS      ns1.example.com.
6       IN      PTR     example.com.
```
### 4. Restart BIND9
```bash
sudo systemctl restart bind9
```
### 5. Test DNS Configuration
#### Test Forward Zone:
```bash
dig @localhost example.com
```
#### Test Reverse Zone:
```bash
dig -x 10.235.150.6 @localhost
```
### 6. Configure Firewall
```bash
sudo ufw allow 53/tcp
sudo ufw allow 53/udp
sudo ufw reload
```
### 7. Point Domain to the Server
 1-Login to your domain registrar's dashboard.
 
 2-Add the following records in the DNS settings:
 ##### A Record: ``` example.com -> 192.168.0 ```
 ##### NS Record: ``` example.com -> ns1.example.com ```

### 8. Verification
#### Forward Lookup
```bash
nslookup example.com
```
#### Reverse Lookup
```bash
nslookup 192.168.0
```
## Your DNS server is successfully set up! 🎉
# Thank You...
