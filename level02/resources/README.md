# level02

On VM
```bash
level02@SnowCrash:~$ ls
level02.pcap
```

On host
```

To install Wireshark on Ubuntu, start by updating your package list with the command sudo apt update.
Next, install Wireshark using sudo apt install wireshark.
During the installation, you will be prompted to decidewhether non-superusers should be able to capture packets.If you choose 'Yes', you must add each user to the'wireshark' user group by running sudo usermod -a -Gwireshark "$USER" and then log out and log back in toapply the changes.
=> choose "No"

### Copy file level02.pcap from VM

```bash
minh-ngu@ngoc:~$ scp '-P 4242' level02@192.168.45.137:level02.pcap .
	   _____                      _____               _     
	  / ____|                    / ____|             | |    
	 | (___  _ __   _____      _| |     _ __ __ _ ___| |__  
	  \___ \| '_ \ / _ \ \ /\ / / |    | '__/ _` / __| '_ \ 
	  ____) | | | | (_) \ V  V /| |____| | | (_| \__ \ | | |
	 |_____/|_| |_|\___/ \_/\_/  \_____|_|  \__,_|___/_| |_|
                                                        
  Good luck & Have fun

          192.168.45.137 
level02@192.168.45.137's password: 
level02.pcap                            100% 8302     4.0MB/s   00:00    
minh-ngu@ngoc:~$ ls
 Darkly     'factures LM'   Public       snap        'VirtualBox VMs'
 Desktop     level02.pcap   pw.txt       snow-crash
 Documents   Music          Red1         Templates
 Downloads   Pictures       Red-tetris   Videos

```

### get the flag
```bash
level01@SnowCrash:~$ su flag01
Password: 
Don't forget to launch getflag !
flag01@SnowCrash:~$ getflag
Check flag.Here is your token : f2av5il02puano7naaf6adaaf
```