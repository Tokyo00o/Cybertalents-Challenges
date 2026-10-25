# Linuxcmd 101

> **Category:** Forensics / Linux Fundamentals · **Challenge Type:** Command-Line Investigation

---

## Challenge Description

> This Challenge will help you understand essential commands in Linux OS, Each point is linked to another point, connect the link and win the Flag!

**Challenge archive:** [`linux_chal.tar.gz`](https://dr-hubchallenges.s3.us-west-2.amazonaws.com/Forensics/linux_chal.tar.gz)

The challenge follows a sequence of files, protected archives, and encoded clues. Each discovery leads to the next part of the chain.

## Walkthrough

### 01 — Extract the Initial Archive

The downloaded file has two archive layers: gzip compression around a tar archive. Remove them in order:

```bash
gzip -d linux_chal.tar.gz
tar -xf linux_chal.tar
```

A directory named `cat` is created. Inspect it, including hidden files:

```bash
cd cat
ls -la
```

```text
.pass.txt
exec.zip
```

### 02 — Recover the Password for `exec.zip`

The hidden `.pass.txt` file contains the first password:

```bash
cat .pass.txt
```

```text
2434237800
```

Extract `exec.zip`, entering the password when prompted:

```bash
unzip exec.zip
```

### 03 — Inspect the Executable

Move into the extracted `exec` directory:

```bash
cd exec
ls -la
```

Alongside `ascii.zip` is a file named `-`. Because the hyphen can be interpreted specially by command-line utilities, use the explicit path `./-`:

```bash
file ./-
```

The file is identified as an **ELF executable**. Executing this challenge-provided file reveals another password:

```bash
./-
```

```text
Here is your password: 'passforasciiii'
```

Use this password to extract `ascii.zip`:

```bash
unzip ascii.zip
```

### 04 — Examine the `f0`–`f8` Files

The extraction reveals `size37.zip` and files named `f0` through `f8`. Check their types in one command:

```bash
file f*
```

The file `f6` is text. Read it:

```bash
cat f6
```

```text
thisisasciiiiiprintapleeeee
```

That string unlocks `size37.zip`:

```bash
unzip size37.zip
```

### 05 — Read the Next Set of Clues

The extracted files include `next.zip` and `test1` through `test7`. First, inspect their types; then combine their contents into one output:

```bash
file test*
cat test*
```

The text includes the password encountered in the previous step:

```text
thisisasciiiiiprintapleeeee
```

Reuse it to extract `next.zip`:

```bash
unzip next.zip
```

### 06 — Investigate `NumberOne.zip`

This stage produces two notable files:

```text
NumberOne.zip
nexttocybertalents
```

The second file resembles a wordlist. Inspect it:

```bash
file *
cat nexttocybertalents
```

A dictionary attack with John the Ripper was attempted:

```bash
zip2john NumberOne.zip > lock
john lock --wordlist=nexttocybertalents
```

The attempt did **not** find the password. Manual examination of the wordlist revealed an unusual longer candidate ending in `orderby1337`. Testing that string successfully unlocked `NumberOne.zip`.

```text
orderby1337
```

### 07 — Open the Remaining Protected Archive

The next stage contains another password-protected ZIP. Its password was recovered through brute force:

```text
rocky
```

After extracting it, two more files appear:

```text
pass
decodeme.zip
```

### 08 — Decode the ZIP Password

Reading `pass` returns an encoded string:

```bash
cat pass
```

```text
dXNlbWVhc3Bhc3N3b3Jk
```

Using the string directly as a ZIP password does not work. The name `decodeme.zip` suggests a decoding step, and the string is valid **Base64**.

Decode it with:

```bash
echo 'dXNlbWVhc3Bhc3N3b3Jk' | base64 -d
```

```text
usemeaspassword
```

Enter the decoded password when extracting the archive:

```bash
unzip decodeme.zip
```

### 09 — Decode the Final String

The extracted content contains a flag-shaped string:

```text
synt{f1zcyr_yvahk_101}
```

This is not the value accepted by the challenge. The letters are transformed using **ROT13**, a substitution that rotates each alphabetic character by 13 positions.

Decode the string in the terminal:

```bash
echo 'synt{f1zcyr_yvahk_101}' | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

The decoded result is the final flag.

## Flag

<details>
<summary><strong>🚩 Reveal Flag</strong></summary>

```text
flag{s1mple_linux_101}
```

</details>
