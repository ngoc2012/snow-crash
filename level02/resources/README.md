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

sudo apt install tshark

Command to extract only the data field
```bash
sudo tshark -r level02.pcap -T fields -e data > payloads.txt
```

-r input.pcap: your capture file
-T fields: output fields only
-e data: extract only the data field (i.e., the payload)



https://www.rapidtables.com/convert/number/hex-to-ascii.html


ÿý%ÿü%ÿû&ÿýÿý ÿý#ÿý'ÿý$ÿþ&ÿûÿû ÿû#ÿû'ÿü$ÿú ÿðÿú#ÿðÿú'ÿðÿúÿðÿú  38400,38400ÿðÿú# SodaCan:0ÿðÿú'  DISPLAYSodaCan:0ÿðÿú xtermÿðÿûÿýÿý"ÿýÿûÿý!ÿýÿüÿû"ÿú"  b  b	B
ÿÿÿÿÿðÿûÿú ± 1ÿðÿýÿû!ÿú"ÿðÿú"ÿðÿú!ÿðÿûÿý ÿþ"ÿýÿû ÿü"ÿú"ââ	Â
ÿÿÿÿÿð
Linux 2.6.38-8-generic-pae (::ffff:10.1.1.2) (pts/10)

 wwwbugs login: l le ev ve el lX X
 
Password: ft_wandrNDRelL0L
 
 
Login incorrect
wwwbugs login: 


### View invisible chracters

https://www.soscisurvey.de/tools/view-chars.php

copy paste the output from rapidtables.com

Password: ft_wandr[7f][7f][7f]NDRel[7f]L0

177   127   7F    DEL

### get the flag
```bash
level01@SnowCrash:~$ su flag02
Password: ft_waNDReL0L
Don't forget to launch getflag !
flag02@SnowCrash:~$ getflag 
Check flag.Here is your token : kooda2puivaav1idi4f57q8iq

```