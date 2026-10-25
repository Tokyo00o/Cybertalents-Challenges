# Open Doors

> **Challenge:** Identify how many ports below 100 are open on the target.
>
> **Flag format:** `md5(ports)` — the MD5 hash of the number of open ports.

---

## Walkthrough

### 1. Scan the requested port range

Run an Nmap scan against ports 0 through 100:

```bash
nmap <ip> -p0-100 -T5
```

- `-p0-100` specifies the port range.
- `-T5` selects the fastest timing template.

### 2. Count the open ports

The scan identifies two open ports:

```text
22/tcp
80/tcp
```

The total is **2**. Since the challenge requests `md5(ports)`, hash the count itself rather than the port numbers.

---

<details>
<summary><strong>🚩 Reveal Flag</strong></summary>

```text
c81e728d9d4c2f636f067f89cc14862c
```

</details>
