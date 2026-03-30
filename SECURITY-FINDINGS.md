# Security Investigation Findings

**Date:** 2026-03-30  
**Trigger:** Accidental commit of macOS `.DS_Store` metadata file (removed in PR #131)  
**Question:** Is there evidence of suspicious activity?

---

## What Was Found

A macOS `.DS_Store` metadata file was present in the repository across multiple commits
from **2026-02-15** to **2026-03-07**, first introduced in a commit that also removed a
temporary `hello.html` test page. It was removed from tracking and added to `.gitignore`
in PR #131.

---

## Investigation

### 1. `.DS_Store` File Contents

All historical versions of the file were examined. The file contained only:

- Standard macOS binary header identifiers (`Bud1`, `DSDB`)
- Icon-position blob tags for files beginning with certain letters (`fIlocblob`,
  `gIlocblob`, `lIlocblob`, `sIlocblob`, `tIlocblob`)
- Partial filenames of files already publicly visible in this repository (e.g.,
  `sargeant-bay-society-methods.html`)

**No credentials, private file paths, tokens, or other sensitive data were present.**

### 2. HTML/CSS/JS Code Audit

All source files were scanned for:

- Obfuscated or encoded JavaScript (`eval`, `atob`, `base64`, `fromCharCode`)
- Hidden iframes
- Cryptocurrency miners
- Unauthorized external script inclusions
- Data exfiltration patterns (`XMLHttpRequest`, `fetch` to unknown domains)

**None found.** The only JavaScript is `nav.js` (mobile navigation toggle) and a small
inline `togglePanel` function in `index.html`, both benign.

### 3. External URLs

All `href` and `src` attributes in HTML files were reviewed. Every external URL points to
a legitimate organisation:

- BC provincial government (`gov.bc.ca`, `www2.gov.bc.ca`)
- University of British Columbia (`ubc.ca`)
- Community science platforms (`inaturalist.org`, `ebird.org`, `gbif.org`, `naturecounts.ca`)
- Local Sunshine Coast conservation organisations (`loonfoundation.org`,
  `sunshinecoaststreamkeepers.com`, `sargbay.ca`, etc.)

**No suspicious or unknown domains found.**

### 4. Git Commit History

All 454 commits were reviewed. Only two committers appear in the full history:

| Author | Identity |
|---|---|
| `locke.rowe@utoronto.ca` | Repository owner/maintainer |
| `198982749+Copilot@users.noreply.github.com` | GitHub Copilot coding agent |

**No commits from unknown or unauthorized authors.**

### 5. Deleted `hello.html`

The commit that first introduced `.DS_Store` also deleted `hello.html`. That file
contained only a minimal "Hello, World!" HTML page with no sensitive content — a
standard test file used when first setting up a repository.

---

## Conclusion

**No suspicious activity was detected.**

The committed `.DS_Store` file was an accidental, common mistake made by macOS users
when `.DS_Store` is not listed in `.gitignore`. The file exposed no sensitive information
beyond filenames already visible in the public repository. No malicious code,
unauthorised access, or data exfiltration was found anywhere in the repository.

The incident has been fully remediated:
- `.DS_Store` removed from git tracking (`git rm --cached`)
- `.DS_Store` added to `.gitignore` to prevent re-occurrence
