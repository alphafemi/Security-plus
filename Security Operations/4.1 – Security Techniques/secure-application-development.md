# Secure Application Development

## Overview

Application vulnerabilities — buffer overflows, SQL injection, insecure authentication — are often introduced during development and discovered later by researchers or attackers. Building security into the development process, rather than retrofitting it afterward, reduces both the number of vulnerabilities and the cost of fixing them.

---

## Input Validation

**Input validation** ensures that data entering an application conforms to what is expected. Every input field is a potential attack vector — unvalidated input can be exploited for SQL injection, buffer overflows, cross-site scripting, and other attacks.

### What to Validate

| Input Type | Validation Rules |
|-----------|-----------------|
| **Zip code (US)** | 5 or 9 digits; numeric only |
| **Email address** | Must contain `@` and a valid domain format |
| **Date fields** | Must be a valid date in the expected format |
| **Numeric fields** | Must be within expected range; no letters or symbols |
| **Free text fields** | Strip or reject HTML tags and SQL syntax |

The application should reject or sanitize any input that does not conform — and prompt the user to correct it rather than attempting to process malformed input.

---

## Fuzzing

**Fuzzing** (fuzz testing) is an automated technique for testing input validation by feeding random, unexpected, or malformed data into an application's input fields.

```
[Fuzzer generates random/malformed inputs]
          |
          ▼
[Application receives unexpected input]
          |
          ├── Application handles it correctly → test passes
          |
          └── Application behaves unexpectedly → vulnerability identified
                    |
                    ▼
              [Developer reviews and fixes input validation]
```

Fuzzing catches edge cases that developers may not think to test manually — including inputs that trigger crashes, buffer overflows, or unexpected behavior.

---

## Cookies

**Cookies** are small data files stored in the browser, used for session management, personalization, and tracking. They are not executable and contain no malware — but the data within them can be valuable to attackers.

### Cookie Security Attributes

| Attribute | Effect |
|-----------|--------|
| **Secure** | Cookie is only transmitted over HTTPS (encrypted) connections — never over plain HTTP |
| **HttpOnly** | Cookie is inaccessible to JavaScript — protects against XSS-based cookie theft |
| **SameSite** | Restricts when the cookie is sent with cross-site requests — helps prevent CSRF |

### What NOT to Store in Cookies

Because cookies can be read from the browser, sensitive information — passwords, full account numbers, or private personal data — should never be stored in a cookie. Session tokens are acceptable because they are meaningless without access to the server-side session store.

---

## Static Code Analysis (SAST)

**Static Application Security Testing (SAST)** analyzes application source code for security vulnerabilities without executing the code.

### What SAST Can Find

- Buffer overflows (e.g., use of unsafe functions like `gets()` instead of `fgets()`)
- SQL injection vulnerabilities
- Hardcoded credentials
- Insecure cryptography usage
- Null pointer dereferences

### SAST Output Example

```
filetest.c:32: error: gets() does not check buffer length — buffer overflow possible
  Recommendation: use fgets() instead
```

The analyzer flags the specific file, line number, and issue — giving developers an actionable remediation target.

### Limitations of SAST

| Limitation | Description |
|-----------|-------------|
| **Cannot detect runtime issues** | Cryptographic implementation errors may only be visible at execution time |
| **False positives** | Developer must review output — not all flagged issues are real vulnerabilities |
| **Logic flaws** | SAST cannot identify flaws in application business logic |

SAST is a valuable first pass — it catches obvious issues automatically — but it is not a complete security solution.

---

## Code Signing

**Code signing** allows users to verify that software comes from a legitimate developer and has not been modified since it was released.

### How Code Signing Works

```
[Developer signs code with private key (via certificate from a CA)]
          |
          ▼
[Signed application distributed to users]
          |
          ▼
[User installs application]
          |
          ▼
[OS validates the digital signature]
          |
          ├── Signature valid → installation proceeds normally
          |
          └── Signature invalid or modified → OS warns user
```

### What Code Signing Answers

| Question | How Code Signing Answers It |
|----------|---------------------------|
| Is this the original code? | The hash embedded in the signature covers the entire application — any modification invalidates it |
| Does this really come from the developer? | The certificate, issued by a trusted CA, ties the signature to the developer's identity |

---

## Sandboxing

**Sandboxing** restricts what an application can access — limiting the potential damage if the application is compromised or malicious.

### Sandboxing Contexts

| Context | Description |
|---------|-------------|
| **Development sandbox** | Isolated environment where developers write and test code without affecting production systems |
| **Virtual machine** | Each VM is isolated from others on the same hypervisor — compromise of one doesn't spread automatically |
| **Mobile OS sandbox** | Each app on a mobile device can only access its own data by default — must request permissions for anything else |
| **Browser sandbox** | Browser processes are isolated from each other and from the OS |

### Mobile Sandbox Example

A mobile browser can access stored bookmarks — but cannot access the camera roll, contacts, or other apps' data without explicit permission. If an attacker exploits the browser, their access is limited to the browser's sandbox.

---

## Application Monitoring

Developers build **monitoring** into applications to detect unusual behavior, log security events, and identify potential vulnerabilities or active attacks.

### What to Monitor

| Event Type | Significance |
|-----------|-------------|
| **SQL injection attempts** | May indicate active exploitation or reconnaissance |
| **Failed authentication** | High failure rates suggest brute force or credential stuffing |
| **Unusual file access patterns** | Unexpected reads of sensitive files |
| **Abnormal data transfer volumes** | Could indicate data exfiltration |
| **Access from unexpected locations** | May indicate compromised credentials |

Application logs feed into security monitoring tools (SIEM) where they can be correlated with events from other systems to identify sophisticated, multi-stage attacks.

---

## Secure Development Summary

| Practice | What It Addresses |
|----------|------------------|
| **Input validation** | Prevents injection attacks, buffer overflows from malformed input |
| **Fuzzing** | Automatically discovers unhandled edge cases in input processing |
| **Secure cookies** | Protects session tokens from interception and XSS theft |
| **SAST** | Finds code-level vulnerabilities before deployment |
| **Code signing** | Ensures distributed software is authentic and unmodified |
| **Sandboxing** | Limits blast radius if an application is compromised |
| **Application monitoring** | Detects attacks and unexpected behavior in real time |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Input validation** | Every input field is a potential attack vector — validate all user input against expected formats |
| **Fuzzing** | Automated testing with unexpected inputs reveals validation failures that manual testing misses |
| **Secure cookie attributes** | `Secure`, `HttpOnly`, and `SameSite` protect cookies from interception and script theft |
| **SAST** | Static code analysis finds common vulnerabilities in source code — but has limitations and produces false positives |
| **Code signing** | Digital signature proves authenticity and integrity of distributed software |
| **Sandboxing** | Isolates applications from each other and from sensitive system resources |
| **Application monitoring** | Runtime detection of attacks, anomalies, and potential undiscovered vulnerabilities |
