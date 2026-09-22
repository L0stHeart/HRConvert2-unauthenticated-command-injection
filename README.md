# HRConvert2: unauthenticated command injection in image conversion

https://github.com/zelon88/HRConvert2/security/advisories/GHSA-wg57-wvw2-9cj8

HRConvert2 is a self-hosted PHP file conversion and sharing server (zelon88/HRConvert2). I reviewed v3.5, commit `f27083b`. The application ships with no authentication. I reported this on 2026-07-31 through GitHub private vulnerability reporting. The vendor published the advisory on 2026-08-03.

There is no fixed version on the advisory, and no CVE.

Image conversion strips shell metacharacters and then encodes the string. Encoding runs second, so the metacharacters are back before the value is placed in a shell command. The sink is in image conversion. `sanitizeString()` is the function that does the two steps in that order. No account is required.

This is the same command sink as CVE-2026-44666, and it is not that bug. CVE-2026-44666 was fixed by adding characters to the strip list. That change is present in v3.5. I checked. It does not change the order of strip and encode, so v3.5 is still exploitable. I verified command execution in a local lab, as the web server user.

| | |
|---|---|
| Affected | 3.5 and earlier |
| Fixed | Not stated on the advisory |
| Severity | Critical, 9.8 |
| Vector | CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H |
| CWE | CWE-78, CWE-116, CWE-838 |
| Privileges | None |
| CVE | Not assigned |

The v3.6.6 release note, 2026-08-14, says the core was hardened against command injection, SSRF, and arbitrary file read. The advisory was not updated with a patched version. I have not retested 3.6.6, or anything after it, against this bug. I am not calling 3.6.6 the fix. Later tags exist, through v3.9.1 as of 2026-09-06. Same caveat.

On 2026-08-07 I asked the maintainer to request CVE ids for the published advisories. GitHub is a CNA, and the reporter cannot press that button. The maintainer closed the request the same day. As of 2026-09-22 this advisory still has no CVE id.

The lab was a local instance of v3.5 that I built. No one else's deployment.

L0stHeart
https://github.com/L0stHeart
