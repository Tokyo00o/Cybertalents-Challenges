# Invincible

> **Challenge:** This challenge introduces the basics of OSINT. The threat intelligence team has noticed suspicious behavior from the user `soremanzo`. Can you trace the account and discover what they are hiding?

---

## Walkthrough

### 1. Search for the username

Start with a username search. The [OSINT Framework](https://osintframework.com/) provides a collection of resources for this kind of investigation, and [NameCheckup](https://namecheckup.com/) can be used to check where a username appears online.

Searching for `soremanzo` points to a **Disqus** profile.

### 2. Follow the clue in the profile

Open the profile at:

```text
https://disqus.com/by/soremanzo/
```

Its **About** section suggests the flag was deleted, along with the clue:

> Not all what you need will be on your way back

This points toward checking an older version of the page rather than relying on its current contents.

### 3. Look through the archived snapshots

Enter the profile URL into the [Wayback Machine](https://web.archive.org/). The snapshot dated **May 5, 2021** contains the flag.

---

<details>
<summary><strong>🚩 Reveal Flag</strong></summary>

```text
FLAG{4rch1v3_Usu4LLy_C0nt41n_Us3fuL_1nf0rm4t10n}
```

</details>
