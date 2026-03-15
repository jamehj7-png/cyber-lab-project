Home Cybersecurity Lab Documentation
Project Overview

This project documents the creation of a practical cybersecurity lab designed to simulate a small enterprise environment. The lab includes a Windows Domain Controller, a Windows client, an Ubuntu Server, a Kali Linux machine, and pfSense for firewall and routing. The objective is to practice system administration, network configuration, Active Directory management, monitoring, and security testing in a controlled environment.

Lab Objectives

Build a realistic virtual enterprise network

Configure Active Directory Domain Services

Configure DNS and DHCP

Join client machines to the domain

Configure Ubuntu Server services and monitoring

Use Kali Linux for reconnaissance and security testing

Use pfSense for network segmentation and routing

Document the whole lab for learning and portfolio purposes

Lab Machines

pfSense: Firewall, gateway, and routing between networks

Windows Server: Domain Controller with AD DS, DNS, and DHCP

Windows Client: Domain-joined workstation for testing users and policies

Ubuntu Server: Linux administration, monitoring, and service hosting

Kali Linux: Security testing and enumeration machine

Network Information

Example addressing used in the lab:

Domain Controller: 192.168.10.10

Kali Linux: 192.168.10.101

Ubuntu Server: 192.168.20.10

This structure allows communication testing, routing validation, and service access across multiple network segments.

Work Completed
1. Windows Server Configuration

The Windows Server was configured as the central management server of the lab. The following roles were installed:

Active Directory Domain Services

DNS

DHCP

The server was promoted to a Domain Controller and used to manage the domain environment.

2. DNS and DHCP Setup

DNS was configured to provide name resolution in the domain. DHCP was configured to assign IP addresses dynamically to client machines where needed.

3. Organizational Unit and Domain Preparation

Organizational Units were created to structure the domain logically. This makes administration easier for users, computers, and policies.

4. Ubuntu Server Configuration

Ubuntu Server was configured with networking and administrative tools. Netplan was used to define network settings. Connectivity tests were performed between Ubuntu, the Domain Controller, and pfSense. Troubleshooting included fixing gateway and routing issues.

5. Domain Join Troubleshooting

Ubuntu domain join and communication with the Domain Controller were tested. Problems such as failed domain joining, unreachable network messages, and DNS/gateway issues were investigated and corrected step by step.

6. Kali Linux Preparation

Kali Linux was installed and configured for practical security testing. Initial setup included:

SSH configuration for remote command execution

Static IP configuration

Password troubleshooting and reset

Installation preparation for reconnaissance and enumeration tools

7. Security and Administration Tools

The lab preparation included identifying tools to use on both Kali and Ubuntu for practical exercises, including network analysis, system monitoring, and Active Directory enumeration.

Tools Planned / Installed
On Kali Linux

openssh-server

nmap

smbclient

enum4linux-ng

ldap-utils

bloodhound

neo4j

python3-pip

pipx

bloodhound-python

tcpdump

wireshark

hydra

evil-winrm

metasploit-framework

On Ubuntu Server

openssh-server

net-tools

htop

curl

wget

git

ufw

tcpdump

wireshark

nginx

rsyslog

docker.io

docker-compose

lynis

prometheus-node-exporter

realmd

sssd

sssd-tools

adcli

krb5-user

samba-common-bin

packagekit

Kali Linux Setup Progress
SSH Service

SSH was installed and enabled on Kali to allow remote access from Windows using PuTTY. This made it easier to copy and paste commands and manage the machine.

Commands used:

sudo apt update
sudo apt upgrade -y
sudo apt install openssh-server -y
sudo systemctl start ssh
sudo systemctl enable ssh
sudo systemctl status ssh
ip a
ss -tulnp | grep :22
Nmap Installation

Nmap was installed to scan hosts and identify open ports and services.

Commands used:

sudo apt install nmap -y
nmap --version
nmap 192.168.10.10
nmap -sV 192.168.10.10
SMB Enumeration

SMB tools were installed to inspect shares and gather information from the Domain Controller.

Commands used:

sudo apt install smbclient -y
smbclient --version
smbclient -L //192.168.10.10 -N
sudo apt install enum4linux-ng -y
enum4linux-ng --help
enum4linux-ng 192.168.10.10
LDAP Enumeration

LDAP tools were installed to query Active Directory and inspect domain information.

Commands used:

sudo apt install ldap-utils -y
ldapsearch -VV
ldapsearch -x -H ldap://192.168.10.10
ldapsearch -x -H ldap://192.168.10.10 -b "dc=lab,dc=lan"
ldapsearch -x -H ldap://192.168.10.10 -b "dc=lab,dc=lan" "(objectClass=*)"
BloodHound Preparation

BloodHound and Neo4j were planned to visualize Active Directory relationships and attack paths.

Commands prepared:

sudo apt install bloodhound -y
sudo apt install neo4j -y
sudo systemctl start neo4j
sudo systemctl enable neo4j
sudo systemctl status neo4j
sudo apt install python3-pip pipx -y
pipx ensurepath
pipx install bloodhound-python
bloodhound-python --help
Issues Encountered

During the lab setup, several issues were encountered and resolved:

Netplan configuration warnings

Ubuntu unable to ping the Domain Controller

Network unreachable errors

pfSense routing and interface verification

Domain join failures

DNS and gateway misconfiguration

Package installation problems on Ubuntu

SSH and login issues on Kali after password changes

These problems were addressed through step-by-step troubleshooting and validation of routing, DNS, SSH, and service configuration.

Skills Practiced

Windows Server administration

Active Directory setup

DNS and DHCP configuration

Linux server configuration

Netplan configuration

Firewall and routing troubleshooting

Domain join troubleshooting

SSH remote administration

Network scanning with Nmap

SMB and LDAP enumeration

Cybersecurity lab design and documentation

Next Steps

Complete BloodHound installation and AD data collection

Continue installing and testing Kali security tools

Configure Ubuntu monitoring tools

Join Ubuntu fully to the domain if required

Add screenshots of each major step

Organize the project into GitHub folders:

/screenshots

/docs

/configs

README.md

Conclusion

This lab project is a practical environment for improving both system administration and cybersecurity skills. It combines Windows and Linux administration, network services, firewall configuration, and security testing in one structured setup. The project will continue with more tools, monitoring services, and attack simulation scenarios.
