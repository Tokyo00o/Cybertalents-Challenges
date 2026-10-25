# The Restricted Sessions

> **Challenge:** The flag is restricted to logged-in users only. Can you become one of them?

---

## Walkthrough

### 1. Inspect the authentication flow

The provided page only displays the flag to authenticated users. Inspecting its client-side script reveals a request to `getcurrentuserinfo.php`, which uses the `PHPSESSID` session identifier to retrieve user information.

Requesting `getcurrentuserinfo.php` directly does not initially reveal anything useful.

### 2. Investigate the session store

Adding a random `PHPSESSID` value produces the following message:

```text
Session not found in data/session_store.txt
```

Following the disclosed path reveals session entries belonging to other users. Trying those sessions on the main page suggests that another parameter, `UserInfo`, is also required. An arbitrary value results in a validation failure.

### 3. Retrieve the associated user information

Return to `getcurrentuserinfo.php` and make a `POST` request, placing one of the session IDs from `data/session_store.txt` in the request body as `PHPSESSID`.

This returns the username associated with the session. Using the session information on the main page allows access to the restricted flag.

---

<details>
<summary><strong>🚩 Reveal Flag</strong></summary>

```text
sessionareawesomebutifitsecure
```

</details>
