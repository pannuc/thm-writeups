# Learning Cyber Security — TryHackMe Writeup

| | |
|---|---|
| **Room** | Learning Cyber Security |
| **Link** | https://tryhackme.com/room/beginnerpathintro |
| **Difficulty** | Easy |
| **Time** | ~45 min |
| **Topics** | Web app security, account takeover, HTTP requests, network security, cost of a breach |
| **Completed** | *10/03/2026* |

---

## Overview
A taster room that previews three sides of cyber security: breaking into a web app (offensive), understanding why networks matter (defensive), and seeing the real-world damage an attack causes. The hands-on parts are simulated in the browser — no tooling required — but the concepts underneath are the real thing.

---

## Task 1 — Web Application Security: Hacking BookFace

**The idea:** You can't attack a web app without understanding how it works. Hacking a site comes down to understanding how one piece of it functions and spotting a **vulnerability** — a weakness that can be taken advantage of. The target here is **BookFace**, TryHackMe's vulnerable Facebook replica, and the weak spot is its **password reset** feature.

**What I did:** Triggered a password reset on BookFace and inspected the HTTP request the browser sent. The exercise breaks that request into its parts:

| Component | What it is | Why it matters here |
|-----------|------------|---------------------|
| **Request method — `POST`** | Submits data *to* the server to create or change something; the data rides in the request **body** rather than the URL | A password reset is a state-changing action carrying account data, so it uses POST (not GET, which just requests data) |
| **Host** | The domain the request is being sent to — tells the server which site should handle it | Confirms the request is actually hitting BookFace |
| **User-Agent** | Identifies the client making the request (browser, version, OS) | Servers log and sometimes branch on it; an attacker can spoof it to disguise their tooling |
| **Request field (body/data)** | The actual submitted parameters — here, the **username** of the account being reset | This is the field you tamper with |

**Why it's exploitable:** BookFace decides *whose* password to reset based on the username supplied in the request — and it never verifies that the person sending the request actually owns that account. Change the username field to someone else's account, submit it, and the reset is applied to **their** account instead of yours. That's an account takeover.

Conceptually this is **broken access control**: the server performs a sensitive action from client-controlled input without an authorization check. The fix is the lesson — never trust client input, and authorize sensitive actions server-side against the *authenticated* user, not a value the user can freely edit. This is also the core idea behind request interception and tampering that tools like Burp Suite are built for, which you'll meet later in the path.

**Answers**
- Username of the BookFace account being taken over: *Ben.Spring*
- Task flag revealed after the takeover: *THM{BRUTEFORCING}*

---

## Task 2 — Network Security & The Cost of an Attack

**Why networking matters:** Networking shows up on both sides of security. Offensively, you **scan** a network to identify who and what is on it. Defensively, you **review network logs** to monitor activity and trace what users (or attackers) have done. You can't do either without understanding how networks work.

**The case study — the Target breach:** The room walks through how retail giant Target was compromised, which is a clean illustration of what an attack actually costs:

- Attackers first got in through a **third-party vendor** (Target's HVAC contractor), then pivoted across the internal network and planted malware on the **point-of-sale (POS)** systems.
- Roughly **40 million** payment card numbers and **70 million** customers' personal records were exposed.
- The fallout ran into the **hundreds of millions of dollars** — breach remediation, legal settlements and fines, lost sales, and lasting reputational damage. Senior executives (including the CEO and CIO) ultimately lost their jobs.

**The takeaway:** The cost of an attack is far more than the data stolen in the moment — it's the cleanup, the legal and regulatory bill, the lost customer trust, and the long-term hit to the brand. It also shows two structural lessons: **third-party/supply-chain risk** (your security is only as strong as the vendors you connect to) and the value of **network segmentation** (vendor access should never have reached the POS systems).

**Answer**
- Cost of the Target data breach: *$300 million*

---

## Task 3 — Learning Roadmap
The room closes by laying out the TryHackMe path: start with **Pre Security** for the technical fundamentals, then branch into either **Offensive Pentesting** (ethically hacking systems) or **Cyber Defense** (investigating attacks and defending systems). Those paths point toward roles like ethical hacker, penetration tester, or security analyst.

---

## Key Takeaways
- A **vulnerability** is just a weakness that can be exploited — hacking is finding and leveraging one.
- Understanding the **anatomy of an HTTP request** (method, host, headers, body) is what lets you spot and abuse web app flaws.
- **Never trust client input** — tampering with a request field broke BookFace's password reset because authorization wasn't enforced server-side.
- Networking knowledge underpins both **attacking** (scanning) and **defending** (log analysis).
- A breach's real cost is remediation, fines, lost trust, and brand damage — usually far more than the initial theft.
