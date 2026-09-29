# Lab 01 — Investigating suspicious login activity

**Status:** Ready to start; personal report not yet completed  
**Level:** Beginner  
**Tools:** Windows PowerShell 5.1 or PowerShell 7  
**Data:** 20 fictional authentication events supplied for this exercise

## Objective

Practice reading login logs, separating facts from assumptions, and writing an evidence-based investigation summary.

You are reviewing a short sample of fictional VPN events. Identify activity worth investigating, reconstruct its timeline, and explain what extra evidence you would request.

This is an offline exercise. It does not connect to the listed IP addresses, attempt logins, or change your computer's security settings.

## 1. Get the files

From the repository's main page, choose **Code → Download ZIP**, then extract it. Open this lab folder and launch PowerShell there. Alternatively, open PowerShell and use `Set-Location` with the full path to this folder.

Confirm that the file is present:

```powershell
Get-Item .\auth-events.csv
```

If it cannot be found, check that you extracted the ZIP and that PowerShell is in the folder containing the CSV.

## 2. Inspect the dataset

```powershell
$events = Import-Csv .\auth-events.csv
$events.Count
$events | Select-Object -First 5 | Format-Table -AutoSize
$events | Group-Object result | Select-Object Name, Count
```

Each row is one event. `timestamp_utc` is its UTC time, `username` is the account presented, and `source_ip` is the recorded origin. A failed login is not automatically an attack.

Write down the total event count and the counts of successful and failed logins.

## 3. Find the largest groups of failures

```powershell
$failures = $events | Where-Object result -eq 'failure'
$failures | Group-Object source_ip | Sort-Object Count -Descending |
    Select-Object Name, Count
$failures | Group-Object username | Sort-Object Count -Descending |
    Select-Object Name, Count
```

Compare failures concentrated on one account with failures spread across several accounts. Record the source addresses that deserve a closer look.

## 4. Reconstruct the timelines

```powershell
$events | Where-Object source_ip -eq '198.51.100.23' |
    Sort-Object timestamp_utc |
    Format-Table event_id, timestamp_utc, username, result, reason -AutoSize

$events | Where-Object source_ip -eq '203.0.113.44' |
    Sort-Object timestamp_utc |
    Format-Table event_id, timestamp_utc, username, result, reason -AutoSize
```

The timestamps in this exercise all use the same fixed UTC format, so sorting these strings orders them chronologically.

Answer these questions:
1. Which account has repeated failures followed by success from the same recorded source?
2. How much time passes between its first failure and the success?
3. Which source tries several different usernames? How many?
4. Does this sample show a successful login from that source?
5. What benign explanations remain possible?
6. What do the logs fail to tell you about who was behind the activity?

## 5. Compare a less suspicious sequence

```powershell
$events | Where-Object source_ip -eq '192.0.2.10' |
    Sort-Object timestamp_utc |
    Format-Table event_id, timestamp_utc, username, result, reason -AutoSize
```

Consider why one failure followed by success might be an ordinary typing mistake. Explain why that remains an interpretation rather than a proven fact.

## 6. Create your report

Copy the [lab report template](../../templates/lab-report.md) into this folder as `REPORT.md`. Include:

- Your event totals
- A timeline with event IDs and UTC times
- The suspicious patterns and alternative explanations
- Screenshots or text output from your run
- The extra evidence you would request, such as MFA outcomes, device information, user confirmation, or post-login activity
- A short reflection on what you learned

Check your reasoning against the [reference answers](REFERENCE-ANSWERS.md) after writing your own findings. Then link your report from the main portfolio page. Follow the [evidence checklist](../../EVIDENCE.md) before marking the lab complete.

## Documentation

- [Microsoft: Import-Csv](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/import-csv)
- [Microsoft: Group-Object](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/group-object)

## Dataset provenance

The bundled data is AI-generated synthetic training material, not a real incident or a capture from Jason's systems. All accounts are fictional. The IP addresses are documentation examples. Reference answers describe the dataset; they do not establish that the learner completed the investigation.
