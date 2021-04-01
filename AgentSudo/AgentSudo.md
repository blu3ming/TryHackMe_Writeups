# Agent Sudo - WriteUp
##https://tryhackme.com/room/agentsudoctf

+ **We start with a nmap scan (TCP-SYN) for open ports (no host discovery or dns resolution)**
  
    ``nmap -sS --min-rate 5000 -p- --open -vvv -n -Pn 10.10.33.214 -oG allPorts``

+ **We see that FTP, SSH and HTTP is open**

    ![1]
    
+ **With the help of whatweb, we try to enumerate for any CMS or software the web server is running. Does not report anything relevant to us**

    ![2]
    
+ **We enumerate for versions with basic enumeration scripts in nmap for the services running**

    ``nmap -sC -sV -p21,22,80 10.10.33.214 -oN targeted``

    ![3]
    
[1]:./images/1.png
[2]:./images/2.png
[3]:./images/3.png
[4]:./images/4.png
[5]:./images/5.png
