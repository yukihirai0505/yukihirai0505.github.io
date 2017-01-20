---
layout: post
title:  "Hacker's school - Partial excerpt from some practical examples"
date:   2016-11-02 12:00:00 +0900
categories: Development
---

Today, I'd like to explain some words from Hacker's school.

## Network hacking

Speaking of hacking, the following are examples.

- Server intrusion
- Remote control
- Hacking web applications
- Eavesdropping on the network
- Denial of service attack

At a hacker's school, I explain it focusing on *** server intrusions ***.
 
If you can gain administrator privileges in order to succeed on intruding the server, you can freely access any file such as saved personal information or confidential information.
It also allows any action, such as invading further internal servers, as a reason for stepping into an external server.

Speaking of hacking, the following are examples.

- Server intrusion
- Remote control
- Hacking web applications
- Eavesdropping on the network
- Denial of service attack

At a hacker's school, I explain it focusing on *** server intrusions ***.
 
If you can gain administrator privileges in order to succeed on intruding the server, you can freely access any file such as saved personal information or confidential information.
It also allows any action, such as invading further internal servers, as a reason for stepping into an external server.

## Server intrusion process

- Targeting ... Official Web site, existence of files published by misconfiguration, mapping of networks, identification of version by application banner
- Scan ... Ping sweep, shared scan, OS specific, enumerate network resources, enumerate users
- Indirect attack ... If you intrude into the room where the target server exists, it is easier than grasping the system via the network. Listen for passwords from people by social engineering, infect remote controlled programs, sniff the network by Sniffing
- Direct attack ... attack directly target server. If you have a shell account on the target server, you aim to take administrator privileges. Exploit
- Cleaning up ... In order to delay security enhancement, it is doubtful to erase the log, so it is ideal to tamper with it. Since it is troublesome to make the same attack every time, set up a backdoor called a backdoor, easy to invade from the next time . Using a backdoor makes it easy to stepping on.
- Treasure hunting ... If it is a server of a company, it may be able to discover information of confidentiality (information on consignment, personal information, unpublished in-house product information etc.). Shared servers, mail servers, DB servers are likely. If you are developing software you can also get the source file from the repository.

## Kali Linux

Kali Linux is a Linux specialized in penestation and security and attack tools are installed as standard by Linux.
A successor version of the old BackTrack Linux OS.

Kali is about Hindu destruction god.

```
vagrant init starflame/kali2_linux4.0.0_amd64
vi Vagrantfile
```

```
config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = true

    # Customize the amount of memory on the VM:
    vb.memory = "1024"
end
```

```
vagrant up --provider virtualbox
```

## Top10 Security Tools

| Tool name | Overview |
| ------ | ------ |
| Aircrack-ng | password cracker for wireless LAN |
| Burpsuite | Local Proxy (Can change packet between client and server) |
| Hydra | High performance online password cracker |
| John | High performance Offline Password Cracker |
| Maltego | Social engineering support tool |
| Metasploit framework | Framework for creating and executing Exploit |
| Nmap | High performance port scanner |
| Owasp - zap | Penestration test tool for diagnosing vulnerabilities on web sites |
| Sqlmap | Tools for testing SQL injection vulnerabilities |
| Wireshark | famous packet capture |

## Optical submarine cable

Long distance wired cable across countries.
It is located in the waters of the whole world and its total extension exceeds 1 million km.
Life line of the Internet.
Increase at a rate of several tens of percent each year.

If a submarine cable corresponding to the main trunk fails or is broken, in some cases some countries may be completely blocked from the Internet.

## Ping

It is possible to determine whether or not the specified host exists on the network.
Ping is realized using the ICMP protocol.

It can be reached if an echo request message (type 8) is transmitted and an ICMP echo response message (type 0) is returned.
Other times it is unreachable.

## traceroute

Traceroute is a program to check the route to the specified host.
Or it is used to identify at which router a problem occurs when a network failure occurs.

## nslookup

Nslookup (name server lookup) is a DNS resolver program.
It displays the response from the DNS server on the screen.

## whois

Whois is to check the registered server information.
Information exists on the whois server, and by accessing that DB you can get information on the server administrator.

## host

Host is a command to inquire about information to be resolved on the DNS server.
Like nslookup and dig, it can be used for various investigations and debugging on DNS.

## Nmap

High performance port scanner.
Stealth scan, wide area scan, finger printing, and so on.
Matrix, Da · Hard 4.0, Battle · Royale, etc. It is also famous for appearing frequently in hacking scenes.

Nmap is already installed on Kali.

## Zenmap

Zenmap is GUI front end for Nmap.

## Netcraft

By doing WebServer Search on the Netcraft website
You can check the information on the specified web server.
Information on the type and version of httpd, and how long the specified Web server is running.

## Sniffing

Sniffing is to acquire packets exchanged in the LAN.
It refers to the wiretap act of the network.

## tcpdump

cpdump is a packet capture that works with CUI.

## Wireshark

Packet Analyzer.

## Cain & Abel

Cain & Abel is a password cracker. Works on Windows.

## Fiddler

Fiddler is a type of Local Proxy that can be used free.
HTTP communication from the browser becomes communication via Fiddler, it is possible to check the details of HTTP request and HTTP response, and to modify the contents of the request.
It works in .NET environment of Windows.

## Dictionary file

A dictionary file is a text file describing one word per line.
In password analysis, it is used for lexicographic attack.
That is, it is a text file that enumerates password candidates.

Sometimes DL Dictionary of Frequently Used Password Dictionary File from Net

## crunch

Crunch is software for generating dictionary files.
It runs on Unix and is installed by default on Kali.

## THC-Hydra

THC-Hydra is an online password cracker developed by The Hacker's Choice.
It runs on Linux.

## Metasploit

Metasploit is a framework for creating and executing Exploit.
It is used as a pen nation tool in the security world.

## w or who

The w command or the who command is a Unix command to check the currently logged in user.
The w command displays user information while logged in the same section as the who command but displays detailed information from the who command.

## Log deletion on Unix system

Programs that easily delete logs have existed from long ago.
Packet Storm has a category of log deletion tool,
so you can search for the latest program.

## Falsification of file time information

By using a special tool, it is possible to easily alter the time information.
However, in order to understand the file system, it is faster to do it manually.

- Time information of the file ... There are 3 types (atime, ctime, mtime)

Some file systems have crtime.

- atime ... last reference time
- ctime ... last state change time
- mtime ... last modification time
- crtime ... creation time

As a result of tampering atime and mtime, do not let the administrator notice the existence of illegal files.

```
$ ls -u  #it shows atime
$ ls -cl #it shows mtime
$ touch test.txt
$ touch -d "2014/9/11 00:11:22 pm" test.txt #it changes atime and mtime
$ touch -a -d "2014/9/11 00:11:22 pm" test.txt #it changes only atime
$ touch -m -d "2014/9/11 00:11:22 pm" test.txt #it changes only mtime
```

## Netcat

Netcat is a tool for reading and writing data using TCP / UDP.
You can listen to a port as a server or connect to a connection as a client and send data.
Sometimes it is used as a backdoor in the world of hacking.

## Cryptcat

Cryptcat realized encrypted communication Netcat.
A network listener that can operate with the same syntax as conventional Netcat.

## Finally

I read a hacker's school and found that there are many convenient toys around hacking and security.