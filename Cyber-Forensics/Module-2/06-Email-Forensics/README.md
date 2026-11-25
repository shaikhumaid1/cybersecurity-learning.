# Practical 06 — Email Forensics

## 1. Aim

To examine a synthetic email in `.eml` format, inspect its headers and body, identify phishing indicators, extract key artefacts (email addresses, URLs and IP addresses), and record the sample's SHA-256 hash for integrity checking.

> **Training-data notice:** This is a completely synthetic training artefact. It was not sent, received, or authenticated by a real mail system. Domains ending in `.example` and IP address `203.0.113.45` are reserved for documentation/examples. The `Authentication-Results` field is illustrative text, not a real SPF, DKIM or DMARC evaluation.

## 2. Requirements

| Tool / item | Purpose |
|---|---|
| Kali Linux | Lab operating system used for command-line examination |
| Synthetic `.eml` sample | Email artefact examined in this practical |
| `grep` | Extract email-like strings, URLs, IP-like strings and selected headers |
| `sed` | Display the header section and message body |
| `nl` | Display the file with line numbers |
| `sha256sum` | Calculate a SHA-256 digest for later integrity comparison |
| `file` | Identify the file type |
| `ls` | Confirm the file exists and inspect its size and permissions |

### Lab file

`practical06_synthetic_phishing_sample.eml`

The sample claims to be a payment-account security notice. It includes a sender, recipient, return path, reply-to address, a Received field, illustrative authentication results, an urgent message body and a verification URL.

## 3. Procedure

### Step 1 — Create the working directory

```bash
mkdir -p ~/CyberForensics/Practical-06
cd ~/CyberForensics/Practical-06
```

- `mkdir` creates a directory.
- `-p` creates missing parent directories and does not report an error if the directory already exists.
- `cd` changes the current working directory.

### Step 2 — Verify the sample

Command:

```bash
ls -lh
file practical06_synthetic_phishing_sample.eml
```

Observed output:

```text
total 4.0K
-rw-r--r-- 1 umaid umaid 1.6K Nov 25 22:42 practical06_synthetic_phishing_sample.eml
practical06_synthetic_phishing_sample.eml: news or mail, ASCII text
```

Interpretation:
- The file was present in the working directory.
- Reported size was 1.6 KB.
- The file command identified it as mail/news-style ASCII text.
- The permissions shown were `-rw-r--r--`: owner read/write, group read, others read.

### Step 3 — Extract the email headers

Command:

```bash
sed -n '1,/^$/p' practical06_synthetic_phishing_sample.eml
```

Explanation:
- `sed` processes text line by line.
- `-n` suppresses automatic printing.
- `1,/^$/p` prints from line 1 through the first blank line. In an email, this is the header block; the blank line separates headers from the body.

Observed header values:

| Header | Observed value | Forensic relevance |
|---|---|---|
| From | `"PayPal Account Security" <billing@secure-payments.example>` | Claimed sender identity and display name |
| To | `analyst@lab.example` | Recipient |
| Subject | `[ACTION REQUIRED] Your account will be limited` | Urgent subject wording |
| Message-ID | `<20261125164512.78421@mail.secure-payments.example>` | Message identifier; not proof of sender identity |
| Return-Path | `bounce@mailer-notify.example` | Envelope return address shown in sample |
| Reply-To | `account-review@protonmail.example` | Address to which replies are directed |
| Received | From `mail.secure-payments.example`, IP `203.0.113.45`, by `mx.lab.example` (Postfix) | Claimed transfer information |
| Authentication-Results | SPF fail, DKIM fail, DMARC fail | Illustrative results included in the sample |

**Analysis:** The claimed sender, return path and reply-to address use different domains. This inconsistency is a phishing indicator worth investigating. It is not conclusive proof on its own. The Received field is also part of a synthetic sample; in real investigations, trust must be placed in headers added by known, trusted receiving infrastructure because earlier Received fields can be forged.

### Step 4 — Inspect the email body

Command:

```bash
sed -n '/^$/,$p' practical06_synthetic_phishing_sample.eml
```

Explanation:
- The pattern `^$` matches a blank line.
- `,$p` prints from that blank line through the end of the file.
- This displays the body and, in this sample, the trailing lab notes.

Observed body content includes:
- A claim of unusual account activity.
- A warning that the account will be restricted within 24 hours.
- A request to verify account details.
- A link to `https://account-check.secure-payments.example/verify`.
- A request to reply to the message.

**Analysis:** The account restriction deadline and request to verify details are urgency and credential-harvesting themes commonly seen in phishing. The URL hostname does not establish an association with the claimed brand. HTTPS alone would not prove that a site is legitimate. The `.example` hostname is reserved for examples and is not a live destination.

### Step 5 — Extract email-address-like strings

Command:

```bash
grep -Eoi '[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,}' practical06_synthetic_phishing_sample.eml
```

Explanation:
- `grep` searches the file for matching text.
- `-E` enables extended regular expressions.
- `-o` prints only the matched portion.
- `-i` makes matching case-insensitive.
- The expression matches strings resembling email addresses; it does not validate whether an address exists.

Observed output:

```text
billing@secure-payments.example
analyst@lab.example
20261125164512.78421@mail.secure-payments.example
bounce@mailer-notify.example
account-review@protonmail.example
analyst@lab.example
```

Interpretation:
- The recipient appears twice because it is present in more than one part of the email.
- The Message-ID was also matched because it contains an `@` and a domain-like string. It is not a normal sender or recipient mailbox.
- The output is a list of regex matches, not a deduplicated or semantically classified address list.

### Step 6 — Extract URLs

Command:

```bash
grep -Eo 'https?://[^[:space:]]+' practical06_synthetic_phishing_sample.eml
```

Explanation:
- `-E` enables extended regular expressions.
- `-o` prints only the matched URL.
- `https?` matches either `http` or `https`.
- `[^[:space:]]+` matches non-whitespace characters until a space or line break.

Observed output:

```text
https://account-check.secure-payments.example/verify
```

Interpretation: One URL-like string was extracted. It was not opened. The domain is a reserved example domain, so this sample provides no live website or hosting evidence.

### Step 7 — Extract IPv4-looking strings

Command:

```bash
grep -Eo '([0-9]{1,3}\\.){3}[0-9]{1,3}' practical06_synthetic_phishing_sample.eml
```

Explanation:
- `[0-9]{1,3}` matches one to three digits.
- `\\.` matches a literal dot.
- The grouped expression repeats three times, followed by the final numeric octet.
- This is a simple pattern match; it does not validate octet ranges or determine whether an address is routable.

Observed output:

```text
203.0.113.45
203.0.113.45
203.0.113.45
```

Interpretation: The same IP-like string appears three times in the file: in the Received header, the illustrative SPF text and the lab notes. It is one unique address repeated three times. `203.0.113.45` belongs to the TEST-NET-3 documentation range and is not a real sender IP for attribution.

### Step 8 — Verify selected header line numbers

Command:

```bash
grep -nE '^(From:|To:|Return-Path:|Reply-To:|Received:|Authentication-Results:)' practical06_synthetic_phishing_sample.eml
```

Explanation:
- `-n` prints the matching line number.
- `-E` enables extended regular expressions.
- `^` anchors each match at the beginning of a line.
- The expression selects the specified header names.

Observed output:

```text
1:From: "PayPal Account Security" <billing@secure-payments.example>
2:To: Digital Forensics Lab <analyst@lab.example>
6:Return-Path: <bounce@mailer-notify.example>
7:Reply-To: <account-review@protonmail.example>
11:Received: from mail.secure-payments.example (unknown [203.0.113.45])
14:Authentication-Results: mx.lab.example;
```

The SPF, DKIM and DMARC values continue on folded header lines 15–17. They are continuations of the Authentication-Results header, not separate headers.

### Step 9 — Display the complete sample with line numbers

Command:

```bash
nl -ba practical06_synthetic_phishing_sample.eml
```

Explanation:
- `nl` numbers lines.
- `-b a` numbers all lines, including blank lines.
- This made it possible to reference specific header and body evidence by line number.

Observed file length: 37 numbered lines.

Key locations:
- Lines 1–17: email headers.
- Line 18: blank separator.
- Lines 19–30: message body.
- Lines 32–37: synthetic lab notes.

### Step 10 — Calculate the SHA-256 digest

Command:

```bash
sha256sum practical06_synthetic_phishing_sample.eml
```

Observed output:

```text
fa8f3d8ddd75c0e3f48bd92c0d1dcd91e1b553847f38c1b8ef2fe06c919fafef  practical06_synthetic_phishing_sample.eml
```

Explanation:
- `sha256sum` calculates the SHA-256 digest of the file.
- The digest can be recalculated later and compared with this baseline to detect whether the file's contents have changed.
- A matching hash supports file-integrity comparison; it does not prove the file's origin, authenticity or chain of custody by itself.

## 4. Result

The synthetic email was examined successfully. Its headers and body were inspected, and email-address-like strings, a URL and an IP-like string were extracted using Linux command-line tools. The sample contains several phishing indicators: inconsistent sender and reply-routing domains, urgency, a request to verify account details, and an example verification URL.

The sample's SHA-256 digest was recorded as:

`fa8f3d8ddd75c0e3f48bd92c0d1dcd91e1b553847f38c1b8ef2fe06c919fafef`

**Conclusion:** The synthetic message demonstrates a phishing-style email and the basic workflow for email-header and content examination. It is a training artefact only. Its domains, IP address and authentication results are illustrative; no real sender attribution, live URL analysis, or independent SPF/DKIM/DMARC validation was performed.

## Evidence and limitations

- The file was identified as ASCII text, 1.6 KB.
- The complete numbered output contained 37 lines.
- The IP extraction produced three occurrences of the same documentation IP.
- The email-address regular expression also matched the Message-ID and repeated recipient; regex output therefore requires manual classification.
- The authentication results were embedded in the sample and were not independently verified.
- No network connection was made to the URL.
- No real mailbox, mail server, sender or recipient was investigated.
- The supplied command outputs are the evidence recorded for this practical; screenshots may be added separately if required by the lab instructor.


## Appendix A — Original synthetic email sample

The following is the complete training email examined in this practical. It is included so the README is self-contained and readers can reproduce the analysis. Save the block exactly as `practical06_synthetic_phishing_sample.eml` if recreating the exercise.

```text
From: "PayPal Account Security" <billing@secure-payments.example>
To: Digital Forensics Lab <analyst@lab.example>
Date: Wed, 25 Nov 2026 16:45:12 +0000
Subject: [ACTION REQUIRED] Your account will be limited
Message-ID: <20261125164512.78421@mail.secure-payments.example>
Return-Path: <bounce@mailer-notify.example>
Reply-To: <account-review@protonmail.example>
MIME-Version: 1.0
Content-Type: text/plain; charset="UTF-8"
X-Mailer: Synthetic-Mailer/1.0
Received: from mail.secure-payments.example (unknown [203.0.113.45])
        by mx.lab.example (Postfix) with ESMTP id LAB123456
        for <analyst@lab.example>; Wed, 25 Nov 2026 16:45:14 +0000
Authentication-Results: mx.lab.example;
        spf=fail (sender IP is 203.0.113.45) smtp.mailfrom=mailer-notify.example;
        dkim=fail (bad signature) header.d=secure-payments.example;
        dmarc=fail (p=reject) header.from=secure-payments.example

Hello,

We detected unusual activity on your payment account. For your protection,
your account will be restricted within 24 hours unless you verify your details.

Review your account now:
https://account-check.secure-payments.example/verify

If you did not request this review, reply directly to this message.

Regards,
PayPal Account Security Team

--- LAB NOTES ---
This is a completely synthetic training artifact. It was not sent, received,
or authenticated by any real mail system. All domains ending in .example and
the IP address 203.0.113.45 are reserved for documentation/examples.
Authentication-Results is illustrative text inserted for the exercise; it is
not a real SPF, DKIM, or DMARC evaluation.
```

**Reproduction note:** The synthetic sample's SHA-256 digest recorded during the practical was `fa8f3d8ddd75c0e3f48bd92c0d1dcd91e1b553847f38c1b8ef2fe06c919fafef`. Recalculate the digest after saving the sample to confirm that the copied file matches the examined artefact.
