# Practical 05: Password Cracking Techniques Using John the Ripper and Hashcat

## 1. Aim

To understand password hashing and perform a controlled dictionary attack against a self-generated SHA-256 test hash using **John the Ripper** and **Hashcat**, then verify and compare the recovered result.

> **Lab scope:** This experiment uses only a synthetic password and a hash generated locally for this practical. Do not use these commands against accounts, devices, or password hashes without explicit authorization.

## 2. Requirements

### Hardware and operating environment

- Operating system: Kali Linux
- Host environment: Windows 11 laptop running Kali Linux
- Processor: AMD Ryzen 5 7520U with Radeon Graphics
- Detected Hashcat compute device: CPU through the PoCL OpenCL runtime
- CPU compute units reported by Hashcat: 8

### Tools used

| Tool | Version / configuration | Purpose |
|---|---|---|
| John the Ripper | 1.9.0-jumbo-1+bleeding-aec1328d6c (2021-11-02) | Password-hash auditing and dictionary attack |
| Hashcat | 7.1.2 | Password-hash recovery using a selected hash mode and candidate list |
| sha256sum | GNU coreutils utility | Generate a SHA-256 digest from test text |
| awk | Command-line text processor | Extract the hash field from sha256sum output |
| Terminal | Kali Linux shell | Execute and record the practical |

### Important concepts

- **Hashing:** A one-way transformation that produces a fixed-length digest. SHA-256 produces a 256-bit (64 hexadecimal-character) digest.
- **Encryption:** A reversible process when the correct decryption key is available. A password hash is not encrypted plaintext.
- **Dictionary attack:** Tests candidate passwords from a wordlist. Each candidate is hashed using the selected algorithm and compared with the target digest.
- **Salt:** A unique random value added to a password before hashing. The test uses an unsalted raw SHA-256 digest, which is suitable for demonstrating the mechanics but not representative of secure password storage.
- **Password hashing for storage:** Real applications should use a dedicated password-hashing scheme such as Argon2id, scrypt, or bcrypt with unique salts and appropriately configured work factors, rather than fast general-purpose SHA-256.

## 3. Procedure

### Step 1: Create a working directory

Command:

```bash
mkdir -p ~/CyberForensics/Practical-05
cd ~/CyberForensics/Practical-05
```

**Command explanation:**

- `mkdir`: Creates a directory.
- `-p`: Creates missing parent directories and does not report an error if the directory already exists.
- `~`: Represents the current user's home directory.
- `cd`: Changes the current working directory.
- `~/CyberForensics/Practical-05`: Keeps the practical's generated files together.

The working directory was created and entered successfully.

### Step 2: Check tool versions

Commands:

```bash
john --version
hashcat --version
```

**Command explanation:**

- `john --version`: Displays the installed John the Ripper build/version.
- `hashcat --version`: Displays the installed Hashcat version.

**Observed versions:**

- John the Ripper: `1.9.0-jumbo-1+bleeding-aec1328d6c 2021-11-02`
- Hashcat: `v7.1.2`

### Step 3: Inspect supported hash formats and compute devices

Commands:

```bash
john --list=formats
hashcat -I
```

**Command explanation:**

- `john --list=formats`: Lists hash formats supported by the installed John build. The observed output reported **416 formats** (with 149 dynamic formats represented as `dynamic_n`).
- `hashcat -I`: Displays detected OpenCL compute platforms and devices. The uppercase `-I` requests device/platform information; it does not start a cracking attack.

**Observed Hashcat environment:**

- OpenCL platform: PoCL (Portable Computing Language), project platform.
- OpenCL version: 3.0 PoCL.
- Device: `cpu-haswell-AMD Ryzen 5 7520U with Radeon Graphics`.
- Device type: CPU.
- Compute units/processors reported: 8.
- Total device memory reported: 3463 MB; maximum allocatable block: 1731 MB.

**Interpretation:** Hashcat detected a CPU OpenCL device through PoCL. The output did not show a separately detected GPU device, so this experiment used CPU-based OpenCL processing. The integrated Radeon graphics name in the CPU device string is not evidence that Hashcat used the GPU.

### Step 4: Generate a test SHA-256 hash

Test string used: `CyberLab@123`

Command:

```bash
echo -n 'CyberLab@123' | sha256sum
```

**Command explanation:**

- `echo`: Writes the supplied text to standard output.
- `-n`: Prevents `echo` from appending a newline. This matters because a newline would change the input and therefore produce a different digest.
- `'CyberLab@123'`: The synthetic test password. Single quotes preserve the literal characters.
- `|`: Sends the output of `echo` to the next command through a pipe.
- `sha256sum`: Calculates the SHA-256 digest of the bytes it receives.

**Observed output:**

```text
c5d042bffb1e249604cc3ee528e173b371be5dd0fdc3e598b19432178321cdd8  -
```

The hyphen indicates that `sha256sum` read the input from standard input rather than from a named file.

### Step 5: Save only the digest into hash.txt

Command:

```bash
echo -n 'CyberLab@123' | sha256sum | awk '{print $1}' > hash.txt
cat hash.txt
```

**Command explanation:**

- The first two commands generate the SHA-256 output as in Step 4.
- The second pipe sends the full `sha256sum` output to `awk`.
- `awk '{print $1}'`: Prints the first whitespace-separated field, which is the digest. The filename marker (`-`) is not included.
- `>`: Redirects output into `hash.txt`. It creates the file or overwrites its previous contents.
- `cat hash.txt`: Displays the saved digest so the file contents can be checked.

**Observed saved digest:**

```text
c5d042bffb1e249604cc3ee528e173b371be5dd0fdc3e598b19432178321cdd8
```

### Step 6: Create a small dictionary (wordlist)

Commands:

```bash
echo 'Password123' > wordlist.txt
echo 'CyberLab@123' >> wordlist.txt
echo 'Welcome123' >> wordlist.txt
```

**Command explanation:**

- The first command creates `wordlist.txt` and writes the first candidate.
- `>` creates or replaces the file.
- The next two commands use `>>`, which appends candidates without replacing existing content.
- Each line is one candidate password for the dictionary attack.

The wordlist contains exactly three candidates:

```text
Password123
CyberLab@123
Welcome123
```

### Step 7: Run a dictionary attack with John the Ripper

Command:

```bash
john --format=Raw-SHA256 --wordlist=wordlist.txt hash.txt
```

**Command explanation:**

- `john`: Starts John the Ripper.
- `--format=Raw-SHA256`: Tells John to interpret the target as a raw, unsalted SHA-256 digest. Explicitly specifying the format avoids relying on format auto-detection.
- `--wordlist=wordlist.txt`: Supplies the file containing candidate passwords.
- `hash.txt`: Supplies the target digest to test.

**Important observed output:**

```text
Loaded 1 password hash (Raw-SHA256 [SHA256 256/256 AVX2 8x])
CyberLab@123     (?)
1g ... DONE
Session completed.
```

**Output interpretation:**

- `Loaded 1 password hash`: John parsed one target hash.
- `Raw-SHA256`: John used the requested raw SHA-256 format.
- `CyberLab@123`: The matching candidate recovered from the wordlist.
- `1g`: One hash was successfully cracked (the `g` indicates a cracked guess).
- `DONE`: The available candidate work completed.

John also displayed warnings about OpenMP scalability and the small candidate list. These are performance-related warnings, not evidence of failure. The list had only three candidates, too few to keep all available threads efficiently occupied.

### Step 8: Display the recovered password with John

Command:

```bash
john --show --format=Raw-SHA256 hash.txt
```

**Command explanation:**

- `--show`: Displays passwords John has already recovered and saved in its pot file.
- `--format=Raw-SHA256`: Selects the same hash format used for the attack.
- `hash.txt`: Identifies the target hash file.

**Observed output:**

```text
?:CyberLab@123

1 password hash cracked, 0 left
```

The question mark is the placeholder for the source label because the hash file contains a bare digest without a username or filename field. John recovered one password.

When the same cracking command was run again, John reported:

```text
No password hashes left to crack (see FAQ)
```

This was expected because John had already recovered the hash and recorded it in its pot file. It did not mean the original attack failed.

### Step 9: Run the dictionary attack with Hashcat

Command:

```bash
hashcat -m 1400 hash.txt wordlist.txt
```

**Command explanation:**

- `hashcat`: Starts Hashcat.
- `-m 1400`: Selects hash mode 1400, which is raw SHA2-256.
- `hash.txt`: Supplies the target hash.
- `wordlist.txt`: Supplies the candidate password list.

**Important observed output:**

```text
Hashes: 1 digests; 1 unique digests
Device #01: cpu-haswell-AMD Ryzen 5 7520U with Radeon Graphics
Dictionary cache built:
* Filename..: wordlist.txt
* Passwords.: 3
* Keyspace..: 3

Status...........: Cracked
Hash.Mode........: 1400 (SHA2-256)
Recovered........: 1/1 (100.00%)
Progress.........: 3/3 (100.00%)
Speed.#01........: 13 H/s
```

**Output interpretation:**

- `Hashes: 1 digests`: One target digest was loaded.
- `Device #01`: Identifies the detected CPU OpenCL device.
- `Passwords.: 3`: The dictionary contained three candidates.
- `Hash.Mode: 1400`: Confirms SHA2-256 mode.
- `Status: Cracked`: Hashcat found a matching candidate.
- `Recovered: 1/1`: One of one target hashes was recovered.
- `Progress: 3/3`: All three candidates were tested.
- `Speed: 13 H/s`: The reported rate for this short run on the detected device.

Hashcat warned that the wordlist was too small to use the full parallel capacity of the device. This is expected with only three candidates. The run nevertheless completed successfully. It also reported use of a pure (unoptimized) kernel; this is a kernel selection/performance note, not a failure.

### Step 10: Display the recovered password with Hashcat

Command:

```bash
hashcat -m 1400 hash.txt --show
```

**Command explanation:**

- `-m 1400`: Uses the SHA2-256 hash mode.
- `hash.txt`: Specifies the target digest file.
- `--show`: Displays matching password records already recovered and stored by Hashcat.

**Observed output:**

```text
c5d042bffb1e249604cc3ee528e173b371be5dd0fdc3e598b19432178321cdd8:CyberLab@123
```

This confirms Hashcat's recovered result.

## 4. Results

The same self-generated SHA-256 digest was tested with both tools using the same three-entry wordlist.

| Item | John the Ripper | Hashcat |
|---|---|---|
| Hash format | Raw-SHA256 | Mode 1400 (SHA2-256) |
| Target hashes | 1 | 1 |
| Wordlist candidates | 3 | 3 |
| Attack result | Successfully cracked | Successfully cracked |
| Recovered test password | `CyberLab@123` | `CyberLab@123` |
| Verification | `john --show` | `hashcat --show` |

**Result statement:** The dictionary attack successfully recovered the known synthetic test password from its raw SHA-256 digest using both John the Ripper and Hashcat. Both tools produced the same recovered password.

## 5. Discussion and Security Observations

1. A hash is not decrypted during this process. Candidate passwords are hashed and their digests are compared with the target digest.
2. The experiment demonstrates why a password that appears complex may still be recovered quickly if it is present in an attacker's candidate list.
3. The successful result depended on the exact test password being included in the three-entry wordlist. This small controlled test does not measure the tools' performance against large real-world wordlists.
4. SHA-256 is designed to be fast. That makes it useful for general integrity checks but unsuitable as a direct password-storage function.
5. Production password storage should use a dedicated password-hashing algorithm such as Argon2id, scrypt, or bcrypt, with unique salts and suitable work factors.
6. Hashcat used the CPU-based PoCL OpenCL device detected in this lab. The reported 13 H/s came from a very small test and should not be treated as a general benchmark or compared directly with John’s reported rate.
7. A dictionary attack can fail when the candidate password is absent from the wordlist. More candidates do not guarantee recovery.

## 6. Limitations

- Only one unsalted SHA-256 hash was tested.
- The wordlist contained only three candidate passwords.
- The password was intentionally included in the wordlist, so the practical demonstrates the method rather than the difficulty of recovering an unknown password.
- Hashcat detected CPU-based OpenCL through PoCL; no separate GPU device was shown in the supplied device output.
- No benchmark with a large wordlist, rule-based attack, mask attack, salted hash, or dedicated password-hashing algorithm was performed.
- This experiment does not establish the security of any real password or password-storage system.

## 7. Conclusion

The practical demonstrated controlled password-hash auditing with John the Ripper and Hashcat. A SHA-256 digest was generated locally, saved to a file, and tested against a small dictionary. Both tools successfully identified the matching test password and their `--show` options verified the recovered result. The exercise illustrates the operation of dictionary attacks and the importance of using purpose-built password-hashing algorithms for secure password storage.

## 8. Evidence / Screenshots to Attach

Add screenshots of the actual terminal outputs for:

1. John and Hashcat version checks.
2. Supported John formats and Hashcat device detection.
3. SHA-256 generation and `hash.txt` contents.
4. Creation of `wordlist.txt`.
5. John dictionary attack and `john --show` verification.
6. Hashcat dictionary attack and `hashcat --show` verification.

Do not include unrelated personal files or real credentials in screenshots.
