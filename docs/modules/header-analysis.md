# Header Analysis Module

> **Module ID:** HEADER-ANALYSIS-001  
> **Version:** 2.0.0  
> **Phase:** 4 - Specialized Modules  
> **Classification:** Educational Email Header Forensics

---

## 1. Overview

This module provides deep analysis of email header structures for forensic investigation. Headers contain metadata about message origin, routing, authentication, and processing history.

---

## 2. Email Header Structure

### 2.1 Header Format

**RFC 5322 Format:**
```
header-field = field-name ":" [ field-value ]
field-name   = 1*ftext
field-value  = *( printable / space / tab )

Headers separated by CRLF (\r\n)
Continuation lines start with whitespace
```

**Example Structure:**
```
From: sender@example.com
To: recipient@example.com
Subject: Test Message
Date: Mon, 15 Jan 2024 14:30:00 +0000
Message-ID: <abc123@example.com>
MIME-Version: 1.0
Content-Type: text/plain; charset="UTF-8"

[Blank line separates headers from body]
[Message body follows]
```

### 2.2 Header Categories

**Origin Headers:**
```
From, Sender, Reply-To
Date, Message-ID
```

**Destination Headers:**
```
To, Cc, Bcc (envelope only)
```

**Transport Headers:**
```
Received, Return-Path
Delivered-To, Envelope-From
```

**Content Headers:**
```
MIME-Version, Content-Type
Content-Transfer-Encoding
Content-Disposition
```

**Authentication Headers:**
```
Authentication-Results
DKIM-Signature, ARC-Seal
```

**Optional/Extension Headers:**
```
X-Mailer, X-Originating-IP
X-Priority, X-MS-Exchange
```

---

## 3. Received Field Analysis

### 3.1 Received Header Syntax

**Standard Format:**
```
Received: from [hostname] ([IP address])
        by [receiving-host] ([receiving-IP])
        with [protocol] id [queue-id]
        for [recipient-address];
        [date-time]
```

**Protocol Values:**
```
ESMTP    - Extended SMTP (encrypted)
SMTP     - Standard SMTP
ESMTPS   - SMTP with TLS
ESMTPSA  - SMTP with TLS + Auth
LMTP     - Local Mail Transfer Protocol
HTTP     - Webmail submission
```

### 3.2 Received Header Components

**From Clause:**
```
"from hostname (HELO hostname [IP])"

Analysis points:
- HELO should match hostname
- IP should resolve to hostname
- Private IPs indicate internal origin
```

**By Clause:**
```
"by receiving-host with protocol id queue-id"

Analysis points:
- Receiving server identity
- Protocol indicates security level
- Queue ID for server log correlation
```

**For Clause:**
```
"for recipient@domain.com"

Analysis points:
- Verify matches intended recipient
- Multiple "for" clauses may indicate forwarding
```

**Envelope-From:**
```
"(envelope-from <bounce@domain.com>)"

Analysis points:
- Should relate to Return-Path
- Used for SPF verification
- May differ from header From
```

### 3.3 Multi-Hop Analysis

**Example Chain:**
```
Received: from mx-final.google.com
        by inbox.google.com
        for <user@gmail.com>;
        Mon, 15 Jan 2024 14:35:22 +0000

Received: from outbound.sendgrid.net (198.37.147.243)
        by mx-final.google.com with ESMTPS id abc
        for <user@gmail.com>;
        Mon, 15 Jan 2024 14:35:18 +0000

Received: from [10.0.1.15] (unknown [203.0.113.50])
        by outbound.sendgrid.net with HTTP id def;
        Mon, 15 Jan 2024 14:35:15 +0000
```

**Hop Analysis:**
```
Hop 1: Web submission from 203.0.113.50
       → Internal IP 10.0.1.15 (NAT)
       → Time: 14:35:15

Hop 2: SendGrid outbound server
       → Protocol: ESMTPS (encrypted)
       → Time: 14:35:18 (+3 seconds)

Hop 3: Google final delivery
       → Time: 14:35:22 (+4 seconds)

Total transit: 7 seconds (normal)
```

### 3.4 Anomaly Detection in Received Headers

**Suspicious Patterns:**
```
Missing timestamps
Identical timestamps on different hops
Future dates
Private IP addresses in middle of chain
Inconsistent timezone progression
Hostname/IP mismatches
```

**Private IP Ranges:**
```
10.0.0.0/8      - Private Class A
172.16.0.0/12   - Private Class B
192.168.0.0/16  - Private Class C
127.0.0.0/8     - Loopback
169.254.0.0/16  - Link-local
```

---

## 4. Authentication Results

### 4.1 Authentication-Results Format

**RFC 8601 Format:**
```
Authentication-Results: authserv-id;
       [method]=[result] [reason] [properties];
       [method]=[result] ...
```

**Example:**
```
Authentication-Results: mx.google.com;
       spf=pass (google.com: domain of sender@example.com
       designates 203.0.113.50 as permitted sender)
       smtp.mailfrom=sender@example.com;
       dkim=pass header.i=@example.com header.s=selector1
       header.b=hash123;
       dmarc=pass (p=REJECT sp=REJECT dis=NONE)
       header.from=example.com
```

### 4.2 Authentication Methods

**SPF Results:**
```
pass       - IP authorized
fail       - IP not authorized
softfail   - Not authorized, but not enforced
neutral    - No policy
none       - No SPF record
temperror  - Temporary DNS error
permerror  - Permanent SPF error
```

**DKIM Results:**
```
pass       - Valid signature
fail       - Invalid signature
none       - No signature present
policy     - Signature present but not for From domain
neutral    - Signature processed, no opinion
 temperror - Temporary verification error
permerror  - Permanent verification error
```

**DMARC Results:**
```
pass       - SPF or DKIM aligned
fail       - Neither aligned
none       - No DMARC record
```

**ARC Results (Authenticated Received Chain):**
```
arc=pass   - Chain validated
arc=fail   - Chain broken
```

### 4.3 Authentication Properties

**Common Properties:**
```
smtp.mailfrom      - Envelope sender for SPF
header.from        - From header domain for DMARC
header.i           - DKIM signing identity
header.s           - DKIM selector
header.b           - DKIM signature data
policy.[option]    - Policy override reasons
```

---

## 5. X-Headers Analysis

### 5.1 Common X-Headers

**Origination:**
```
X-Originating-IP: 198.51.100.25
X-Originating-Email: [sender@example.com]
X-Originating-Name: [Sender Name]
```

**Priority/Classification:**
```
X-Priority: 1 (Highest) to 5 (Lowest)
X-MSMail-Priority: High/Normal/Low
Importance: high/normal/low
X-Mailer: Microsoft Outlook 16.0
```

**Spam/Antivirus:**
```
X-Spam-Status: Yes, score=5.2
X-Spam-Score: 5.2
X-Virus-Status: Clean
X-Spam-Flag: YES
```

**Mailing List:**
```
List-ID: <list.example.com>
List-Unsubscribe: <mailto:unsubscribe@example.com>
X-Mailing-List: list@example.com
```

### 5.2 X-Header Interpretation

**Microsoft-Specific:**
```
X-MS-Exchange-Organization-AuthSource
X-MS-Exchange-Organization-AuthAs
X-MS-Has-Attach: yes
X-MS-TNEF-Correlator
```

**Google-Specific:**
```
X-Gm-Message-State
X-Google-DKIM-Signature
X-Gm-Spam: 0
X-Gm-Phishy: 0
```

**Apple-Specific:**
```
X-Mailer: Apple Mail (2.3654)
X-Apple-Base-Url
X-Apple-Mail-Remote-Attachments
```

---

## 6. Server Fingerprinting

### 6.1 Server Identification

**Mail Transfer Agents (MTAs):**
```
Postfix: "with ESMTP id XXXXX"
Exim:    "with esmtp (Exim X.XX)"
Sendmail: "with SMTP id XXXXX"
Microsoft: "with Microsoft SMTP Server"
```

**Queue ID Patterns:**
```
Postfix: 8-12 character alphanumeric
Exim:    6 character alphanumeric + timestamp
Sendmail: Often includes process ID
qmail:   Numeric timestamp based
```

**EHLO/HELO Patterns:**
```
Well-configured: FQDN (mail.example.com)
Default:         IP literal ([192.168.1.1])
Poorly config:   Bare hostname (mail)
Suspicious:      Generic (localhost, mailserver)
```

### 6.2 Fingerprinting Analysis

**Legitimate Indicators:**
```
- Consistent server naming
- Proper reverse DNS
- Matching HELO/FQDN
- Known MTA signatures
- Standard queue ID format
```

**Suspicious Indicators:**
```
- Generic HELO names
- Missing or private HELO
- Unknown/custom MTA
- Inconsistent formatting
- Spoofed server names
```

---

## 7. Delay Analysis

### 7.1 Delay Types

**Queuing Delays:**
```
Greylisting: Temporary rejection, retry delay
Rate limiting: Throttling at sending server
Queue congestion: High volume delays
Scheduled delivery: Intentional delay
```

**Transit Delays:**
```
Network latency: Geographic distance
Server processing: Resource constraints
DNS delays: Resolution timeouts
Authentication delays: Verification overhead
```

**Delivery Delays:**
```
Recipient verification: Validating address
Content filtering: Scanning attachments
Quota checks: Recipient mailbox full
Forwarding delays: Additional routing
```

### 7.2 Calculating Delays

**Per-Hop Delay:**
```
Delay = Current_Timestamp - Previous_Timestamp

Example:
  Hop 1: 14:30:00
  Hop 2: 14:32:30
  Delay: 2 minutes 30 seconds
```

**Acceptable Ranges:**
```
Within same datacenter: < 1 second
Same continent: 1-10 seconds
International: 10-60 seconds
Via greylisting: 5-15 minutes (first attempt)
```

### 7.3 Delay Investigation

**Investigation Steps:**
```
1. Calculate delay between each hop
2. Identify which hop caused delay
3. Check for temporary failure codes
4. Look for retry indicators
5. Correlate with authentication results
```

**Long Delay Causes:**
```
Hours: Greylisting, queue issues, scheduled delivery
Days: Server outages, DNS problems, manual release
Weeks: Archival and retrieval, legal hold
```

---

## 8. Header Security Analysis

### 8.1 Sensitive Information Exposure

**Potentially Sensitive Headers:**
```
X-Originating-IP        - Sender's IP address
Received: from [IP]     - Internal network structure
X-Envelope-From         - Internal usernames
Message-ID domain       - Internal hostnames
X-Mailer version        - Software versions (vulnerability intel)
```

**Information Disclosure Risks:**
```
Internal IPs: Network topology exposure
Software versions: Targeted attack preparation
User agents: Client environment details
Hostnames: Infrastructure enumeration
Paths: File system structure
```

### 8.2 Header Injection Detection

**Injection Indicators:**
```
Multiple headers with same name (non-standard)
Headers appearing out of order
Malformed header syntax
Unexpected header combinations
Unicode in header names
```

**Testing for Injection:**
```
Look for:
- Newlines in header values (CRLF injection)
- Encoding anomalies
- Header name spoofing
- Value manipulation
```

---

## 9. Confidence Ratings

| Header Element | Confidence | Notes |
|---------------|-----------|-------|
| Received timestamps | HIGH | Server-generated |
| IP addresses | HIGH | Connection records |
| Queue IDs | HIGH | Server logs verifiable |
| Authentication results | HIGH | Cryptographic |
| X-Header values | MEDIUM | Implementation varies |
| HELO names | LOW | Easily spoofed |
| Transit path | MEDIUM | May include forged entries |

---

## 10. Limitations

### 10.1 Header Trustworthiness

**Trust Hierarchy (highest to lowest):**
```
1. Final Received header (destination server)
2. Authentication-Results (destination verification)
3. Earlier Received headers
4. Content headers (sender-controlled)
5. X-Headers (varies by implementation)
6. Origination headers (easily forged)
```

### 10.2 Analysis Constraints

1. **Early headers may be forged** - Only final Received is trustworthy
2. **Internal routing hidden** - NAT masks true topology
3. **Clock skew affects timing** - Server time may be wrong
4. **Mailing lists modify headers** - Original headers altered
5. **Gateways may strip headers** - Information loss

---

## 11. Command Reference

| Command | Purpose |
|---------|---------|
| `/analyze-email` | Full header forensics |
| `/email-auth-check` | Authentication verification |
| `/header-trace` | Visual routing path |
| `/delay-analysis` | Transit timing analysis |
| `/security-check` | Exposure assessment |

---

## 12. Quick Reference

**Header Reading Order:**
```
Bottom-to-Top for chronological routing
Top-to-Bottom for processing priority
```

**Critical Headers:**
```
1. Authentication-Results (authenticity)
2. Received chain (routing)
3. Return-Path (bounce address)
4. DKIM-Signature (integrity)
5. From/Date (sender claims)
```

**Red Flag Checklist:**
```
□ SPF/DKIM/DMARC fail
□ Private IPs in wrong position
□ Impossible routing path
□ Future timestamps
□ Missing authentication headers
□ Suspicious X-headers
□ Mismatched domains
□ Unusual delays
```

---

*Header Analysis Module v2.0.0*  
*Part of OSINT Investigator Skill - Phase 4*
