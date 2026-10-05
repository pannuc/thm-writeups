# Introductory Researching — TryHackMe Writeup

| | |
|---|---|
| **Room** | Introductory Researching |
| **Link** | https://tryhackme.com/room/introtoresearch |
| **Difficulty** | Easy |
| **Time** | *45 Min* |
| **Topics** | Research skills, steganography, package managers, vulnerability databases, man pages |
| **Completed** | *10/05/2026* |

---

## Overview
Research is the single most-used skill in security work — you constantly hit tools, technologies, and vulnerabilities you've never seen before, and the job is knowing *where* to look and *how* to read what you find. This room drills that across four fronts: answering unfamiliar questions with a search engine, researching how to install and use a new tool (steganography with **steghide**), finding known vulnerabilities in **vulnerability databases**, and reading a tool's own **manual pages** without leaving the terminal.

---

## Example Research Question
A set of questions you can't answer from memory on day one — the point is to practice finding the answer quickly and trusting good sources. What each one actually is matters more than the word itself:

| Research question | Answer | What it is |
|---|---|---|
| In Burp Suite, which mode manually crafts/resends a request? | **Repeater** | Burp's tab for hand-editing and replaying individual HTTP requests — a core web-testing workflow |
| What format are modern Windows login passwords stored in? | **NTLM** | The hash algorithm behind Windows credentials |
| What are automated/scheduled tasks called in Linux? | **Cron Jobs** | Tasks run on a schedule by the cron daemon (configured in crontab) |
| What base is shorthand for base 2 (binary)? | **Base 16** | Hexadecimal — one hex digit maps to exactly 4 binary bits, so it compresses binary cleanly |
| What hashing format does a `$6$` prefix indicate? | **sha512crypt** | The Unix SHA-512 crypt scheme |

---

## Researching & Using an Unfamiliar Tool — Steganography with steghide

**Steganography** is the practice of hiding data *inside* other data — here, concealing a file or message within an image so that its very existence isn't obvious. It's worth drawing the line clearly:

- **Encryption** hides the *content* of a message (you can see something is there, you just can't read it).
- **Steganography** hides the *existence* of the message (nothing looks out of place at all).

The two are often combined — encrypt first, then hide the ciphertext. This section is really a worked example of the research loop you run on *any* new tool: find its prerequisites, install it, then learn its usage.

**Installing it with apt.** `apt` (Advanced Package Tool) is the package manager used by Debian-based Linux distributions like **Ubuntu** and **Kali**. It pulls tools — and whatever dependencies they need — from the distro's repositories, so you don't hunt them down by hand:

```bash
sudo apt install steghide
```

**Using steghide.** The two operations you'll care about are embedding and extracting (you'd learn these flags from steghide's help/man page — which is the whole research point):

```bash
# Hide secret.txt inside image.jpg (prompts for a passphrase)
steghide embed -cf image.jpg -ef secret.txt

# Extract whatever is hidden inside image.jpg (prompts for the passphrase)
steghide extract -sf image.jpg
```

`-cf` is the **cover file** (the carrier image), `-ef` is the **embed file** (the secret), and `-sf` is the **stego file** (the image that already has something hidden in it).

---

## Vulnerability Searching
When you're looking at software that might be exploitable, a few databases do different jobs — and good research chains them together rather than treating them as interchangeable:

- **CVE (MITRE)** — the system that assigns each publicly known vulnerability a unique ID (`CVE-YEAR-NUMBER`) plus a short description. This is the **common reference name** so everyone's talking about the same bug.
- **NVD (NIST)** — the National Vulnerability Database enriches each CVE with a **CVSS severity score**, affected-product data, and references. Where CVE is the ID, NVD is the **scored analysis**.
- **Exploit-DB (OffSec)** — an archive of actual **exploit code and proof-of-concepts**. Where CVE/NVD describe the flaw, Exploit-DB often has the working code.
- **searchsploit** — the offline, command-line front end to Exploit-DB (ships with Kali). It searches a local copy of the database straight from the terminal, e.g.:

```bash
searchsploit fuel cms
```

**The CVEs found in this task:**

| Software / vulnerability | CVE |
|---|---|
| WPForms — 2020 Cross-Site Scripting (XSS) | **CVE-2020-10385** |
| Apache Tomcat (Debian package) — 2016 local privilege escalation | **CVE-2016-1240** |
| VLC media player — the very first CVE | **CVE-2007-0017** |
| sudo — buffer overflow (the `pwfeedback` flaw, exploited in 2020) | **CVE-2019-18634** |

---

## Manual Pages (`man`)
Before reaching for a search engine, remember the documentation is already on the machine. `man <command>` opens that command's **manual page** — the authoritative local reference for its syntax (the SYNOPSIS section) and its switches (the OPTIONS section). To jump straight to the switch you need, pipe the page to `grep` or search inside the pager:

```bash
man ssh | grep -i "port"      # find the port-related options in ssh's manual
# or, inside an open man page, type:  /port   then press n to cycle matches
```

**The switches looked up in this task:**

| Tool & task | Switch / command |
|---|---|
| `scp` — copy an entire directory (recursive) | **-r** |
| `fdisk` — list the current partitions | **-l** |
| `nano` — make a backup when opening a file | **-b** |
| `netcat` — start listen mode on port 12345 | **nc -l -p 12345** |

(For netcat, `-l` sets **listen** mode and `-p` sets the **port** — the two combine to make the machine wait for an incoming connection on 12345.)

---

## Key Takeaways
- Research is *the* core security skill: search engines for quick facts, vulnerability databases for known flaws, and a tool's own docs **first**.
- **Steganography hides that data exists** (inside an image); **encryption hides what the data says** — different jobs, often stacked together.
- `apt` installs a tool *and its dependencies* on Debian-based distros (Ubuntu, Kali): `sudo apt install <tool>`.
- **CVE = the ID, NVD = the scored analysis, Exploit-DB / searchsploit = the working exploit** — chain them during recon.
- `man` is the authoritative offline reference for any command's switches; pipe it to `grep` to find a specific flag fast.
