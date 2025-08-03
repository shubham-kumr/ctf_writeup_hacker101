## Exploiting Case-Sensitive Security Headers for Capture the Flag

### Description

This document details the solution for the “XSS Playground by zseano - Web (Hacker101 CTF)” challenge, focusing on exploiting Cross-Site Scripting (XSS) vulnerabilities and overcoming API authentication issues caused by header case sensitivity.

### XSS Vulnerabilities Identified

- 5 instances of reflected XSS
- 3 instances of stored XSS
- 2 instances of DOM-based XSS
- 1 instance of CSP bypass via XSS
- 1 scenario involving XSS to leak sensitive data

### Analysis of the Issue

While reviewing the site’s JavaScript files, an API endpoint responsible for email retrieval was discovered in `custom.js`:

```
/api/action.php?act=getemail
```

Access to this endpoint required a custom security header:

```
X-SAFEPROTECTION: enNlYW5vb2Zjb3Vyc2U=
```

Attempts to interact with the endpoint using Burp Suite and cURL were unsuccessful due to header casing issues. HTTP/2 standardizes headers to lowercase, which can interfere with servers that enforce case-sensitive header checks.

### Why Requests Were Blocked

- **Burp Suite:** Automatically changes header casing to Title Case, resulting in `X-Safeprotection` instead of the required `X-SAFEPROTECTION`.
- **cURL:** When using HTTP/2, headers are normalized to lowercase, causing authentication to fail.

### Solution

To resolve this, HTTP/1.1 was enforced to preserve the original header casing. The following cURL command was used:

```bash
curl -H "X-SAFEPROTECTION: enNlYW5vb2Zjb3Vyc2U=" --http1.1 "https://52b94adfadeff85d1d161f93a34909e8.ctf.hacker101.com/api/action.php?act=getemail"
```

### Key Steps

1. **Custom Header:** Ensured the header was sent exactly as required.
2. **HTTP/1.1 Enforcement:** Prevented automatic header normalization.
3. **URL Formatting:** Quoted the URL for proper shell parsing.

### Flag Extraction

The command returned an email address, which was essential for progressing in the challenge. The flag was provided, but without the expected `FLAG$` suffix. Based on hints, the suffix was manually appended:

```
{'email':'zseano@ofcourse.com','flag':'^FLAG^7cf095fe3d1562a95054dedcf6960eb68559619fb3357bdf8d6ddb8b1411e6f4$'}
```

If the flag appears incomplete, append the missing suffix as indicated by the challenge instructions.

### Lessons Learned

- Header case sensitivity can affect authentication, especially when HTTP/2 is involved.
- Tools like Burp Suite may alter header casing, impacting API requests.
- Forcing HTTP/1.1 can resolve issues with header case enforcement.
- Using verbose output (`-v` in cURL) aids in troubleshooting failed requests.

### Additional Debugging Tips

If issues persist:

- Use verbose mode in cURL:
    ```bash
    curl -v -H "X-SAFEPROTECTION: enNlYW5vb2Zjb3Vyc2U=" --http1.1 "URL"
    ```
- Try `--http1.0` if HTTP/1.1 does not resolve the problem.
- Check for other security controls such as user agent or cookie requirements.


### Conclusion

By understanding and circumventing header case sensitivity issues, the flag was successfully retrieved. This scenario demonstrates the importance of recognizing subtle protocol behaviors and their impact on security mechanisms.

---

During the investigation, the email address was found embedded in the source code. Searching for terms like 'email' and 'getemail' within JavaScript files led to the discovery of the relevant function in `custom.js` and the associated API endpoint:

```
/api/action.php?act=getemail
```

The request required the following header:

```
X-SAFEPROTECTION: enNlYW5vb2Zjb3Vyc2U=
```

This step was critical for extracting the email and completing the challenge.

💀 🏴 HAPPY HACKING!