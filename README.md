# Windows Security & SOC Investigation Lab

## Executive Summary

This repository documents my hands-on Windows security practice with a focus on the type of endpoint activity an entry-level **SOC Analyst / Cyber Defense Analyst** is expected to understand.

Instead of only studying Windows concepts, I generated safe activity in my own lab and then investigated the evidence left behind. The labs cover Windows users and groups, processes, services, Registry activity, scheduled tasks, Event Viewer, Windows Security logs, Sysmon process telemetry, authentication events, account creation, privilege changes, and PowerShell Script Block Logging.

The main goal of this project was to build the habit of connecting:

```text
User / System Activity
        ↓
Windows Process or Configuration Change
        ↓
Event / Telemetry Generated
        ↓
Investigation
        ↓
Security Meaning
```

This project shows my current practical foundation for investigating Windows endpoint activity, reading security events, building simple timelines, and deciding what evidence should be checked next.

---

## Recruiter Snapshot

**Environment:** Windows lab  
**Security Focus:** Endpoint monitoring, log analysis, authentication investigation, process analysis, privilege monitoring and persistence awareness  
**Tools:** Windows Event Viewer, Windows Security Event Log, Sysmon, PowerShell, Task Manager, Task Scheduler, Registry Editor  
**Target Role:** Entry-Level SOC Analyst / Cyber Defense Analyst

### Skills demonstrated

- Windows system and endpoint fundamentals
- Local user and group management
- Administrator-group / privilege-change investigation
- Process identification and parent-process analysis
- Windows service inspection
- Registry startup and persistence awareness
- Scheduled-task monitoring
- Windows Event Viewer navigation
- Windows Security Event Log analysis
- Sysmon process-creation analysis
- Successful and failed authentication investigation
- New-account creation monitoring
- PowerShell Script Block Logging
- Event correlation and basic investigation timelines
- Evidence-based SOC reasoning

---

# Hands-On Labs

## 1. Windows Architecture & System Basics

📄 [View Lab](win-architecture.md)

I reviewed Windows system information and used Task Manager to understand how users, processes and services are represented on an endpoint.

**SOC relevance:** Before investigating an alert, an analyst needs to understand where Windows exposes system, process, user and service information.

---

## 2. Users, Groups & Local Privileges

📄 [View Lab](users-groups.md)

I created a local test account, reviewed local users and groups, and added and removed the account from the local **Administrators** group using PowerShell.

**SOC relevance:** Unexpected accounts or administrator-group membership can indicate unauthorized access or privilege escalation.

---

## 3. Windows Services

📄 [View Lab](services.md)

I reviewed Windows services using PowerShell and the Services console, including the status of **Microsoft Defender** and the **Windows Event Log** service.

**SOC relevance:** Stopped security services, unexpected services or unusual service changes may require further investigation.

---

## 4. Process Investigation

📄 [View Lab](processes.md)

I started Notepad, identified the running process and PID, and then stopped it using PowerShell.

**SOC relevance:** Process details such as name, PID and execution context are basic evidence when investigating unknown or unwanted software.

---

## 5. Registry Basics & Persistence Awareness

📄 [View Lab](registry.md)

I inspected a Windows startup Registry location and created, viewed and removed a safe test Registry key using PowerShell.

I also reviewed how startup-related Registry locations can influence which programs run automatically.

**SOC relevance:** Registry changes can be useful evidence when investigating persistence, unwanted startup entries or system configuration changes.

---

## 6. Scheduled Task Monitoring

📄 [View Lab](scheduled-tasks.md)

I created a scheduled task that started Notepad and then reviewed its activity through Task Scheduler and Event Viewer.

**SOC relevance:** Scheduled tasks are used for legitimate administration, but they can also be abused to run programs automatically or maintain persistence.

---

# Windows Event & Telemetry Investigation

## 7. Event Viewer & Sysmon Process Investigation

📄 [View Lab](eventvwr-%26-sysmon.md)

I used **Event Viewer** to review Windows Security activity and **Sysmon** to investigate process creation.

One of the process investigations showed:

```text
powershell.exe
      ↓
notepad.exe
      ↓
Sysmon Event ID 1
```

I checked information such as:

- Process name
- PID
- Parent process
- User
- Command line
- Timestamp

**SOC relevance:** Parent-child process relationships and command-line information can help explain how a process was started and whether the execution chain is expected.

---

## 8. Authentication Investigation

📄 [View Lab](authentication-events.md)

I intentionally generated failed login attempts for the `soclab` test account and then logged in successfully.

I investigated:

| Event ID | Meaning |
|---:|---|
| `4625` | Failed logon |
| `4624` | Successful logon |

I compared the account name, timestamps and authentication results to reconstruct the sequence of activity.

### Investigation flow

```text
Failed login
     ↓
Event ID 4625
     ↓
Failed login
     ↓
Event ID 4625
     ↓
Successful login
     ↓
Event ID 4624
     ↓
Review surrounding activity
```

**SOC relevance:** Repeated failed logins can be caused by user mistakes, password guessing or brute-force activity. The event alone is not enough to call something malicious; the surrounding evidence matters.

This was an important lesson from the lab: **an alert needs investigation before a verdict is made.**

---

## 9. User Account Creation Monitoring

📄 [View Lab](account-creation.md)

I filtered Windows Security logs for:

```text
Event ID 4720
```

to review new local user-account creation activity.

The event allowed me to check details about the account that performed the action and the account that was created.

**SOC relevance:** Unexpected account creation can be an indicator of unauthorized access or an attempt to maintain access to a system.

---

## 10. Administrator Group / Privilege Change

📄 [View Lab](privilege-change.md)

I investigated:

```text
Event ID 4732
```

after the `soclab` account was added to the local **Administrators** group.

The event showed the group change and the account involved in performing it.

### Investigation idea

```text
Normal local account
        ↓
Added to Administrators
        ↓
Privilege level increases
        ↓
Security event generated
        ↓
SOC analyst validates whether the change was expected
```

**SOC relevance:** Unexpected administrator-group changes may indicate privilege escalation or unauthorized administrative access.

---

## 11. PowerShell Script Block Logging

📄 [View Lab](powershell-logging.md)

I enabled PowerShell Script Block Logging, executed PowerShell activity and reviewed:

```text
Event ID 4104
```

in Event Viewer.

This showed how Windows can record PowerShell script-block activity for later investigation.

**SOC relevance:** PowerShell is a legitimate administration tool, but it is also commonly examined during security investigations because scripts and commands can be used for both normal administration and malicious activity.

---

# Key Windows Events Practiced

| Event ID | Activity Investigated | Why It Matters |
|---:|---|---|
| `4624` | Successful logon | Helps confirm successful authentication and build a login timeline |
| `4625` | Failed logon | Useful when investigating password guessing, brute-force activity or user login problems |
| `4720` | User account creation | Helps identify unexpected or unauthorized accounts |
| `4732` | Member added to a local security group | Useful for investigating local privilege / administrator-group changes |
| `4104` | PowerShell Script Block Logging | Helps review PowerShell commands and scripts |
| `1` *(Sysmon)* | Process creation | Provides process, parent process, user and command-line evidence |

---

# Investigation Skills Demonstrated

## Authentication Analysis

I practiced moving from a login event to questions such as:

```text
Which account was involved?
        ↓
Did authentication fail or succeed?
        ↓
When did it happen?
        ↓
Were there repeated attempts?
        ↓
What happened after the successful login?
```

This helps avoid treating every failed-login alert as an attack without enough evidence.

---

## Account & Privilege Analysis

I generated account and group changes in the lab and then looked for the corresponding Windows security evidence.

```text
Account created
      ↓
Event ID 4720

User added to Administrators
      ↓
Event ID 4732
```

This helped me understand the difference between normal account administration and changes that may deserve investigation in a real environment.

---

## Process Analysis

Using Sysmon, I moved beyond simply seeing that a process was running.

I checked:

```text
Process
   ↓
PID
   ↓
Parent Process
   ↓
User
   ↓
Command Line
   ↓
Time
```

This is useful because understanding **how a process started** can be just as important as knowing that the process exists.

---

## Persistence Awareness

I practiced two Windows areas commonly checked when investigating persistence:

- Registry startup locations
- Scheduled tasks

The purpose of the labs was not to label these features as malicious. Both are normal Windows functionality. The security skill is being able to review them and decide whether the activity is expected.

---

# PowerShell Skills Practiced

PowerShell was used throughout the lab for basic Windows administration and investigation.

Examples of areas practiced include:

```powershell
Get-Process
Get-Service
Get-LocalUser
Get-LocalGroup
Get-LocalGroupMember
Start-Process
Stop-Process
New-LocalUser
Add-LocalGroupMember
Remove-LocalGroupMember
```

I also used PowerShell to create and inspect safe Registry test data and to generate activity that could later be reviewed in Windows logs.

---

# How I Approach a Windows SOC Investigation

The main investigation habit I developed during this phase is:

```text
Alert / Suspicious Activity
        ↓
Identify the affected user or system
        ↓
Check Windows Security Events
        ↓
Check processes and parent processes
        ↓
Review PowerShell activity when relevant
        ↓
Check users / groups / privileges
        ↓
Check scheduled tasks or Registry if persistence is suspected
        ↓
Build a timeline
        ↓
Decide whether more evidence is required
```

The goal is not to immediately call an event malicious.

The goal is to **collect enough evidence to explain what happened**.

---

# What This Project Proves

After completing these labs, I can perform basic Windows endpoint investigation tasks such as:

- Navigate Windows Event Viewer
- Review Windows Security logs
- Identify successful and failed authentication activity
- Investigate local account creation
- Review local Administrator-group changes
- Inspect processes and PIDs
- Use Sysmon process-creation telemetry
- Identify parent-child process relationships
- Review PowerShell Script Block Logging
- Inspect Windows services
- Review scheduled tasks
- Check basic Registry startup locations
- Use PowerShell for Windows security investigation and administration
- Document findings in simple investigation notes

These are foundational skills I am continuing to develop for an entry-level **SOC Analyst / Cyber Defense Analyst** role.

---

# Related Linux Security Project

I am also building Linux investigation skills on **Red Hat Enterprise Linux (RHEL)**.

The Linux project covers users and groups, file permissions, processes, services, SSH authentication logs, open ports, package management, cron jobs and other Linux investigation fundamentals.

**Linux Repository:** [github.com/utkarshs3000/linux](https://github.com/utkarshs3000/linux)

Together, the Windows and RHEL projects show my effort to build practical endpoint-investigation skills across both operating systems.

---

# Repository

**Windows Security Lab:** [github.com/utkarshs3000/windows](https://github.com/utkarshs3000/windows)

---

## Current Career Focus

I am building toward an entry-level **SOC / Cyber Defense Analyst** role and using these projects to turn security concepts into hands-on investigation experience.

My focus is on becoming comfortable with:

**Windows & Linux → Logs → Processes → Authentication → Network Activity → Alert Investigation → Evidence → Security Decision**

---

> **Lab Safety:** All activity documented in this repository was generated in my own learning environment for defensive cybersecurity practice.
