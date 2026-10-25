# hidden

> **Challenge:** Can you find the hidden directory on the target web application?
>
> **Flag format:** Submit only the directory name.

---

## Walkthrough

### 1. Enumerate directories

Use `ffuf` to fuzz the target's directory paths with a wordlist of common names:

```bash
ffuf -u http://<ip/domain>/FUZZ -w /usr/share/wordlists/dirb/common.txt
```

- `-u` sets the target URL.
- `FUZZ` marks the part of the URL that will be replaced with wordlist entries.
- `-w` selects the wordlist.

### 2. Identify the hidden path

The scan returns several directories. Among them is the hidden directory requested by the challenge. Submit its name without the URL or a leading slash.

---

<details>
<summary><strong>🚩 Reveal Flag</strong></summary>

```text
secret
```

</details>
