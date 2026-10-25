# Birdsweet

> **Challenge:** I am tired of using Firefox, so I made Birdsweet!

---

## Walkthrough

### 1. Change the browser identity

Opening the provided link displays the message:

> This website only works if you have the birdsweet browser (or an IQ of 130+)

There is no standard browser named *Birdsweet*, but the browser identity sent with an HTTP request can be changed through the `User-Agent` header.

Intercept the request, send it to **Repeater**, and set:

```http
User-Agent: birdsweet
```

The server responds with another hint:

> If I remember correctly this button will take you to the flag, but i think it is broken

### 2. Inspect the button's request

Inspecting the page source shows that the button sends a request to `/` with an `id` query parameter. Begin with:

```http
GET /?id=1
```

The response says `no flag here`. Changing the ID manually does not immediately identify the correct value.

### 3. Enumerate the IDs

Send the request to **Intruder** and mark the `id` value as the payload position. Configure a numeric payload covering **0–100**, then start the attack.

Filter the results by **response length**. One response stands out: `id=26`.

### 4. Decode the response

The matching response contains encoded data. Convert the hexadecimal content to text using a [hex-to-ASCII converter](https://www.rapidtables.com/convert/number/hex-to-ascii.html), then decode the resulting Base64 data with [CyberChef](https://gchq.github.io/CyberChef/).

---

<details>
<summary><strong>🚩 Reveal Flag</strong></summary>

```text
Flag{QCFATjNDUFg5ZHhIK0ZHU0d3WHhYSFNiNE5jU0FNVUJDWDIxWkFkOWtjMkZhVlJNZlpnMGhuQjJSNWtJaXpFamcyeUkyNzRZTjZHTnJrRlEvNktOSHpWMFE9PTFjMmQ5MmZkODMzZmU2MGI=}
```

</details>
