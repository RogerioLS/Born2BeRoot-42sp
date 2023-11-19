<div align = center>

# :anchor: Born2BeRoot | 42 SP

![42 São Paulo](https://img.shields.io/badge/42-SP-1E2952)
![License](https://img.shields.io/github/license/mendes-jv/libft?color=dark-green)
![Last commit](https://img.shields.io/github/last-commit/mendes-jv/libft?color=dark-green)

</div>

---

<div align = center>

![](https://game.42sp.org.br//static/assets/achievements/born2beroote.png)


</div>

In this project I learned the basics of virtualisation, system administration and security policies in order to set up my very first basic SSH server!
The virtual machine was set up in Oracle Virtualbox. The chosen operating system was the latest stable release of Debian GNU/Linux (bullseye), and I used `apt` package manager to install the tools used to enforce minimal security policies, such as:
- setting up a firewall using `UFW`
- configuring system administration with `sudo`
- implementing strong password policy using `libpam-quality`
- setting up a `cron` task to run a monitoring script every 10 minutes

This project also covered the basics of **criptography**, **networking**, **hard disk partitioning**, and **shell scripting** (lots of output formatting!)

## Summary
Password policies:
- minimum 10 characters
- must contain at least 1 upper case character and 1 digit
- must not contain username
- must be at least 7 chars different from one password update to the other
- must not contain more than 3 consecutive characters
- expires in 30 days
- minimum 2 days to change password
- system warning 7 days prior do expiry date

Firewall:
- all ports must be closed except for port 4242 

SSH:
- default port changed to port 4242
- must not be able to connect using SSH as root

Monitoring:

```pthon
#!/bin/bash
arc=$(uname -a)
pcpu=$(grep "physical id" /proc/cpuinfo | sort | uniq | wc -l) 
vcpu=$(grep "^processor" /proc/cpuinfo | wc -l)
fram=$(free -m | awk '$1 == "Mem:" {print $2}')
uram=$(free -m | awk '$1 == "Mem:" {print $3}')
pram=$(free | awk '$1 == "Mem:" {printf("%.2f"), $3/$2*100}')
fdisk=$(df -BG | grep '^/dev/' | grep -v '/boot$' | awk '{ft += $2} END {print ft}')
udisk=$(df -BM | grep '^/dev/' | grep -v '/boot$' | awk '{ut += $3} END {print ut}')
pdisk=$(df -BM | grep '^/dev/' | grep -v '/boot$' | awk '{ut += $3} {ft+= $2} END {printf("%d"), ut/ft*100}')
cpul=$(top -bn1 | grep '^%Cpu' | cut -c 9- | xargs | awk '{printf("%.1f%%"), $1 + $3}')
lb=$(who -b | awk '$1 == "system" {print $3 " " $4}')
lvmu=$(if [ $(lsblk | grep "lvm" | wc -l) -eq 0 ]; then echo no; else echo yes; fi)
ctcp=$(ss -neopt state established | wc -l)
ulog=$(users | wc -w)
ip=$(hostname -I)
mac=$(ip link show | grep "ether" | awk '{print $2}')
cmds=$(journalctl _COMM=sudo | grep COMMAND | wc -l)
wall "	#Architecture: $arc
	#CPU physical: $pcpu
	#vCPU: $vcpu
	#Memory Usage: $uram/${fram}MB ($pram%)
	#Disk Usage: $udisk/${fdisk}Gb ($pdisk%)
	#CPU load: $cpul
	#Last boot: $lb
	#LVM use: $lvmu
	#Connections TCP: $ctcp ESTABLISHED
	#User log: $ulog
	#Network: IP $ip ($mac)
	#Sudo: $cmds cmd"

```

## Submission 
This projects submission is a SHA hash of the virtual machine in its final state prior to evaluation. It was necessary to take a snapshot within Virtualbox to save the VM state, as it would be altered during each project defense.
I've included in this repository the shell script used to make the monitoring panel as well.

## Final note
If you're a 42 student struggling to understand this project, I got your back! Please refer to this Notion page that I wrote covering the general concepts related to this project! It's in Portuguese :cactus:

NOTE:The author of the manual made in Portuguese is the student [Rods](https://github.com/rodsmade).

> [Acelera — Born2BeRoot](https://rodsmade.notion.site/Acelera-Born2BeRoot-99adac7a7bdc4bbf81b4eaf977625d5c)