# 🕸️ Hunt for Lateral Movement (Pass-the-Hash) - CTF Challenge

## 📋 Overview

**Hunt for Lateral Movement (Pass-the-Hash)** is an interactive, browser-based Capture The Flag (CTF) challenge designed for cybersecurity training. This challenge focuses on detecting Pass-the-Hash (PtH) attacks where attackers use stolen NTLM hashes to authenticate across multiple systems without knowing the plaintext password. Participants analyze Windows Event 4624 logs and Zeek NTLM logs to identify lateral movement patterns.

## 🎯 Learning Objectives

By completing this CTF, participants will learn:

- **Threat Hypothesis Development**: Formulate detection hypotheses for Pass-the-Hash attacks
- **Log Analysis**: Analyze Windows Event 4624 (Logon Type 3) for lateral movement patterns
- **Correlation Analysis**: Cross-reference Windows logs with Zeek NTLM logs
- **Account Analysis**: Distinguish between service accounts and compromised user accounts
- **Incident Response**: Execute proper containment procedures for compromised accounts

## 🛠️ Challenge Tasks (5 Total)

| Task | Description | Skill Focus |
|------|-------------|-------------|
| **Task 1** | Select correct hunting hypothesis (same account, multiple machines) | Threat Hunting |
| **Task 2** | Identify suspicious account showing lateral movement (svc_backup) | Log Analysis |
| **Task 3** | Correlate with Zeek NTLM log to find source workstation IP | Correlation |
| **Task 4** | Determine if account is legitimate service account | Account Analysis |
| **Task 5** | Propose containment action (disable account) | Incident Response |

## 🚀 Quick Start

### Prerequisites
- A modern web browser (Chrome, Firefox, Edge, Safari)
- No server required - runs entirely in the browser
- No installation needed

### Access the Challenge
1. Open the HTML file directly in your browser
2. Enter your name
3. Use the password: `45_2026`
4. Complete all 5 tasks to capture the flag

### Hosting on GitHub Pages
1. Fork or clone this repository
2. Go to repository Settings > Pages
3. Select the branch (usually `main`) and save
4. Access via `https://your-username.github.io/repository-name`

## 🎮 How to Play

### Login
```
Password: 45_2026
Name: Enter any name (progress is saved locally)
```

### Game Features

- **Animated Network Map**: Visual representation of lateral movement with pulsing compromised nodes
- **Interactive Query Builder**: Drag-and-drop SPL/SQL parts to build detection queries
- **Event Log Display**: Simulated Windows Event 4624 logs with timestamps and details
- **Zeek NTLM Log Integration**: Cross-referenced network authentication logs
- **Service Account Whitelist**: Simulated whitelist for legitimacy checking
- **Answer Validation**: Immediate feedback on submitted answers
- **Progress Tracking**: Local storage saves your progress across sessions

### Completing Tasks
1. Read each task description carefully
2. Analyze the network map and event logs
3. Use the query builder to construct detection logic
4. Type your answer in the input field
5. Click "Submit" to validate
6. Complete all 5 tasks to reveal the flag

## 🏆 Flag

```
FLAG{PASS_THE_HASH_HUNT}
```

The flag is revealed only after completing all 5 tasks successfully.

## 📊 Challenge Details

### Network Map Visualization

```
🌐 Lateral Movement Network Map
├─ 🟠 10.0.1.50 (Attacker Source)
│  ├─ 🔴 SRV-DC01 (Compromised)
│  ├─ 🔴 SRV-FILE01 (Compromised)
│  ├─ 🔴 SRV-SQL01 (Compromised)
│  ├─ 🔴 SRV-WEB01 (Compromised)
│  └─ 🔴 SRV-EXCH01 (Compromised)
```

### Windows Event 4624 Logs (Logon Type 3 - Network)

```
14:23:01 | svc_backup | 10.0.1.50 → SRV-DC01 | Type:3 ⚠️
14:23:15 | svc_backup | 10.0.1.50 → SRV-FILE01 | Type:3 ⚠️
14:23:28 | svc_backup | 10.0.1.50 → SRV-SQL01 | Type:3 ⚠️
14:23:42 | svc_backup | 10.0.1.50 → SRV-WEB01 | Type:3 ⚠️
14:23:55 | svc_backup | 10.0.1.50 → SRV-EXCH01 | Type:3 ⚠️
14:30:00 | jdoe | 10.0.1.100 → SRV-FILE01 | Type:3 (Normal)
14:31:00 | asmith | 10.0.1.101 → SRV-WEB01 | Type:3 (Normal)
```

### Zeek NTLM Logs

```
ntlm: 10.0.1.50 -> SRV-DC01 | user:svc_backup | status:success
ntlm: 10.0.1.50 -> SRV-FILE01 | user:svc_backup | status:success
ntlm: 10.0.1.50 -> SRV-SQL01 | user:svc_backup | status:success
ntlm: 10.0.1.50 -> SRV-WEB01 | user:svc_backup | status:success
ntlm: 10.0.1.50 -> SRV-EXCH01 | user:svc_backup | status:success
```

### Service Account Whitelist

```
✅ svc_sqlagent
✅ svc_webpool
✅ svc_exchange
✅ svc_scan
❌ svc_backup (NOT IN WHITELIST)
```

## 🔍 Investigation Walkthrough

### Task 1: Hunting Hypothesis
The correct hypothesis is **"same account multiple machines"** - Pass-the-Hash is detected when a single account authenticates to multiple systems in a short time window. Key indicators include:
- Same username across multiple destinations
- Logon Type 3 (Network) indicating remote authentication
- Short time intervals between logons
- Source workstation remains consistent

### Task 2: Identify Suspicious Logon
**svc_backup** shows clear lateral movement because:
- 5 different servers accessed within 54 seconds
- All from the same source IP (10.0.1.50)
- Pattern is consistent with automated credential use
- Normal users don't access 5 servers in under a minute

### Task 3: Correlate with Zeek NTLM
The source workstation IP is **10.0.1.50** - identified by:
- Consistent source in all NTLM authentication attempts
- All successful authentications originate from this IP
- Zeek logs confirm NTLM protocol usage
- Correlation with Windows Event 4624 logs

### Task 4: Service Account Check
**svc_backup is NOT legitimate** because:
- Not present in the service account whitelist
- Legitimate service accounts: svc_sqlagent, svc_webpool, svc_exchange, svc_scan
- Service accounts typically access limited, specific systems
- Backup accounts shouldn't need access to web servers and SQL servers simultaneously

### Task 5: Containment Action
The first containment step is to **"disable account"** - immediately disable the compromised account to:
- Prevent further lateral movement
- Stop ongoing authentication attempts
- Break the attack chain
- Allow time for investigation

## 🎨 Visual Features

- **Animated Network Map**: Pulsing red nodes showing compromised systems with orange attacker source
- **Connection Lines**: Animated dashed lines showing lateral movement paths
- **Drag-and-Drop Query Builder**: Interactive SPL/SQL query construction interface
- **Color-coded Log Entries**: Red borders for suspicious events, orange for Zeek correlations
- **Progress Indicators**: Visual completion status for each task
- **Glowing Flag Animation**: Celebratory golden flag reveal
- **Toast Notifications**: Non-intrusive success/error messages
- **Dark Theme**: Blue-accented UI for network security theme

## 💾 Data Storage

- **Progress**: Saved in browser's `localStorage`
- **Persistence**: Progress survives page refreshes
- **Privacy**: All data stays on the user's device
- **Reset**: Clear browser data to reset progress

## 🛡️ Pass-the-Hash Detection Indicators

### High-Fidelity Indicators:
- Event 4624 with Logon Type 3 from unexpected source
- Same account authenticating to 5+ systems in under 2 minutes
- NTLM authentication instead of Kerberos
- Source workstation IP not associated with the user account
- Authentication outside normal business hours

### Medium-Fidelity Indicators:
- Service account accessing non-service systems
- Multiple failed logons followed by success
- Unusual combination of destination servers
- Authentication from non-domain-joined systems

### Low-Fidelity Indicators:
- Single network logon from new source
- Periodic logons that could be scheduled tasks
- Logons from known administrative workstations

## 📁 File Structure

```
hunt-lateral-movement-pth/
│
├── index.html          # Main CTF challenge file
├── README.md           # This documentation
└── (no other files required)
```

## 🔧 Technical Implementation

- **Pure Frontend**: HTML5, CSS3, JavaScript (Vanilla)
- **No Dependencies**: Zero external libraries
- **Responsive Design**: Works on desktop and mobile
- **Animations**: CSS keyframe animations for network map
- **Storage**: Browser localStorage API
- **Gamification**: Progress tracking, badge system, visual rewards
- **Interactive Elements**: Drag-and-drop query builder

## 📊 Attack Pattern Reference

### Pass-the-Hash Attack Flow
```
1. Attacker compromises initial host
2. Extracts NTLM hashes from LSASS/SAM
3. Uses hash for authentication (no password needed)
4. Authenticates to multiple systems (lateral movement)
5. Repeats process on each compromised host
6. Eventually reaches high-value targets
```

### Detection Query Logic
```
EventID=4624
AND LogonType=3
AND same_user
AND multiple_targets
AND time_window < 2 minutes
| stats count by user
| where count > 3
```

## 🎓 Educational Use Cases

- **Cybersecurity Training Programs**
- **SOC Analyst Onboarding**
- **Threat Hunting Workshops**
- **Blue Team Exercises**
- **Incident Response Training**
- **Academic Courses** (Network Security, Attack Detection)
- **Self-paced Learning**
- **Purple Team Exercises**

## 🔄 Version History

- **v1.0** - Initial release
  - 5 tasks with validation
  - Animated network map with pulsing compromised nodes
  - Drag-and-drop query builder
  - Simulated Event 4624 and Zeek NTLM logs
  - Local storage progress tracking
  - Student login system

## 👥 Target Audience

- Security Operations Center (SOC) Analysts
- Incident Response Team Members
- Threat Hunters
- Network Security Engineers
- Cybersecurity Students
- IT Security Professionals
- Blue Team Practitioners
- Active Directory Security Specialists

---

**Happy Pass-the-Hash Hunting! 🕸️**
