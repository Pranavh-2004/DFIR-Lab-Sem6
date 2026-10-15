---
name: pesu-tracelab-ctf
description: Use this skill when helping solve challenges for the PESU TraceLabs Cloud Forensic Labs CTF. Triggers on any mention of this CTF, its questions (Q1–Q6), forensic disk image investigation, flag decryption, or the CFL{} flag format. Provides full context on the competition structure, per-question evidence locations, the 4-layer answer→seed→key→flag system, tooling, and anti-cheat constraints so Claude can give precise, terminal-level help without re-explaining the setup each time.
---

## Competition Overview

**Event**: PESU TraceLabs — Cloud Forensic Labs CTF  
**Date**: Saturday, 11 April 2026 — PES University, Ring Road Campus  
**Format**: 6 challenges × 200 points = 1200 points total (+ optional Boss Question: 1200 pts)  
**Duration**: 1:00 PM – 4:00 PM (3 hours); rules page says 1:30 PM – 4:30 PM — treat 4:00 PM as safe cutoff  
**Platform**: Browser-based VNC desktop (remote Ubuntu) — no local tool downloads allowed

---

## Evidence Setup

Each student gets a **unique** 80 MB ext4 disk image. No two students share the same answers, seeds, or flags. Sharing is useless and actively detected.

```bash
# The disk image lives here:
/home/analyst/ctf/evidence/evidence.dd

# Mount it (read-only):
sudo mount -o loop,ro /home/analyst/ctf/evidence/evidence.dd /mnt/evidence

# Browse:
cd /mnt/evidence && ls -la
```

---

## The 4-Layer System (applies to ALL 6 questions)

Every challenge follows this exact pipeline. Master it once.

### Step 1 — Find the ANSWER
Locate the specific forensic artefact described in the question. The challenge statement tells you exactly what to look for.

### Step 2 — Find the SEED
The seed is **always** a **6-character hex string**, hidden in the **same file** as the answer. If you extracted something shorter or longer, you grabbed the wrong value. Seed locations per question are listed in the Quick Reference below.

### Step 3 — Build the KEY
```bash
echo -n "ANSWERSEED" | md5sum | cut -c1-6
# Concatenate answer + seed with NO space, then MD5, take first 6 chars
```

### Step 4 — Validate the KEY
```bash
grep "AUTH-PARTIAL" /mnt/evidence/event_logs/prefetch_data/ntfs_journal/syslog_Q<N>.log
# First 3 characters of your computed key must match AUTH-PARTIAL value
# WARNING: 200+ decoy AUTH-PARTIAL entries exist on disk — use ONLY the correct syslog for the question
```

### Step 5 — Find and Decrypt the FLAG
```bash
# Flag files have NO .enc extension — they look like normal Windows artefacts:
# .log  .tmp  .cfg  .sys  .bak  .old
# There are 6 decoy encrypted files scattered across the disk

# Decrypt attempt:
cat FILENAME | openssl enc -d -aes-256-cbc -a -pbkdf2 -pass pass:"YOUR_KEY"

# Correct file + correct key → outputs CFL{...}
# Wrong file or wrong key → gibberish or OpenSSL error
```

### Step 6 — Submit
Paste the full `CFL{...}` string into the platform submission box.

---

## Quick Reference Table

| Q | Evidence Artefact | Seed Location | Flag Directory |
|---|-------------------|---------------|----------------|
| Q1 | EXIF metadata of a deleted `.jpg` (recover via Autopsy) | `SerialNumber` field — first 6 chars | `system_recovery/` |
| Q2 | `chrome_history.db` browser database | `cookies` table → `session_id` column — first 6 chars | `service_configs/` |
| Q3 | `SYSTEM_hive_export.reg` registry export | `ProductId` value — 3rd segment (lowercase) | `windows_update/` |
| Q4 | `wireshark_conversations.csv` network capture | Header comment: `Capture File Hash` — first 6 chars | `driver_packages/` |
| Q5 | `memory_analysis_report.txt` Volatility output | Section 5 → `CRED-001` offset — 6 hex chars | `security_audit/` |
| Q6 | `recycle_store/` folder (Autopsy) | `.metadata` file → `Case Reference` — 6 chars | `task_scheduler/` |

---

## Tools on the VNC Desktop

| Tool | Purpose | Used In |
|------|---------|---------|
| `Autopsy` | GUI forensic disk analyser — browse files, recover deleted items | Q1, Q6 |
| `exiftool` | Extract EXIF metadata from images (camera model, serial numbers, GPS) | Q1 |
| `sqlite3` | Query SQLite databases from terminal | Q2 |
| `grep` / `cat` / `less` | Search and read text files | Q3, Q4, Q5 |
| `openssl` | Decrypt AES-256 encrypted flag files | All |
| `strings` | Extract readable text from binary files | General |
| `ls -la` | List all files including hidden ones (names starting with `.`) | Q6 |

---

## Scoring & Hints

| Points Range | Marks Awarded |
|---|---|
| > 800 | 50 marks |
| 700 – 800 | 45 marks |
| 600 – 700 | 40 marks |
| 500 – 600 | 35 marks |
| < 500 | 30 marks |

**Hints**: Hint 1 = −25 pts. Hint 2 = −50 pts. Both hints = −75 pts max (you still get 75% of base).  
Hint 1 gives you half the seed — worth using if you've been stuck 20+ minutes on seed extraction.

**Rate limiting**:
- First 5 wrong attempts → 3-second cooldown
- 5–10 wrong → 10-second cooldown
- 10+ wrong → 30-second cooldown + 5-minute lockout per challenge

Do not brute-force. Validate key against AUTH-PARTIAL before decrypting.

---

## Anti-Cheat — What Gets You Flagged

- Submitting another slot's flag → immediate cross-slot detection
- Multiple IP addresses submitting simultaneously → security alert
- 10+ fullscreen violations → automatic flagging
- Browser DevTools / view-source → restricted in exam mode

---

## How Claude Should Help

When a user pastes a question or terminal output:

1. **Identify the question** (Q1–Q6) from context or explicit label
2. **Point to the exact artefact and tool** needed based on the Quick Reference
3. **Give precise terminal commands** for that specific step — don't re-explain the whole pipeline
4. **Debug by asking for actual output** — seed errors, key mismatches, and wrong syslog are the three most common failure points
5. **Never pre-compute flags** — every slot is unique; work from what the user actually extracted

---

## Standard Session Prompt (user can paste this each time)

```
[CTF Session — PESU TraceLabs Cloud Forensic Labs]

Question: Q<N>
Step I'm on: [ANSWER / SEED / KEY / VALIDATE / DECRYPT]
What I did:
  <paste command>
Output:
  <paste terminal output>
Problem:
  <what's wrong or what I'm unsure about>
```
