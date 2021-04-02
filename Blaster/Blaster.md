# Blaster - WriteUp
## https://tryhackme.com/room/blaster

**Note: This writeup does not contemplate the metasploit stage.**

+ **The machine appears to be behind a firewall, because it refuses all ICMP traces.**

    We use tcpping to try to reach the machine, and it works. So, the machine is up.
  
    ![1]

+ **We scan for open ports with nmap (no DNS resolution, no host discovery and with a TCP-SYN scan).**

    ``nmap -sS --min-rate 5000 -p- --open -vvv -n -Pn 10.10.20.34 -oG allPorts``

    ![2]
    
+ **If we try to scan for versions with nmap, it says that the ports are filtered now.**

    ``nmap -sC -sV -p80,3389 10.10.20.34 -oN targeted``
    
    ![3]
    
    There's nothing more we can do here.
    
+ **If we scan the web service with whatweb, we see that it runs in an IIS Windows Server.**

    ![4]
    
+ **Let's make a scan for hidden directories in the web service. We are going to use gobuster.**

    ``gobuster dir --url http://10.10.20.34 --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt --no-error -t 50``
    
    ![5]
    
    We see a directory: **retro**.
    
+ **Let's visit the web page. We see the default page for an IIS, so let's go to the /retro directory.**

    ![6]
    
    ![7]
    
+ **We see that all the post were written by an user called "Wade".**

    ![8]
    
+ **If we try to login with that username (doesn't matter the password) we get the next error:**

    ![9]
    
    Wordpress says that the password for that user is incorrect. From this, we now know that **wade** is a valid user.
    
+ **If we investigate further in the website, we encounter with a post called "Ready Player One".**

    ![10]
    
    Read carefully, Wade says: "I keep mistyping the name of his avatar whenever I log in..."
    
    If you have read (or watched the movie) "Ready Player One", you know that the avatar of Wade in OASIS is "XXXXXXXX".
    
+ **So, let's try to login with that password, and the username wade. It works.**
    
    ![11]
    
    We are now in the admin panel of Wordpress. We can upload a malicious php file to get a reverse shell. But let's try another thing.
    
+ **The another open port in the machine was the 3389. This is used for Windows Remote Desktop.**

    So, let's try to login to this service with the credentials obtained for the Wordpress service. We are going to use Remmina.
    
    We just need to put the IP and the credentials, and we are now connected to the remote machine.
    
    ![12]
    
    Here we can see the user flag in the Desktop.
    
    ![13]
    
+ **We can see too an executable file in the Desktop, called "hhupd".**

    If we google it, we can see a CVE as result. The CVE-2019-1388.
    
    ![14]
    
    The process to exploit this vulnerability is described in this [video](https://www.youtube.com/watch?v=3BQKpPNlTSo)
    
+ **First, we need to right click this executable, and run it as administrator.**

    Of course, it will ask for a password, but doesn'a matter. We need to click on the blue text that says: "Show information about the publisher's certificate".
    
    ![15]
    
    We see inormation about the certificate of the executable. We must click on the hyperlink next to "Issued by:" label.
    
    ![16]
    
+ **It redirects to a webpage, doesn'a matter if you have internet or not. We must click on the gear icon (right upper corner, next to the "star" icon).**

    Select "Save page as..."
    
    ![17]
    
+ **It will open a new window with an error. Just click on OK (this means that the exploit is going to work).**

    ![18]
    
+ **In the "File name:" field, write:**

    ``C:\Windows\System32\*.*``
    
    And click Enter.
    
    ![19]
    
+ **The window will open the system32 folder. Just look for the cmd.exe, do a right click on it and select "Open" (not "Run as admin", just "Open").**

    ![20]
    
+ **Now, we have a cmd console as the admin user. You can check it with the command "whoami".**

    ![21]
    
+ **The flag is in the Desktop folder of the Admin user.**

    ![22]
    
[1]:./images/1.png
[2]:./images/2.png
[3]:./images/3.png
[4]:./images/4.png
[5]:./images/5.png
[6]:./images/6.png
[7]:./images/7.png
[8]:./images/8.png
[9]:./images/9.png
[10]:./images/10.png
[11]:./images/11.png
[12]:./images/12.png
[13]:./images/13.png
[14]:./images/14.png
[15]:./images/15.png
[16]:./images/16.png
[17]:./images/17.png
[18]:./images/18.png
[19]:./images/19.png
[20]:./images/20.png
[21]:./images/21.png
[22]:./images/22.png
