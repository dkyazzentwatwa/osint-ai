# Recon Vectors Playbook

This file contains detailed search playbooks for each target type. When running `/recon`, identify the target type and follow the appropriate playbook below.

---

## Table of Contents
1. Domain Recon
2. Person Recon
3. Email Recon
4. Username Recon
5. IP Address Recon
6. Organization Recon
7. Phone Number Recon

---

## 1. Domain Recon

**Goal:** Map the domain's infrastructure, ownership, history, and associated entities.

### Search Sequence

**Ownership & Registration:**
- Search: `"example.com" WHOIS` or `whois example.com site:who.is OR site:whois.domaintools.com`
- Look for: Registrant name, organization, email, registration date, expiry, name servers
- Pivot on: Registrant email (often reveals other domains), registrant name

**DNS & Infrastructure:**
- Search: `"example.com" DNS records` or `site:securitytrails.com "example.com"`
- Look for: A records (IP addresses), MX records (email provider), NS records, TXT records (SPF, DKIM)
- Pivot on: IP addresses (what else is hosted there?), email provider choice

**Technology Stack:**
- Search: `site:builtwith.com "example.com"` or `"example.com" technology stack`
- Look for: CMS, frameworks, analytics tools, CDN, hosting provider
- Pivot on: Unusual technologies (might indicate the developer's preferences)

**Historical Snapshots:**
- Search: `site:web.archive.org "example.com"` or `"example.com" wayback machine`
- Look for: How the site has changed over time, old team pages, removed content
- Pivot on: Names that appeared on old versions but were removed

**Subdomains & Related Domains:**
- Search: `site:*.example.com` or `"example.com" subdomain enumeration`
- Search: `"example" site:crt.sh` (certificate transparency logs)
- Look for: dev/staging subdomains, internal tools, forgotten services

**Content & Exposure:**
- Search: `site:example.com filetype:pdf OR filetype:doc OR filetype:xls`
- Search: `site:example.com inurl:admin OR inurl:login OR inurl:config`
- Search: `"example.com" site:github.com` (code repos referencing the domain)
- Search: `"example.com" site:pastebin.com OR site:paste.org`
- Look for: Exposed documents, admin panels, credentials in code repos

**Reputation & Mentions:**
- Search: `"example.com" -site:example.com` (what others say about it)
- Search: `"example.com" site:reddit.com`
- Search: `"example.com" review OR scam OR complaint`

---

## 2. Person Recon

**Goal:** Build a profile of the person's digital footprint, professional history, and public activity.

### Search Sequence

**Identity Basics:**
- Search: `"Full Name"` (in quotes for exact match)
- Search: `"Full Name" city OR location` (narrow by geography if known)
- Search: `"Full Name" site:linkedin.com`
- Look for: Professional title, employer, location, education
- Pivot on: Employer name, university, any usernames visible in profiles

**Professional Presence:**
- Search: `"Full Name" site:linkedin.com OR site:crunchbase.com`
- Search: `"Full Name" speaker OR conference OR presentation`
- Search: `"Full Name" author OR published OR paper`
- Search: `"Full Name" patent site:patents.google.com`
- Look for: Professional history, expertise areas, public speaking, publications

**Social Media:**
- Search: `"Full Name" site:twitter.com OR site:x.com`
- Search: `"Full Name" site:facebook.com`
- Search: `"Full Name" site:instagram.com`
- Search: `"Full Name" site:reddit.com`
- Search: `"Full Name" site:medium.com OR site:substack.com`
- Pivot on: Any usernames found (run username recon on each)

**Public Records (US-focused, adapt for other jurisdictions):**
- Search: `"Full Name" site:courtlistener.com` (court records)
- Search: `"Full Name" site:opencorporates.com` (corporate filings)
- Search: `"Full Name" site:sec.gov` (SEC filings, for executives)
- Search: `"Full Name" property records [county/state]`
- Note: Always clarify that public records may match other people with the same name

**Documents & Media:**
- Search: `"Full Name" filetype:pdf`
- Search: `"Full Name" site:youtube.com` (videos, interviews)
- Search: `"Full Name" site:slideshare.net OR site:speakerdeck.com`

---

## 3. Email Recon

**Goal:** Determine what accounts, services, and data are associated with an email address.

### Search Sequence

**Direct Presence:**
- Search: `"email@example.com"` (exact match — where does this email appear publicly?)
- Look for: Forum posts, mailing list archives, data breach mentions, code commits

**Associated Accounts:**
- Search: `"email@example.com" site:github.com`
- Search: `"email@example.com" site:gravatar.com` (profile photo link)
- Search: `"email@example.com" site:keybase.io` (crypto identity)
- Pivot on: Any usernames, real names, or profile photos found

**Domain Analysis (if custom domain):**
- If the email uses a custom domain (not gmail/outlook/yahoo), run Domain Recon on that domain
- Search: `site:example.com` to understand the organization

**Breach Exposure:**
- Search: `"email@example.com" breach OR leak OR exposed`
- Direct user to: haveibeenpwned.com (suggest they check manually)
- Note: Never attempt to access actual breach data

**Mailing Lists & Forums:**
- Search: `"email@example.com" site:groups.google.com`
- Search: `"email@example.com" site:lists.apache.org OR site:sourceforge.net`
- Search: `"email@example.com" forum OR community`

---

## 4. Username Recon

**Goal:** Find all platforms where a username is registered and build a profile from aggregate activity.

### Search Sequence

**Cross-Platform Search:**
- Search: `"username" site:twitter.com OR site:x.com`
- Search: `"username" site:reddit.com`
- Search: `"username" site:github.com`
- Search: `"username" site:instagram.com`
- Search: `"username" site:youtube.com`
- Search: `"username" site:tiktok.com`
- Search: `"username" site:linkedin.com`
- Search: `"username" site:medium.com`
- Search: `"username" site:hackernews.com OR site:news.ycombinator.com`
- Search: `"username" site:keybase.io`
- Search: `"username" site:mastodon.social OR site:fosstodon.org`

**Username Lookup Services (via search):**
- Search: `"username" namechk OR namecheck OR social media check`
- Direct user to: namechk.com, whatsmyname.app (suggest manual checking)

**Content Analysis:**
- For each platform where the username is found, search for recent activity
- Look for: Bio text (often reveals real name, location, interests), posting patterns, shared links
- Cross-reference: Does the bio on Platform A mention Platform B?

**Pivot Extraction:**
- Real names mentioned in bios
- Emails visible in profiles
- Linked websites or other social accounts
- Location information

---

## 5. IP Address Recon

**Goal:** Determine ownership, geolocation, associated services, and reputation of an IP address.

### Search Sequence

**Geolocation & Ownership:**
- Search: `"IP_ADDRESS" geolocation OR location`
- Search: `"IP_ADDRESS" site:ipinfo.io OR site:shodan.io OR site:censys.io`
- Look for: ISP, ASN, approximate location, organization

**Hosted Services:**
- Search: `"IP_ADDRESS" site:shodan.io` (open ports, services)
- Search: `"IP_ADDRESS" hostname OR reverse DNS`
- Look for: Web servers, mail servers, open services

**Reputation:**
- Search: `"IP_ADDRESS" blacklist OR spam OR malicious OR abuse`
- Search: `"IP_ADDRESS" site:abuseipdb.com`
- Look for: Abuse reports, blacklist presence, threat intelligence mentions

**Associated Domains:**
- Search: `"IP_ADDRESS" site:securitytrails.com` (reverse IP lookup)
- Look for: All domains pointing to this IP (shared hosting reveals associations)

---

## 6. Organization Recon

**Goal:** Map the organization's structure, key personnel, digital assets, and public posture.

### Search Sequence

**Corporate Basics:**
- Search: `"OrgName" site:opencorporates.com OR site:dnb.com`
- Search: `"OrgName" incorporation OR registered OR founded`
- Look for: Registration date, jurisdiction, officers, registered agent

**Leadership & Personnel:**
- Search: `"OrgName" CEO OR founder OR director`
- Search: `"OrgName" site:linkedin.com` (employees)
- Search: `"OrgName" team OR about page`
- Pivot on: Key personnel names (run Person Recon)

**Financial & Legal:**
- Search: `"OrgName" site:sec.gov` (public company filings)
- Search: `"OrgName" funding OR investment site:crunchbase.com OR site:pitchbook.com`
- Search: `"OrgName" lawsuit OR litigation OR court`
- Search: `"OrgName" site:courtlistener.com`

**Digital Assets:**
- Identify primary domain → run Domain Recon
- Search: `"OrgName" site:github.com` (open source presence)
- Search: `"OrgName" app site:play.google.com OR site:apps.apple.com`

**Reputation & News:**
- Search: `"OrgName" news` (recent coverage)
- Search: `"OrgName" site:glassdoor.com` (employee sentiment)
- Search: `"OrgName" site:bbb.org` (BBB profile)
- Search: `"OrgName" review OR complaint OR scam`

---

## 7. Phone Number Recon

**Goal:** Identify the owner and associated accounts for a phone number.

### Search Sequence

**Direct Search:**
- Search: `"phone_number"` (with country code, various formats: +1-555-123-4567, 5551234567, (555) 123-4567)
- Look for: Business listings, social media profiles, public records, ads

**Carrier & Type:**
- Search: `"phone_number" carrier lookup`
- Look for: Is it a landline, mobile, or VoIP number?

**Associated Profiles:**
- Search: `"phone_number" site:facebook.com`
- Search: `"phone_number" site:whatsapp.com OR site:telegram.org`
- Direct user to: Manual Telegram/WhatsApp lookups (add number to contacts to see profile)

**Spam/Scam Check:**
- Search: `"phone_number" spam OR scam OR robocall`
- Search: `"phone_number" site:whocalled.us OR site:800notes.com`
