# SQL Injection Detection with Snort3 and Apache

![Static Badge](https://img.shields.io/badge/Version-1.0-blue)
[![Ubuntu](https://img.shields.io/badge/Ubuntu-22.04-E95420?logo=ubuntu&logoColor=white)](https://ubuntu.com/)
[![Apache](https://img.shields.io/badge/Apache-2.4-red)](https://httpd.apache.org/)
[![PHP](https://img.shields.io/badge/PHP-7.4-green)](https://www.php.net/)
[![MySQL](https://img.shields.io/badge/MySQL-8.0-blue)](https://www.mysql.com/)
[![Snort3](https://img.shields.io/badge/Snort-3.1.28.0-lightgrey)](https://www.snort.org/)
[![tcpdump](https://img.shields.io/badge/tcpdump-networking-critical)](https://www.tcpdump.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

This project aims to implement an Intrusion Detection System (IDS) using **Snort3** to detect **SQL injection** attempts on a web page hosted on an **Apache2** server.

## Index

1. [Introduction](#1-introduction)  
2. [Technologies Used](#2-technologies-used)  
3. [Environment Setup](#3-environment-setup)  
   - [3.1 VirtualBox Configuration](#31-virtualbox-configuration)  
   - [3.2 Web Access Verification](#32-web-access-verification)  
   - [3.3 SQL Injection Test Database](#33-sql-injection-test-database)  
4. [Snort3 Rule Configuration](#4-snort3-rule-configuration)  
   - [4.1 Rule Creation](#41-rule-creation)  
   - [4.2 Testing and Logs](#42-testing-and-logs)  
5. [Conclusion](#5-conclusion)  
6. [Web Resources](#6-web-resources)

---

## 1. Introduction

Detecting web attacks is a key aspect of cybersecurity. In this project, we implemented an IDS using **Snort3** to detect common SQL injection patterns in HTTP requests. **VirtualBox** is used to simulate a realistic environment and allow network access to the hosted web server.

---

## 2. Technologies Used

- **Ubuntu 22.04** – Base operating system.
- **Apache2** – Web server.
- **Snort3 (v3.1.28.0)** – IDS configured to detect SQL injection attempts.
- **PHP** – Backend language for the web application.
- **MySQL** – Database for test data.
- **CSS** – Frontend styling.
- **tcpdump** – Network monitoring and packet capture tool.

---

## 3. Environment Setup

### 3.1 VirtualBox Configuration

Configured a **NAT network** with **port forwarding**.  
Settings → Network → NAT Adapter → Advanced → Port Forwarding:

| Name | Protocol | Host Port | Guest IP | Guest Port |
|------|----------|-----------|----------|------------|
| HTTP | TCP      | 8080      | 127.0.0.1| 80         |

### 3.2 Web Access Verification

To verify:

```
http://localhost:8080
```

Or from another device in the network:

```
http://<host-ip>:8080
```

### 3.3 SQL Injection Test Database

A database named **`injeccion`** with a **`usuarios`** table was created to simulate real SQL injection attempts and test the detection rule effectiveness.

---

## 4. Snort3 Rule Configuration

### 4.1 Rule Creation

Custom rule added to `local.rules` to detect suspicious URL parameters:

```
alert tcp any any -> any 80 (msg:"SQL Injection test"; content:"id="; sid:1000001;)
```

### 4.2 Testing and Logs

Run Snort3 with:

```bash
snort -c /etc/snort/snort.lua -R /etc/snort/rules/local.rules -i enp0s3 -s 65535 -k none -l /var/log/snort
```

Local test:

```
http://localhost/injeccion.php?id=1' OR '1'='1
```

From another machine:

```
http://192.168.1.X:8080/injeccion.php?id=1' OR '1'='1
```

Example log output:

```
02/26-15:47:10.872362 [**] [1:1000001:0] "SQL Injection test" [Priority: 0] {TCP} 10.0.2.2:42298 -> 10.0.2.15:80
```

---

## 5. Conclusion

This project provided practical experience in using Snort3 to detect SQL injection in a controlled environment. The integration with Apache and the virtual network setup enabled comprehensive testing and validation of our detection rule.

**Future improvements**:
- Add more rules for various SQLi techniques.
- Automate blocking malicious IPs.
- Use additional monitoring tools like `fail2ban`.

---

## 6. Web Resources

- [GitHub Project Repository](https://github.com/izaann3/SQL-injection-Snort3)
- [SQL Injection Tutorial – YouTube](https://www.youtube.com/watch?v=IQPtBA1p4fk)
- [Preventing SQL Injection in PHP – YouTube](https://www.youtube.com/watch?v=ZH0YCUmuZc4)
- [SQL Injection Practical Exploitation – YouTube](https://www.youtube.com/watch?v=qLeeLRn9Z78)
- [Complete Guide to SQL Injection Attacks – YouTube](https://www.youtube.com/watch?v=vIDfTbG1llw)

> Created by Izan Lozano, Iker Macias & Nicolas Rubiales  
> INS Manuel Vázquez Montalbán – M11 SAD 2024/25
