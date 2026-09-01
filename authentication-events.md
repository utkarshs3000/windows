# Windows Authentication Investigation

## Scenario

I intentionally entered an incorrect password for the soclab test account and then logged in successfully.

## Activity Generated

- Failed login attempts
- Successful login

## Log Source

Windows Security Event Log

## Events Investigated

- 4625 - Failed logon
- 4624 - Successful logon

## What I Found

Failed logins for user `soclab` followed with a successful login.

## Investigation

I compared the account name, timestamps and login results to understand the sequence of authentication activity.

## Result

The logs showed failed authentication attempts followed by a successful login for the test account.

## What I Learned

Windows Security logs can be used to identify successful and failed login attempts and build a timeline of authentication activity.

## SOC Relevance

Repeated failed logins can be normal user mistakes, but they can also indicate password guessing or brute-force activity. More evidence is needed before deciding that the activity is malicious.

<img width="1920" height="1080" alt="Screenshot 2026-09-01 102613" src="https://github.com/user-attachments/assets/411c52a9-2f78-4ce1-ad37-d5a304094da4" />
<img width="1920" height="1080" alt="Screenshot 2026-09-01 102711" src="https://github.com/user-attachments/assets/af616f39-1234-421e-8082-fd10e72a6660" />
