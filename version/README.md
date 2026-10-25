# version

> **Challenge:** Identify the version of the web server running on the target.
>
> **Flag format:** `Xxxxxxy.y.yy` (example: `Nginx3.4.11`)

---

## Walkthrough

### 1. Scan for services and versions

Use Nmap to identify open ports and query the services for version information:

```bash
nmap <target_ip> -p- -T5 -sV
```

The options used are:

- `-p-` — scan all TCP ports.
- `-T5` — use Nmap's fastest timing template.
- `-sV` — probe open ports to identify service versions.

### 2. Read the web server banner

The scan identifies an **Apache HTTP Server** running on port `80`. The service banner includes its version and the platform designation `(UNIX)`.

Following the required flag format, submit the server name immediately followed by its version number, omitting the extra banner text.

---

<details>
<summary><strong>🚩 Reveal Flag</strong></summary>

```text
Apache2.4.29
```

</details>
