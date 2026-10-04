# Starting Out In Cyber Sec — TryHackMe Writeup

| | |
|---|---|
| **Room** | Starting Out In Cyber Sec |
| **Link** | https://tryhackme.com/room/startingoutincybersec |
| **Difficulty** | Easy |
| **Time** | ~10 min |
| **Topics** | Career paths, offensive security, defensive security, red team vs blue team |
| **Completed** | *10/04/2026* |

---

## Overview
A short, no-machine room that maps out the two big halves of the cybersecurity industry — **offensive** (attacking systems to find weaknesses) and **defensive** (detecting and stopping attacks) — and the career roles that live under each. The goal is to help you figure out which direction fits how you think before you commit to a learning path.

---

## Task 1 — Welcome
Intro task — no question to answer, just orientation. The room previews the career tracks and points toward TryHackMe's structured learning paths vs. picking individual rooms from Hacktivities.

**Answer:** No answer needed.

---

## Task 2 — Offensive Security

The **offensive** side is about actively attacking applications and technologies to **discover vulnerabilities** — ideally before a real attacker does. It tends to suit people who like understanding how things work, think analytically, and approach problems from unexpected angles.

The flagship role here is the **penetration tester**: someone **legally employed by an organisation to find vulnerabilities in their products**. That "legally employed" part is the whole distinction between a pentester and a criminal hacker — the techniques overlap heavily, but the pentester operates with **authorization, a defined scope, and a report at the end**. Permission is what makes it ethical.

A pentester needs a *broad* base of knowledge rather than one deep specialty, at least to start:
- **Web application security** — how sites work and where they break
- **Network security** — scanning, services, and how networks are attacked
- **Programming/scripting** — to automate tasks and write custom tooling and exploits
- **Cloud security** — increasingly important as organisations move infrastructure to providers like AWS and Azure

You can specialise later, but a wide foundation is the better way in. This is "**red team**" territory.

**Q: What is the name of the career role that is legally employed to find vulnerabilities in applications?**
**A:** Penetration tester

---

## Task 3 — Defensive Security

The **defensive** side is the mirror image: instead of finding vulnerabilities, you **detect and stop attacks**. It suits analytical problem-solvers. A recurring theme across every defensive role is that you can't spot an attack unless you first understand how the underlying technology is *supposed* to work — you detect the abnormal by knowing the normal. Three roles the room highlights:

- **Security Analyst** — monitors the organisation's systems and detects whether any of them are under attack. Day-to-day, this means watching logs and alerts (often through a **SIEM** like Splunk) and recognising what an attack looks like against the tech being monitored. This is the classic entry-level **SOC / blue team** role.
- **Incident Responder** — brought in *after* an attack has happened. Their job is to work out **what the attacker did and what the impact is**, by analysing the trace evidence left behind (digital forensics). Where the analyst catches it, the responder reconstructs it.
- **Malware Analyst** — a more specialist role focused on dissecting malicious software to understand exactly what it does. Since attackers use malware at every stage — from gaining initial access to **maintaining persistence** — understanding a sample lets defenders detect it and shut down further abuse.

This whole track is "**blue team**" work, and TryHackMe's SOC Level 1 path is the structured route into it.

**Q: What is the name of the role whose job is to identify attacks against an organisation?**
**A:** Security Analyst

---

## Offensive vs. Defensive at a Glance

| | **Offensive (Red)** | **Defensive (Blue)** |
|---|---|---|
| **Goal** | Find vulnerabilities before attackers do | Detect, stop, and respond to attacks |
| **Mindset** | Curious, analytical, creative | Analytical, problem-solving |
| **Example roles** | Penetration tester | Security Analyst, Incident Responder, Malware Analyst |
| **Core skills** | Web / network / cloud security, scripting | Log & SIEM monitoring, forensics, malware analysis |

The two sides aren't rivals — they're two halves of the same goal. Offensive findings feed directly into defensive improvements, and when the two teams work together deliberately it's called **purple teaming**. The best defenders understand how attacks work, and the best attackers understand how defenses work.

---

## Key Takeaways
- Cyber security splits broadly into **offensive** (finding weaknesses) and **defensive** (detecting and stopping attacks).
- A **penetration tester** does what a hacker does, but with **authorization and scope** — permission is the line between ethical and criminal.
- Offensive work rewards **broad** knowledge first (web, network, scripting, cloud); specialise later.
- Defensive roles — **analyst, incident responder, malware analyst** — all rest on knowing normal behaviour well enough to recognise the abnormal.
