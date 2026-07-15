# CS4CS Practical Labs — Facilitator Guide

Two hands-on sessions, built to run in lockstep with a room of about 60 high schoolers on their own laptops, with no admin rights and on filtered Wi-Fi. Each session is a slide deck you put on the screen plus a self-contained browser activity the students follow step by step.

## What is in this pack

- **Operation_Ransomware_Lab.pptx** — July 15 (OS Security), the deck you present.
- **operation_ransomware.html** — the July 15 activity, a simulated Linux terminal escape room.
- **Internet_Detective_Lab.pptx** — July 20 (Networking), the deck you present.
- **internet_detective.html** — the July 20 activity, a simulated packet analyzer.
- This guide.

Both activities run entirely in the browser. No install, no login, no account, no SSH, no ping. Once a student has loaded the page, it even keeps working if the Wi-Fi drops.

## The two sessions at a glance

| | July 15 — Operation Ransomware | July 20 — Internet Detective |
|---|---|---|
| Follows the lecture on | OS security (Day 3) | Networking (Day 6) |
| Format | Terminal escape room | Packet forensics case |
| Students practice | CLI, file permissions, standard vs root, privilege escalation | DNS, HTTP vs HTTPS, TCP vs UDP, TCP/IP layers, subnets |
| Tasks | 8 steps to a final flag | 8 tasks to solve the case |
| Win condition | Submit the recovered flag | Solve the case correctly |

---

## Before class

### 1. Host the activity (do this once per session)

Pick whichever you are comfortable with. Option A is best for reuse.

**Option A — GitHub Pages (recommended).** Put the one HTML file in a repo, turn on Pages, and you get a permanent `https://yourname.github.io/...` link. It is on standard HTTPS, so the NYU filter will not block it, and you reuse the same link every cohort.

**Option B — Netlify Drop (fastest).** Go to app.netlify.com/drop and drag the HTML file in. You get a live HTTPS link in about ten seconds. Make a free account if you want it to stay up.

**Option C — Share the file (most filter-proof).** Because each file is fully self-contained, students can open it locally with no server at all. Put it in a shared Drive folder or on Brightspace, they download it once and double-click. After the download it does not even need the network.

My suggestion: host on GitHub Pages as the primary link, and also drop the raw HTML in a Drive folder as a backup. If anything flakes mid-session, you tell the room "download this file and open it" and you are back in business.

**Make it easy to reach.** Typing a long URL across 60 laptops is slow. Generate a QR code (any free QR site) or a short link (tinyurl) for your hosted page, and put that on the screen.

### 2. Drop your link into the deck

Each deck has one placeholder to replace. On **slide 4 (Setup)** of each deck there is a box that reads `[ paste your lab link here ]`. Replace it with your real link or a QR image before class. That is the only edit the decks need.

### 3. Pre-class checklist

- [ ] Activity hosted, link works on a phone and a laptop.
- [ ] QR code or short link ready.
- [ ] Slide 4 placeholder replaced in both decks (only the one you are running that day).
- [ ] Backup HTML file in a shared Drive folder.
- [ ] Quick self-run of the activity end to end (10 minutes) so the flow is fresh.

---

## Running a session (works for both days)

The decks are built for **lockstep**. You drive, the room follows.

- **Pairs are fine and encouraged.** One laptop per pair means about 30 screens, which is far easier to manage and keeps quieter students involved.
- **One step per slide.** Present the slide, let everyone do that one action, then advance. Ask the room not to race ahead.
- **Each activity has a built-in safety net.** The OS terminal has a Hint button per objective. The packet analyzer has quick filter buttons and a task checklist. Nobody gets permanently stuck.
- **Floaters watch the room, not the front.** You present, others walk the aisles for the raised hands. The slowest pair sets your pace, not the fastest.
- **Progress is visible.** Both activities show a progress bar or checklist, so a glance tells a floater who is behind.

Suggested clock for a 2-hour block (leave a 10-minute buffer):

| Phase | OS day | Net day |
|---|---|---|
| Welcome and briefing | 10 min | 10 min |
| Open the lab and tour the tool | 12 min | 14 min |
| Connect to the lecture | 5 min | 5 min |
| The 8 tasks together | 55 min | 58 min |
| Win, recap, what just happened | 18 min | 16 min |
| Debrief discussion | 10 min | 8 min |
| Go further and wrap | 5 min | 5 min |

---

## Session 1 — July 15 — Operation Ransomware (OS)

The story: ransomware locked a Linux server, the students are the incident response team, and all they have is a low-privilege shell. They work through eight steps to recover the decryption key (the flag) and unlock the server.

### Step-by-step answer key

The terminal logs them in as the user `intern` (a standard user, not admin). Each step matches a deck slide.

1. `whoami` — confirms they are `intern`, a limited account.
2. `pwd` — they are in `/home/intern`.
3. `ls` — shows `briefing.txt`, `logs`, `recover.sh`.
4. `cat briefing.txt` — tells them to look for hidden files.
5. `ls -la` — reveals the hidden `.recovery` folder and the permissions column.
6. `cd .recovery` then `cat note.txt` — the note says to make `recover.sh` executable.
7. `cd ..`, then `ls -l recover.sh` (no x), then `chmod +x recover.sh`, then `./recover.sh` — the script points them at the admin-only key.
8. `sudo cat /root/decryption_key.txt` — plain `cat` is denied, `sudo` works, and the key appears.

**The flag is `CS4CS{r00t_4cc3ss_rec0v3r3d}`.** Students copy it into the flag box on the right of the lab and press Submit, which pops the "Server recovered" screen.

Useful built-in commands if a student explores: `help`, `clear`, `ls -a`, `ls -l`, `cd ..`. The Hint button on each objective reveals the exact next command.

### The teaching beats

- Standard users are limited on purpose (steps 1 and 8).
- The `rwx` permission letters control read, write, and run (steps 5 to 7). Slide 14 is a decoder for `-rwxr-xr-x` you can return to any time.
- Going from `intern` to `root` is privilege escalation, the move almost every real attack is trying to make (slide 15).
- The attacker won because `intern` was on the sudo list and the admin reused a password. The fix is least privilege (slide 17).

### Troubleshooting

- **"Permission denied" on step 8.** That is expected for plain `cat`. They need `sudo cat`. This is the lesson, not a bug.
- **A pair is far behind.** Point them at the Hint button, which walks straight to the next command.
- **Typo loops.** Commands are case sensitive and need exact spelling. `ls -la`, not `ls-la`.
- **Lost their place.** `clear` cleans the screen, `help` lists every command, the objective list on the right shows what is left.

---

## Session 2 — July 20 — Internet Detective (Networking)

The case: a laptop on the school Wi-Fi may have leaked a password. The students open a captured trace and follow the evidence to the one packet that proves it. The activity maps directly onto the Day 6 lecture, especially the "MAC to IP to port equals device to host to app" idea.

### The cast (the IPs in the capture)

- `192.168.1.105` — the victim laptop (on the LAN).
- `192.168.1.1` — the gateway and DNS (on the LAN).
- `192.168.1.20` — the office printer (on the LAN).
- `104.18.22.10` — the real school portal, reached over HTTPS (out on the internet).
- `185.220.101.42` — the suspicious server, `free-prize-login.tk`, reached over plain HTTP (out on the internet).
- `8.8.8.8` — a public DNS server (out on the internet).

The LAN is `192.168.1.0/24`.

### Task-by-task answer key

1. **Find the DNS lookups.** Filter `dns`. The bait name `free-prize-login.tk` resolves to `185.220.101.42`, an outside address.
2. **Find the cleartext HTTP.** Filter `http`. The smoking gun is a `POST /submit` to `free-prize-login.tk` whose form data reads `username=j.smith&password=Wildcats2026!` in plain text.
3. **Find the encrypted HTTPS/TLS.** Filter `tls`. Traffic to the real portal `104.18.22.10` is scrambled and unreadable. Same internet, but the bait site skipped this.
4. **Read a packet's layers.** Click any packet, read it outside-in: frame (MAC), then IP, then port. Device to host to app.
5. **Inspect a TCP packet.** Filter `tcp`. Look for the three-way handshake (SYN, SYN-ACK, ACK).
6. **Inspect a UDP packet.** Filter `udp`. The DNS lookup is UDP. Contrast it with the TCP handshake (no setup, no acknowledgements).
7. **Subnet round.** Sort six addresses using `192.168.1.0/24` as the line:
   - Local (LAN): `192.168.1.105`, `192.168.1.1`, `192.168.1.20`.
   - Internet (WAN): `104.18.22.10`, `185.220.101.42`, `8.8.8.8`.
8. **Solve the case.** The correct call: a phishing site reached over plain HTTP, with the credentials sent in cleartext. HTTPS would have hidden them.

### The reveal in one line

DNS pointed a bait domain to an outside server, the laptop connected over plain HTTP on port 80, and the POST carried the password in clear text. The only thing missing was the S in HTTPS.

### Troubleshooting

- **"I cannot read the TLS packet."** Correct. That is the point. Encrypted traffic is meant to be unreadable.
- **Filter shows nothing.** Filters are lowercase keywords (`dns`, `http`, `tls`, `tcp`, `udp`) or an IP. The quick buttons do the same thing with no typing.
- **Subnet round confusion.** Remind them a `/24` fixes the first three numbers. If the address starts with `192.168.1`, it is local. Otherwise it is out on the internet.
- **A pair finishes early.** Have them write the four-step timeline of the attack in their own words, or open a second packet and name every layer out loud.

---

## If the internet or a laptop fails

This is the scenario the whole design protects against.

- **Wi-Fi drops after loading.** Both activities keep running. They live entirely in the browser tab.
- **Wi-Fi will not load the page at all.** Share the HTML file from your backup Drive folder. Students download it once and double-click, no server needed.
- **A laptop is fully locked down or broken.** Pair that student with a neighbor. Pairs are already encouraged.
- **The projector dies.** The activity has its own on-screen task list and hints, so students can keep moving while you fix the screen.

---

## Go further at home (the stretch slide)

For the students who want the real tools. These are on the final "go further" slide of each deck.

OS day:
- **picoCTF** (picoctf.org) — free beginner capture-the-flag challenges in the browser.
- **OverTheWire: Bandit** (overthewire.org/wargames/bandit) — a real Linux server you SSH into, level by level. Note this needs SSH, which the school Wi-Fi blocks, so it is a home activity.
- **A real shell** — Terminal on Mac or Linux, or WSL on Windows, to practice `ls`, `cd`, `chmod` for real.

Networking day:
- **Wireshark** (wireshark.org) — the real version of today's tool. Free, with sample captures to open.
- **See your own path** — look up your public IP, or run `tracert` (Windows) or `traceroute` (Mac/Linux) at home to watch your data hop across the internet. These often fail on school Wi-Fi but work at home.
- **picoCTF** — has a networking track that follows on nicely from today.

---

## Answer key quick reference

Keep this where students cannot see it.

- **OS flag:** `CS4CS{r00t_4cc3ss_rec0v3r3d}`
- **OS final move:** `sudo cat /root/decryption_key.txt`
- **Net smoking gun:** `POST /submit` to `free-prize-login.tk`, body `username=j.smith&password=Wildcats2026!`
- **Net subnet round, local:** 192.168.1.105, 192.168.1.1, 192.168.1.20
- **Net subnet round, internet:** 104.18.22.10, 185.220.101.42, 8.8.8.8
- **Net case answer:** phishing over plain HTTP, credentials sent in cleartext, HTTPS would have prevented it.
