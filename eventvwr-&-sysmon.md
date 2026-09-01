## [Windows Lab]

## // Security Event Logs

## What I practiced

 I opened Event Viewer and checked Windows Security logs for login and account activity.

## What I learned

 Security logs record events such as successful logins, special logons and account-related activity using event IDs.

## SOC Relevance

 Reviewing security event logs helps me find login activity, privilege use and other events that may need investigation.

<img width="1920" height="1080" alt="Screenshot 2026-09-01 001220" src="https://github.com/user-attachments/assets/6cfa6b5b-44f8-42f0-8459-2951f4daad99" />


# Windows Process Investigation

## Process

notepad.exe

## Evidence Source

Sysmon Operational Log

## Event

Event ID 1 - Process Creation

## What I Checked

Process name: notepad
PID: 3912
Parent process: powershell
User: `utkarsh`
Command line: "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe"
Time: 1:04:32 AM

## Result

Process notepad created by parent process powershell.

## What I Learned

Sysmon provides more detail about process execution than simply viewing the process in Task Manager. The event can show which process started another process, which user was involved and what command was executed.

## SOC Relevance

Process creation data can help determine how suspicious software or commands were started on a Windows endpoint.

<img width="1920" height="1080" alt="Screenshot 2026-09-01 011014" src="https://github.com/user-attachments/assets/5f6df5d9-1c61-4288-983e-a136751afcf6" />

<img width="1920" height="1080" alt="Screenshot 2026-09-01 011045" src="https://github.com/user-attachments/assets/3ffd67cc-7c69-4aeb-a77b-c667027d9195" />

