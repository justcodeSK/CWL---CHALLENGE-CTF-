# OSINT Investigation Report – `emp_exch`

## Table of Contents
1. Scope & Objective  
2. Methodology  
3. Tools Used  
4. Investigation Steps  
5. Findings  
6. Correlation Analysis  
7. Final Conclusion  

---

# 1. Scope & Objective

**Objective:**  
This OSINT investigation report analyzes the social media account **`emp_exch`** to identify its online presence, linked data, and behavioral patterns across various social media and public platforms.

The investigation focuses on gathering information from publicly available sources and correlating the discovered data to understand the digital footprint of the username.

**Note:**  
All information gathered during this investigation was obtained from **publicly accessible sources**.  
No attack, exploitation, phishing, or unauthorized access attempts were conducted during this investigation.

---

# 2. Methodology

The investigation followed a structured OSINT methodology:

1. Search the username across multiple social media platforms.
2. Collect available profile metadata and account details.
3. Cross-check details such as usernames, links, profile pictures, and activity.
4. Review posts and activity history for patterns and interests.
5. Correlate discovered data across platforms.
6. Build a basic digital identity timeline.
7. Document all findings with supporting screenshots.

---

# 3. Tools Used

The following OSINT tools and techniques were used during the investigation:

1. **Google Dorks** – Advanced search queries for locating indexed data.
2. **Sherlock** – Username enumeration across multiple platforms.
3. **whoami (Chrome Extension)** – Social profile discovery tool.
4. **Namechk** – Username availability checker across websites.
5. **WhatsMyName** – OSINT username enumeration tool.
6. **WhatsMyIPAddress** – Email analysis, IP lookup, and domain-related information.
7. **WHOIS Lookup** – Domain registration investigation.

---

# 4. Investigation Steps

## 4.1 Identify Platform Presence

### A. Google Dorks

The following Google search queries were used:

```
"emp_exch"
intitle:emp_exch
"emp_exch" filetype:pdf
"emp_exch" filetype:txt
```

**Findings:**

The username **emp_exch** appeared on:

- X (Twitter)
- Scribd documents
- Various OSINT or technical reports

Many references were related to **DevOps and OSINT activities**, indicating that the user may be involved in technical or cybersecurity-related communities.

---

### B. Username Enumeration – Namechk

Website used:

```
https://namechk.com
```

The username **emp_exch** was checked across multiple platforms.

**Findings:**

The username appears registered on multiple platforms including:

- Facebook
- Twitter (X)
- LinkedIn
- Instagram
- Tumblr
- Pinterest
- YouTube
- Flickr

However, **only the X (Twitter) account is active**.  
Other platforms returned **404 errors or inactive pages**, indicating that they are either unused or reserved.

---

### C. Username Search – WhatsMyName

Website used:

```
https://whatsmyname.me
```

The tool was used to scan for the username across various websites.

**Findings:**

Results showed matches on several platforms including:

- X (Twitter)
- Arch Linux GitLab
- Udemy
- Znanija
- SourceForge

However, the **X account appeared to be the primary active account**.  
Other results were either inactive profiles or references inside technical reports.

Example results included:

```
https://x.com/emp_exch
https://gitlab.archlinux.org/emp_exch
https://www.udemy.com/user/emp_exch/
https://sourceforge.net/u/emp_exch/profile
```

---

### D. Username Enumeration – Sherlock

Command used:

```
python3 sherlock emp_exch
```

Output saved using:

```
sherlock emp_exch --output emp_exch.txt
```

**Findings:**

Sherlock detected multiple platform matches, but verification showed that **only the X (Twitter) account was currently active and accessible**.

---

### E. Profile Discovery – whoami (Chrome Extension)

The **whoami** browser extension was used to identify additional accounts associated with the username.

**Findings:**

The username appears across several **technical and CTF-related platforms**, suggesting that the user may participate in:

- OSINT communities
- DevOps learning environments
- Cybersecurity or technical forums

This made the username a strong starting point for deeper OSINT investigation.

---

# 5. Profile Metadata Collection

Information collected from the **X (Twitter) profile**:

**Username:**  
`@emp_exch`

**Past Username Changes:**  
2 previous changes (last recorded in **October 2023**)

**Display Name:**  
emp_exch

**Bio:**  
Learning Automation in Online & On-Site MS Exchange

**Position Mentioned:**  
Associate Technician at **PwrGrid Mgmt**

**Location:**  
Not specified

**Account Created:**  
April 2023

**Followers / Following:**  
9 Followers / 16 Following

**Access Method:**  
Connected via Web

**Observations:**

The account follows several **major technology and cloud-related organizations**, including:

- Microsoft
- Azure
- AWS
- Google
- Kubernetes
- IBM Cloud

This suggests strong interests in:

- Cloud infrastructure
- DevOps automation
- Enterprise systems
- Microsoft Exchange environments

---

# 6. Profile Picture Analysis

The X profile uses a **simple letter "G" as the display picture**.

Possible interpretations include:

- The first letter of the user's real name
- The first letter of a Gmail account
- A placeholder profile image

However, **there is no evidence confirming the actual identity behind the letter**.

---

# 7. Cross-Platform Analysis

## A. Email Investigation

No publicly visible **email address** was found linked to the username across any platforms.

---

## B. WHOIS Domain Investigation

A WHOIS search was conducted to check whether the username had any associated domain registrations.

**Findings:**

No domains were found to be registered under the username **emp_exch**.

---

# 8. Correlation Analysis

Some technical publications and automation-related links shared on **GitHub** were also posted on the **X account**.

This strengthens the assumption that:

- The same individual manages both accounts
- The activity is related to **DevOps learning and automation practices**

The consistency of topics across platforms suggests a **technical background focused on infrastructure automation and cloud technologies**.

---

# 9. Final Conclusion

Based on the OSINT investigation, the username **emp_exch** appears to belong to an individual actively involved in **technology, DevOps, and cybersecurity-related communities**.

Key observations include:

- The primary active presence is on **X (Twitter)**.
- Interests strongly align with **DevOps, cloud infrastructure, and automation**.
- The account follows multiple **cloud and technology companies**.
- Cross-platform references indicate engagement with **technical learning platforms**.
- No direct **real-world identity exposure** or sensitive personal data was discovered.

Overall, the digital footprint of **emp_exch** indicates a technically oriented user with a strong focus on **DevOps and infrastructure automation**, with no major privacy leaks or sensitive information exposed publicly.
