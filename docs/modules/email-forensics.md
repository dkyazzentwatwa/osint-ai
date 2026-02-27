# Email Forensics Module

> **Module ID:** EMAIL-FOR-001  
> **Version:** 2.0.0  
> **Phase:** 4 - Specialized Modules  
> **Classification:** Educational Email Analysis

---

## 1. Overview

Email forensics examines message headers, routing paths, and content to determine origin, authenticity, and transmission details. This module covers header analysis, authentication verification, and anomaly detection.

---

## 2. Header Field Explanations

### 2.1 Essential Header Fields

**From:**
```
Format: From: "Display Name" <address@domain.com>
Purpose: Indicates sender (easily forged)
Analysis: Verify against Return-Path, DKIM signatures
```

**To:**
```
Format: To: recipient@example.com
Purpose: Primary recipient(s)
Analysis: Check for unexpected recipients
```

**Cc (Carbon Copy):**
```
Format: Cc: other@example.com
Purpose: Secondary recipients (visible to all)
Analysis: Check who else received the message
```

**Bcc (Blind Carbon Copy):**
```
Format: Not visible in received headers
Purpose: Hidden recipients
Analysis: Cannot determine from headers
```

**Subject:**
```
Format: Subject: Meeting Tomorrow
Purpose: Message topic
Analysis: May contain urgency tactics in phishing
```

**Date:**
```
Format: Date: Mon, 15 Jan 2024 14:30:00 +0500
Purpose: Sending timestamp
Analysis: Check for future/past dates, timezone consistency
```

**Message-ID:**
```
Format: Message-ID: <unique-id@server.domain>
Purpose: Globally unique identifier
Analysis: Check format consistency, uniqueness
```

**MIME-Version:**
```
Format: MIME-Version: 1.0
Purpose: Indicates MIME formatting
Analysis: Should be 1.0 for modern email
```

**Content-Type:**
```
Format: Content-Type: text/plain; charset="UTF-8"
Purpose: Defines message format
Analysis: Check for suspicious attachments
```

### 2.2 Routing Header Fields

**Received:** (Most Important for Tracking)
```
Format:
Received: from server1.example.com (HELO mail.example.com)
        by server2.receiving.com with ESMTPS id 12345
        for <recipient@receiving.com>
        (envelope-from <sender@example.com>)
        id ABC123; Mon, 15 Jan 2024 14:30:00 +0000

Purpose: Records each server hop
Analysis: Read bottom-to-top (oldest first)
```

**Return-Path:**
```
Format: Return-Path: <bounce@example.com>
Purpose: Bounce address (envelope sender)
Analysis: Should match From domain (roughly)
```

**Delivered-To:**
```
Format: Delivered-To: recipient@receiving.com
Purpose: Final delivery address
Analysis: Confirm intended recipient
```

**Envelope-From:**
```
Format: Envelope-From: <sender@example.com>
Purpose: SMTP envelope sender
Analysis: Compare to header From field
```

### 2.3 Authentication Header Fields

**Authentication-Results:**
```
Format:
Authentication-Results: mx.google.com;
       spf=pass smtp.mailfrom=example.com;
       dkim=pass header.i=@example.com;
       dmarc=pass header.from=example.com

Purpose: Summarizes authentication checks
Analysis: Look for "pass" or "fail" results
```

**DKIM-Signature:**
```
Format:
DKIM-Signature: v=1; a=rsa-sha256; d=example.com; s=selector;
       c=relaxed/relaxed; q=dns/txt; t=1705327800;
       h=from:to:subject:date:message-id;
       bh=base64hash;
       b=signaturedata

Purpose: Domain-level signature
Analysis: Verify signing domain matches From domain
```

**ARC-Authentication-Results:**
```
Purpose: Authentication Results for forwarded email
Analysis: Preserves authentication through forwarding
```

---

## 3. Routing Path Analysis

### 3.1 Reading the Received Chain

**Reading Order:**
```
Received: from final-server...        ← Most recent (top)
Received: from middle-server...       
Received: from origin-server...       ← Original (bottom)
```

**Example Analysis:**
```
Received: from mail-receiver.google.com (HELO mx.google.com)
          by inbox with ESMTPS id abc123
          for <user@gmail.com>
          Mon, 15 Jan 2024 14:35:00 +0000

Received: from outbound.company.com (HELO mail.company.com [203.0.113.50])
          by mail-receiver.google.com with ESMTPS id def456
          for <user@gmail.com>
          Mon, 15 Jan 2024 14:30:00 +0000

Received: from workstation.local ([192.168.1.100])
          by mail.company.com with ESMTPS id ghi789
          for <user@gmail.com>
          Mon, 15 Jan 2024 14:25:00 +0000
```

**Analysis:**
```
Origin: 192.168.1.100 (internal workstation)
First hop: mail.company.com (203.0.113.50)
Second hop: Google mail servers
Final: User inbox

Time analysis: 5 minutes total transit time (normal)
```

### 3.2 Geographic Analysis

**IP Geolocation:**
```
Use IP addresses in Received headers
Lookup location via:
  - whois lookup
  - GeoIP databases
  - Online services (ipinfo.io, etc.)
```

**Timezone Analysis:**
```
Check timestamp offsets in each Received header
Should follow logical geographic progression

Suspicious:
  Hop 1: +08:00 (Asia)
  Hop 2: -05:00 (US East)
  Hop 3: +08:00 (Asia)
  → Unlikely routing path
```

### 3.3 Timing Analysis

**Transit Time Calculation:**
```
Timestamp difference between hops

Normal: Seconds to minutes per hop
Suspicious:
  - Negative time (clock skew or forgery)
  - Very long delays (hours, days)
  - Identical timestamps (copied headers)
```

---

## 4. SPF/DKIM/DMARC Verification Guide

### 4.1 SPF (Sender Policy Framework)

**Purpose:** Verify sending server is authorized

**How It Works:**
```
1. Extract domain from Return-Path (or HELO)
2. Query DNS for SPF record
3. Check if sending IP matches authorized IPs
4. Result: pass, fail, softfail, neutral, none
```

**SPF Record Example:**
```
example.com.  IN  TXT  "v=spf1 ip4:203.0.113.0/24 include:_spf.google.com -all"

Mechanisms:
  ip4:       IPv4 address range
  ip6:       IPv6 address range
  include:   Reference another SPF record
  a:         Domain's A records
  mx:        Domain's MX records
  all:       Default result (-all = hard fail, ~all = soft fail)
```

**Verification Steps:**
```
1. Identify sending IP from earliest Received header
2. Query: dig TXT example.com
3. Parse SPF record
4. Check if IP matches any mechanism
5. Note result in Authentication-Results header
```

### 4.2 DKIM (DomainKeys Identified Mail)

**Purpose:** Cryptographic signature verifying message integrity

**How It Works:**
```
Signing (sending side):
1. Hash message headers and body
2. Sign hash with private key
3. Add DKIM-Signature header

Verification (receiving side):
1. Extract selector and domain from DKIM header
2. Query DNS for public key: selector._domainkey.domain.com
3. Verify signature matches content
4. Result: pass, fail, none
```

**DKIM DNS Record:**
```
selector._domainkey.example.com. IN TXT "v=DKIM1; k=rsa; p=MIGfMA0G..."
```

**Verification Steps:**
```
1. Extract d= and s= from DKIM-Signature header
2. Query: dig TXT selector._domainkey.domain.com
3. Extract public key
4. Verify signature cryptographically
5. Check body hash matches
```

### 4.3 DMARC (Domain-based Message Authentication)

**Purpose:** Policy enforcement for SPF/DKIM

**How It Works:**
```
1. Check SPF result
2. Check DKIM result
3. Verify alignment (domain matches From header)
4. Apply policy based on DMARC record
```

**DMARC DNS Record:**
```
_dmarc.example.com. IN TXT "v=DMARC1; p=reject; rua=mailto:dmarc@example.com; pct=100"

Policies:
  p=none:     Monitor only
  p=quarantine: Mark as suspicious
  p=reject:   Reject failed messages
```

**Alignment Modes:**
```
Strict (aspf=s, adkim=s): Exact domain match required
Relaxed (default): Organizational domain match acceptable
```

### 4.4 Verification Checklist

```
□ SPF Result: pass/fail/none
□ DKIM Result: pass/fail/none
□ DMARC Result: pass/fail/none
□ SPF Alignment: domain matches From?
□ DKIM Alignment: signing domain matches From?
□ Overall Authentication: pass or suspicious?
```

**Authentication Outcomes:**
```
Fully Authenticated:
  SPF: pass + aligned
  DKIM: pass + aligned
  → High confidence in sender identity

Partially Authenticated:
  SPF: pass, DKIM: none
  or
  DKIM: pass, SPF: fail
  → Moderate confidence

Unauthenticated:
  SPF: none, DKIM: none
  → No verification possible

Authentication Failed:
  SPF: fail or DKIM: fail
  → Likely spoofed or misconfigured
```

---

## 5. Email Client Fingerprinting

### 5.1 Client Identification Clues

**Message-ID Format:**
```
Outlook: <GUID@domain.com>
Gmail: <random-hash@mail.gmail.com>
Apple Mail: <GUID@MacBook-Pro.local>
Thunderbird: <GUID@domain>
Yahoo: <random@yahoo.com>
```

**X-Mailer Header:**
```
X-Mailer: Microsoft Outlook 16.0
X-Mailer: Apple Mail (2.3654.120.0.1.13)
X-Mailer: Mozilla Thunderbird
```

**Content Formatting:**
```
Apple Mail: Specific MIME boundaries
Outlook: winmail.dat attachments
Gmail: Specific HTML structure
Yahoo: Characteristic footer formatting
```

### 5.2 Fingerprinting Analysis

**Consistency Check:**
```
Claimed client vs header signatures:
  From: iPhone user
  X-Mailer: Outlook for Windows
  → Inconsistency (investigate)
```

**Device Correlation:**
```
Message-ID domain matches claimed service
X-Originating-IP reveals ISP/location
User-Agent in webmail reveals browser
```

---

## 6. Timezone Analysis

### 6.1 Timestamp Sources

**Header Timestamps:**
```
Date: Sender's claimed time
Received: Each server's time
```

**Timezone Formats:**
```
Offset: +0500, -0800
Named: PST, EST, CET (avoid in modern email)
UTC: +0000, GMT
```

### 6.2 Timezone Verification

**Consistency Checks:**
```
1. Compare Date header timezone to sender's location
2. Check Received header timezones follow geographic path
3. Verify no impossible timezone jumps
4. Confirm business hours alignment
```

**Example Analysis:**
```
Claimed sender: New York (EST/EDT, -05:00/-04:00)
Date header: +0100 (CET)
→ Timezone mismatch (unless traveling)

If traveling:
  Check IP geolocation matches claimed timezone
  Verify DKIM signature from claimed domain
```

---

## 7. Suspicious Pattern Detection

### 7.1 Spoofing Indicators

**Header Mismatches:**
```
From domain ≠ Return-Path domain
From display name ≠ actual sender
Envelope-From ≠ header From
```

**Authentication Failures:**
```
SPF: fail
DKIM: fail
DMARC: fail
→ High probability of spoofing
```

**Routing Anomalies:**
```
Unusual number of hops
Suspicious intermediate servers
Geographically impossible routing
Private IPs in unexpected positions
```

### 7.2 Phishing Indicators

**Common Tactics:**
```
Urgent language in Subject
Display name spoofing (CEO, IT Support)
Suspicious links (URL different from text)
Unexpected attachments
Poor grammar/spelling
Mismatched logos/branding
```

**Technical Indicators:**
```
Missing authentication headers
Forwarded from suspicious addresses
Recently registered sender domains
IP addresses from high-risk countries
Mismatched encodings
```

### 7.3 Verification Command

**Command:** `/analyze-email`

**Input:** Email header text
**Process:**
1. Parse all header fields
2. Trace routing path
3. Verify SPF/DKIM/DMARC
4. Check for anomalies
5. Assess authenticity
**Output:** Forensic analysis report

---

## 8. Confidence Ratings

| Finding | Confidence | Notes |
|---------|-----------|-------|
| Routing path | HIGH | Server logs are reliable |
| Authentication results | HIGH | Cryptographic verification |
| Client fingerprinting | MEDIUM | Heuristic matching |
| Geographic origin | MEDIUM | IP geolocation accuracy varies |
| Timezone consistency | HIGH | Mathematical verification |
| Spoofing detection | HIGH | Authentication failures |

---

## 9. Limitations

### 9.1 Technical Limitations

1. **Headers can be forged** - Early Received headers may be fake
2. **Private IPs** - NAT hides true origin
3. **Proxy servers** - May obscure routing
4. **Clock skew** - Server time may be wrong
5. **Encrypted transit** - Some details hidden in TLS

### 9.2 Analysis Limitations

1. **Bcc recipients** - Invisible in headers
2. **Forwarded email** - Complex header chains
3. **Mailing lists** - Modified headers
4. **Delayed delivery** - Queued messages show old timestamps
5. **Time zone ambiguity** - Multiple interpretations possible

---

## 10. Command Reference

| Command | Purpose | Input |
|---------|---------|-------|
| `/analyze-email` | Full header analysis | Email headers |
| `/email-auth-check` | Authentication verification | Email headers |
| `/trace-route` | Routing path visualization | Email headers |
| `/verify-sender` | Sender authenticity | Email headers |

---

*Email Forensics Module v2.0.0*  
*Part of OSINT Investigator Skill - Phase 4*
