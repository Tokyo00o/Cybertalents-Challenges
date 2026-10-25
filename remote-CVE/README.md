# remote-CVE

> **Challenge:** Which CVE could be used against the web application running on the target?
>
> **Hint:** The vulnerability allows unauthenticated remote code execution (RCE).
>
> **Flag format:** `CVE-xxxx-xxxx`

---

## Walkthrough

### 1. Identify the web application

The supplied target presents a login page. Inspecting the site's technology with the **Wappalyzer** browser extension identifies **Drupal 7**.

### 2. Match the technology to the vulnerability

Searching for unauthenticated RCE vulnerabilities affecting Drupal 7 points to **Drupalgeddon 2**.

The CVE identifier associated with that vulnerability is the required answer.

---

<details>
<summary><strong>🚩 Reveal Flag</strong></summary>

```text
CVE-2018-7600
```

</details>
