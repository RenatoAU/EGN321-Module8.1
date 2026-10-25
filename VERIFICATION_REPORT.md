# Technical Verification Report

Repository: https://github.com/RenatoAU/EGN321-Module8.1

## Scope
Actual automated execution in the assistant Linux environment on October 25, 2026. This is a technical system check with synthetic temperatures at a 0.5-second delivery interval; it is not a classmate handoff, physical sensor test, or Windows verification. The silent observation was omitted because the student reports that the instructor made it optional.

## Commands
From the application root after installing requirements:
```bash
python3 -m pytest -q
python3 capture_evidence.py --no-browser
```
The evidence script uses localhost port 5057 and a separate `test_runtime` folder. Before repeating, preserve and rename an existing `test_runtime` folder. Normal operation uses port 5000 and `runtime`.

## Actual results
- Unit/integration suite: **4 passed in 1.55 seconds**.
- HTTP dashboard returned a HEALTHY state with a synthetic 24°C input.
- Repeated 32°C input activated ALERT; accepted 27°C cleared it.
- Paused delivery produced STALE/DEGRADED, then FAILED.
- Empty input with source MISSING produced MISSING with no current accepted value.
- Killing the application child changed its PID from 6 to 12; the supervisor restarted it and the HTTP status returned HEALTHY.
- Replay record count before crash: 7; after recovery: 8.
- Original archive preserved: 8780 records.
- SQLite integrity check: **ok**.

## Evidence index
| File | Evidence |
|---|---|
| evidence/01_dashboard_healthy.json and .html | Healthy status and saved dashboard response |
| evidence/02_original_history.html | Original temperature archive returned over HTTP |
| evidence/03_alert_active.json and .html | Active high-temperature alert |
| evidence/04_alert_cleared.json and .html | Alert cleared after accepted low input |
| evidence/05_stale.json and .html | Stale delivery and unavailable current temperature |
| evidence/06_failed.json and .html | Failed health after extended interruption |
| evidence/07_missing.json and .html | Missing input preserved |
| evidence/08_recovered_healthy.json and .html | Healthy response after child restart |
| evidence/verification.json | Complete captured states, PID changes, counts, integrity result |
| evidence/application.jsonl | Actual application and supervisor events |
| evidence/process_output.txt | Captured process output |
| verification/TEST_RESULTS.txt | Automated test result and execution scope |

HTML files are saved HTTP responses, not screenshots. No browser screenshots were generated. The evidence script also creates a local database snapshot; the repository retains text/JSON evidence and does not include that generated binary database.
