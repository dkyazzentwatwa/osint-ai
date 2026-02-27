# Google Dork Library

A curated collection of advanced search operator patterns for OSINT investigations. Organized by target type and purpose.

---

## Table of Contents
1. Search Operator Cheat Sheet
2. Domain Dorks
3. Person/Identity Dorks
4. Document Discovery Dorks
5. Social Media & Platform Dorks
6. Infrastructure & Technical Dorks
7. Financial & Legal Dorks
8. Leaked Data & Exposure Dorks

---

## 1. Search Operator Cheat Sheet

| Operator | Purpose | Example |
|----------|---------|---------|
| `"exact phrase"` | Match exact text | `"john doe"` |
| `site:` | Limit to one domain | `site:github.com` |
| `filetype:` or `ext:` | File type filter | `filetype:pdf` |
| `inurl:` | Text must appear in URL | `inurl:admin` |
| `intitle:` | Text must appear in page title | `intitle:"index of"` |
| `intext:` | Text must appear in body | `intext:password` |
| `OR` | Either term | `site:twitter.com OR site:x.com` |
| `-` | Exclude | `-site:pinterest.com` |
| `*` | Wildcard | `"john * doe"` |
| `before:` / `after:` | Date range (Google) | `after:2024-01-01` |
| `cache:` | Cached version | `cache:example.com` |
| `related:` | Similar sites | `related:example.com` |

**Combining operators** is where the power lies. Chain 2–4 operators per query for precision.

---

## 2. Domain Dorks

### Exposed Files & Directories
```
site:TARGET filetype:pdf
site:TARGET filetype:doc OR filetype:docx
site:TARGET filetype:xls OR filetype:xlsx
site:TARGET filetype:ppt OR filetype:pptx
site:TARGET filetype:csv
site:TARGET filetype:sql
site:TARGET filetype:xml
site:TARGET filetype:json
site:TARGET filetype:log
site:TARGET filetype:env
site:TARGET filetype:bak OR filetype:old
site:TARGET intitle:"index of /"
site:TARGET intitle:"index of" "parent directory"
```

### Admin & Login Panels
```
site:TARGET inurl:admin
site:TARGET inurl:login
site:TARGET inurl:signin
site:TARGET inurl:dashboard
site:TARGET inurl:cpanel
site:TARGET inurl:wp-admin
site:TARGET inurl:administrator
site:TARGET intitle:"admin" OR intitle:"login" OR intitle:"sign in"
```

### API Endpoints & Dev Artifacts
```
site:TARGET inurl:api
site:TARGET inurl:v1 OR inurl:v2 OR inurl:v3
site:TARGET inurl:graphql
site:TARGET inurl:swagger OR inurl:api-docs
site:TARGET filetype:yaml OR filetype:yml
site:TARGET inurl:config OR inurl:settings
site:TARGET inurl:debug OR inurl:test OR inurl:staging
site:TARGET inurl:.git
```

### Third-Party References
```
"TARGET" -site:TARGET
"TARGET" site:github.com
"TARGET" site:pastebin.com
"TARGET" site:trello.com
"TARGET" site:notion.so
"TARGET" site:stackoverflow.com
"TARGET" site:reddit.com
```

---

## 3. Person / Identity Dorks

### Core Identity
```
"Full Name"
"Full Name" city_or_state
"Full Name" employer_or_school
"Full Name" resume OR CV filetype:pdf
"Full Name" bio OR about
"Full Name" "phone" OR "email" OR "contact"
```

### Professional
```
"Full Name" site:linkedin.com
"Full Name" site:crunchbase.com
"Full Name" site:bloomberg.com
"Full Name" speaker OR keynote OR panel
"Full Name" author site:medium.com OR site:substack.com
"Full Name" patent site:patents.google.com
"Full Name" "co-founder" OR "CEO" OR "CTO"
```

### Academic
```
"Full Name" site:scholar.google.com
"Full Name" site:researchgate.net
"Full Name" site:academia.edu
"Full Name" thesis OR dissertation filetype:pdf
"Full Name" ORCID
```

### Social
```
"Full Name" site:twitter.com OR site:x.com
"Full Name" site:facebook.com
"Full Name" site:instagram.com
"Full Name" site:youtube.com
"Full Name" site:reddit.com
"Full Name" site:tiktok.com
```

---

## 4. Document Discovery Dorks

### Sensitive Documents
```
site:TARGET "confidential" filetype:pdf
site:TARGET "internal use only" filetype:pdf
site:TARGET "not for distribution"
site:TARGET "draft" filetype:doc OR filetype:docx
site:TARGET "password" filetype:xls OR filetype:csv
```

### Financial Documents
```
site:TARGET "budget" OR "revenue" OR "forecast" filetype:xls
site:TARGET "invoice" filetype:pdf
site:TARGET "financial" filetype:pdf
"TARGET" annual report filetype:pdf
```

### Meeting Notes & Plans
```
site:TARGET "meeting notes" OR "minutes"
site:TARGET "roadmap" OR "strategy"
site:TARGET "project plan" filetype:pdf OR filetype:ppt
```

---

## 5. Social Media & Platform Dorks

### Cross-Platform Username Search
```
"username" site:twitter.com OR site:x.com
"username" site:reddit.com
"username" site:github.com
"username" site:instagram.com
"username" site:youtube.com
"username" site:medium.com
"username" site:keybase.io
"username" site:hackernews.com
"username" site:mastodon.social
"username" site:linkedin.com
"username" site:pinterest.com
"username" site:tumblr.com
"username" site:soundcloud.com
"username" site:behance.net
"username" site:dribbble.com
"username" site:about.me
```

### Email-Based Discovery
```
"email@domain.com"
"email@domain.com" site:github.com
"email@domain.com" site:gravatar.com
"email@domain.com" site:keybase.io
"email@domain.com" site:groups.google.com
"email@domain.com" forum OR community OR list
```

---

## 6. Infrastructure & Technical Dorks

### Server & Configuration Exposure
```
site:TARGET inurl:server-status
site:TARGET inurl:server-info
site:TARGET intitle:"Apache Status"
site:TARGET "phpinfo()" OR intitle:"phpinfo"
site:TARGET inurl:.env OR inurl:config.php
site:TARGET inurl:web.config
site:TARGET intitle:"Configuration File"
```

### Code Repository Leaks
```
"TARGET" site:github.com password OR secret OR key
"TARGET" site:github.com filename:.env
"TARGET" site:github.com filename:config
"TARGET" site:gitlab.com
"TARGET" site:bitbucket.org
```

### Certificate Transparency
```
"TARGET" site:crt.sh
"TARGET" certificate transparency
```

---

## 7. Financial & Legal Dorks

### Corporate Filings
```
"OrgName" site:sec.gov
"OrgName" site:opencorporates.com
"OrgName" site:companieshouse.gov.uk
"OrgName" 10-K OR 10-Q OR 8-K filetype:pdf
"OrgName" annual report filetype:pdf
```

### Legal
```
"Subject" site:courtlistener.com
"Subject" site:unicourt.com
"Subject" lawsuit OR litigation OR plaintiff OR defendant
"Subject" site:law.justia.com
"Subject" "court" filetype:pdf
```

---

## 8. Leaked Data & Exposure Dorks

### Paste Sites
```
"TARGET" site:pastebin.com
"TARGET" site:paste.org
"TARGET" site:ghostbin.com
"TARGET" site:dpaste.org
"TARGET" site:hastebin.com
```

### Exposed Databases & Directories
```
intitle:"index of" "TARGET"
"TARGET" inurl:backup
"TARGET" ext:sql OR ext:db OR ext:sqlite
"TARGET" "database dump"
```

### Misconfigured Cloud Storage
```
site:s3.amazonaws.com "TARGET"
site:blob.core.windows.net "TARGET"
site:storage.googleapis.com "TARGET"
"TARGET" inurl:s3 bucket
```

---

## Usage Tips

1. **Replace TARGET** with the actual domain, name, username, or email
2. **Don't run all dorks blindly** — select the ones relevant to your investigation
3. **Execute the top 3–5 most promising** via web_search and analyze results
4. **Log negative results** — knowing what *wasn't* found is valuable
5. **Combine with temporal filters** — `after:2024-01-01` narrows to recent content
6. **Adapt for Bing/DuckDuckGo** — most operators work but syntax varies slightly (e.g., Bing uses `contains:` instead of `filetype:` in some cases)
