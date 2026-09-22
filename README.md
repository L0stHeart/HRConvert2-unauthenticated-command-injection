# HRConvert2 image conversion runs shell commands without a login

https://github.com/zelon88/HRConvert2/security/advisories/GHSA-wg57-wvw2-9cj8

Severity: critical (CVSS 9.8, `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H`). CWE-78, CWE-116, CWE-838. No CVE. The advisory names no fixed release.

HRConvert2 3.5 and earlier is affected. The application ships with no authentication. I reviewed 3.5, commit f27083b.

Image conversion strips shell metacharacters and then encodes the string. Encoding runs second, so the metacharacters are back before the value is placed in a shell command. `sanitizeString()` is the function that does the two steps in that order. The sink is in image conversion. No account is required. I confirmed command execution in a local lab, running as the web server user.

This is the same command sink as CVE-2026-44666, and it is not that bug. CVE-2026-44666 was fixed by adding characters to the strip list. That change is present in 3.5. I checked. It does not change the order of the two steps, so 3.5 is still affected.

The 3.6.6 release note, 14 August 2026, says the core was hardened against command injection, among other classes. The advisory was not updated with a patched version. I have not retested 3.6.6, or anything after it. Later tags exist, through 3.9.1 as of 6 September 2026. I am not calling any of them the fix.

On 7 August 2026 I asked the maintainer to request CVE ids for the published HRConvert2 advisories. A reporter cannot press that button. The maintainer closed the request the same day. As of 22 September 2026 this advisory still has no CVE id.

The lab was a local instance of 3.5 that I built. No one else's deployment.

Reported privately on 31 July 2026. The vendor published the advisory on 3 August 2026.
