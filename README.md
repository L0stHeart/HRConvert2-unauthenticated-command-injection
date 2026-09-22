# HRConvert2: unauthenticated command injection in image conversion

https://github.com/zelon88/HRConvert2/security/advisories/GHSA-wg57-wvw2-9cj8

Affects HRConvert2 3.5 and earlier. The advisory names no fixed release. No CVE assigned.

Critical. CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H (9.8).
CWE-78, CWE-116, CWE-838.

No login exists in this application. Image conversion strips shell metacharacters and then encodes the string. Encoding runs second, so metacharacters are back in the value by the time it is placed in a shell command.

CVE-2026-44666 is the same command sink. That fix added characters to the strip list. The addition is present in 3.5. It does not change the order of the two steps, and 3.5 still has this bug.

The v3.6.6 release note says the core was hardened against command injection, among other classes. The advisory was not updated with a patched version, and I have not retested 3.6.6 for this bug. I am not treating 3.6.6 as the fix.

Reported 2026-07-31 through GitHub private vulnerability reporting. The vendor published the advisory on 2026-08-03.

L0stHeart
