# Networking Basics Notes

## Networking Fundamentals

### Topics Covered

* IP Addressing basics
* IPv4 overview
* Public IP vs Private IP
* Network ID and Host ID
* Subnetting basics
* CIDR notation
* DNS (Domain Name System)
* Domain and hostname basics

---

## IP Addressing

Every device connected to a network has an IP address.

It helps identify the device and allows communication between systems.

Example:

192.168.1.10

IPv4 addresses use 32 bits and are written in 4 octets separated by dots.

Example:

192.168.1.1

---

## Public IP vs Private IP

### Public IP

A public IP is used to access the internet and can be reached globally.

Example:
EC2 public IP

### Private IP

A private IP is used inside internal networks like home Wi-Fi, office networks, or AWS VPC.

Examples:

192.168.x.x
10.x.x.x
172.16.x.x to 172.31.x.x

---

## Subnetting

Subnetting means dividing one network into smaller networks.

This helps with better network management, security, and proper IP usage.

Important concepts:

* Network ID
* Host ID
* Subnet Mask
* CIDR notation

Example:

192.168.1.0/24

Here `/24` means the first 24 bits are used for the network portion.

---

## DNS

DNS stands for Domain Name System.

It converts domain names into IP addresses.

Example:

google.com → IP address

Because systems communicate using IP addresses, not domain names.

---

## Domain and Hostname

### Domain

A human-readable website name.

Example:

example.com

### Hostname

The specific name of a machine or server.

Example:

app-server-01

---

## Basic Flow

User enters google.com
→ DNS finds the IP address
→ Browser sends request to that server
→ Server responds
→ Website opens

---

# Networking Basics Notes

## Networking Fundamentals

### Topics Covered

* IP Addressing basics
* IPv4 overview
* Public IP vs Private IP
* Network ID and Host ID
* Subnetting basics
* CIDR notation
* DNS (Domain Name System)
* Domain and hostname basics

---

## IP Addressing

Every device connected to a network has an IP address.

It helps identify the device and allows communication between systems.

Example:

192.168.1.10

IPv4 addresses use 32 bits and are written in 4 octets separated by dots.

Example:

192.168.1.1

---

## Public IP vs Private IP

### Public IP

A public IP is used to access the internet and can be reached globally.

Example:
EC2 public IP

### Private IP

A private IP is used inside internal networks like home Wi-Fi, office networks, or AWS VPC.

Examples:

192.168.x.x
10.x.x.x
172.16.x.x to 172.31.x.x

---

## Subnetting

Subnetting means dividing one network into smaller networks.

This helps with better network management, security, and proper IP usage.

Important concepts:

* Network ID
* Host ID
* Subnet Mask
* CIDR notation

Example:

192.168.1.0/24

Here `/24` means the first 24 bits are used for the network portion.

---

## DNS

DNS stands for Domain Name System.

It converts domain names into IP addresses.

Example:

google.com → IP address

Because systems communicate using IP addresses, not domain names.

---

## Domain and Hostname

### Domain

A human-readable website name.

Example:

example.com

### Hostname

The specific name of a machine or server.

Example:

app-server-01

---

## Basic Flow

User enters google.com
→ DNS finds the IP address
→ Browser sends request to that server
→ Server responds
→ Website opens

---

## Notes

These are the foundational networking concepts before moving to advanced topics like routing, ports, load balancers, VPC design, and cloud networking.
