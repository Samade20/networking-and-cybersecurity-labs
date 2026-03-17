# Splunk Brute Force Login Detection Lab

## Project Overview

This project demonstrates how Splunk Enterprise can be used to investigate authentication logs and detect suspicious login activity such as brute-force attacks and password spraying.

The dataset was uploaded into Splunk as a CSV file and analyzed using Splunk Processing Language (SPL) to identify failed login attempts, suspicious source IP addresses, frequently targeted usernames, and time-based attack patterns.

---

## Objectives

This investigation was carried out to determine:

- The total number of failed login attempts
- The IP address with the highest failed login attempts
- The username that appeared most frequently in the logs
- Any suspicious activity patterns that may indicate brute-force or password spraying behavior

---

## Dataset Fields

The dataset contains the following fields:

- `timestamp`
- `username`
- `src_ip`
- `status`
- `location`

---

## Tools Used

- Splunk Enterprise
- Splunk Processing Language (SPL)
- CSV authentication dataset
- GitHub for documentation
- Basic SOC analysis methodology

---

## Splunk Queries Used

### 1. Total Failed Login Attempts

```spl
index=login_dataset status=failed
| stats count as failed_login_attempts
```

### 2. IP Address With the Highest Failed Logins

```spl
index=login_dataset status=failed
| stats count by src_ip
| sort - count
```

### 3. Username Appearing Most Frequently

```spl
index=login_dataset
| stats count by username
| sort - count
```

### 4. Suspicious Brute-Force / Password Spraying Detection

```spl
index=login_dataset status=failed
| stats dc(username) as targeted_accounts count by src_ip
| sort - targeted_accounts
```

### 5. Failed Login Spikes Over Time

```spl
index=login_dataset status=failed
| timechart span=1h count
```

---

## Findings

### Total Failed Login Attempts
A total of **92 failed login attempts** were identified in the dataset.

### Most Suspicious IP Address
The IP address **203.0.113.45** generated the highest number of failed login attempts with **19 failures**.

### Username Appearing Most Frequently
The username **alice** appeared most frequently in the logs.

### Suspicious Pattern Observed
Multiple source IP addresses attempted login attempts against multiple usernames, which is consistent with **password spraying** or **brute-force attack behavior**.

The strongest suspicious IPs included:

- `203.0.113.45`
- `192.168.1.22`
- `198.51.100.23`
- `172.16.0.3`
- `8.8.8.8`

---

## Security Recommendations

- Enable account lockout after repeated failed logins
- Enforce multi-factor authentication (MFA)
- Monitor authentication failures continuously in Splunk
- Block or investigate suspicious source IPs
- Create automated Splunk alerts for brute-force activity

---

## Skills Demonstrated

- SIEM log analysis
- Splunk SPL query writing
- Authentication log investigation
- Brute-force detection
- Threat hunting
- Security reporting

---

## Project Structure

```text
splunk-bruteforce-detection-lab/
├── README.md
├── dataset/
├── queries/
├── results/
└── screenshots/
```
## Screenshots

### Splunk Search Results
![Search Results](screenshots/search_results.png)

### Total Failed Logins
![Total Failed Logins](screenshots/total_failed_logins.png)

### Top Attacking IPs
![Top Attacking IPs](screenshots/top_attacking_ips.png)

### Brute Force Detection
![Brute Force Detection](screenshots/brute_force_detection.png)

### Failed Login Spikes
![Failed Login Spikes](screenshots/failed_login_spikes.png)

## Splunk Projects

- [Splunk Brute Force Login Detection Lab](splunk-bruteforce-detection-lab/README.md)
