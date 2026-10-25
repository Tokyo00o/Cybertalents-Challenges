# WPA Crack

## Challenge

> You are conducting a Wi-Fi penetration test. A handshake has been captured, and the objective is to crack the password.

**Provided file:** `dump-01.cap`  
**Flag format:** The recovered password.

---

## Walkthrough

The provided `.cap` file contains the captured Wi-Fi handshake. A dictionary attack can be performed against the capture using `aircrack-ng` and the `rockyou.txt` wordlist.

```bash
aircrack-ng dump-01.cap -w /usr/share/wordlists/rockyou.txt
```

After processing the capture against the wordlist, the password is recovered.

---

<details>
<summary><strong>🚩 Reveal Flag</strong></summary>

```text
biscotte
```

</details>
