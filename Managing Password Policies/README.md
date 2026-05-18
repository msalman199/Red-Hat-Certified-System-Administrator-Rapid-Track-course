# 🔐 Managing Password Policies

## 🎯 Objectives
By the end of this lab, you will be able to:
* 🛡️ Enforce robust password policies on Linux systems to ensure secure access control.
* ⚙️ Configure granular password complexity rules and validation requirements.
* ⏳ Implement password aging limits globally and via local configuration structures.
* 🧪 Test custom security policies using isolated, sandbox test user scenarios.

## 📋 Prerequisites
* **Operating System:** A Linux system (preferably RHEL, CentOS 8+, or Fedora).
* **Privileges:** Full root access or administrative `sudo` execution rights.
* **Skills:** Basic knowledge of the Linux command line.
* **Utilities:** Standard system binaries `passwd` and `chage` (bundled by default).

---

## 🏗️ Lab Setup
Ensure you have a working Linux installation, then spin up two independent user structures exclusively for policy testing:
```bash
sudo useradd testuser1
sudo useradd testuser2
```

---

## 🛠️ Lab Tasks

### 📂 Task 1: Configure Password Expiration and Complexity Requirements

#### ⚙️ Subtask 1.1: Install Required Packages
Pull down the system password validation framework utility package:
```bash
sudo dnf install libpwquality -y
```
* **Explanation:** `libpwquality` hooks into the system authentication engine to provide active password quality metric checking and entropy testing.

#### ⚙️ Subtask 1.2: Configure Password Complexity
Open the primary password strength profile layout using a terminal text editor:
```bash
sudo nano /etc/security/pwquality.conf
```
Uncomment, append, or modify the following runtime evaluation directives:
```text
minlen = 12
minclass = 4
dcredit = -1
ucredit = -1
lcredit = -1
ocredit = -1
```
* **🧠 Parameters Explanation:**
  * `minlen`: Enforces the absolute minimum allowed password length to 12 characters.
  * `minclass`: Demands character diversity across standard character blocks (upper, lower, numerical, special characters).
  * `dcredit` / `ucredit` / `lcredit` / `ocredit`: Dictates validation credits and explicit requirements for uppercase characters, lowercase flags, digits, and special formatting characters. Setting these parameters to negative limits acts as a strict minimum requirement constraint.

#### ⚙️ Subtask 1.3: Verify Configuration
Pipe testing string configurations directly into the validation evaluation parser to check their alignment scores:
```bash
echo "Weakpass1" | sudo pwscore
echo "StrongPass123!" | sudo pwscore
```
* **Expected Outcome:** The first command evaluation string fails immediately and drops out with a low quality score error block, while the second string passes security threshold requirements cleanly.

---

### 📂 Task 2: Set Password Aging and Enforce Policies

#### ⚙️ Subtask 2.1: Configure Global Password Aging
Open the primary template file that sets the default parameters for system identities and authorization attributes:
```bash
sudo nano /etc/login.defs
```
Locate and reconfigure the global system baseline constraints:
```text
PASS_MAX_DAYS 90
PASS_MIN_DAYS 7
PASS_WARN_AGE 14
PASS_MIN_LEN 12
```
* **🧠 Parameters Explanation:**
  * `PASS_MAX_DAYS`: The maximum lifespan threshold a password can remain unchanged before expiring.
  * `PASS_MIN_DAYS`: The minimum cooling period requiring a password to stay locked in place before a user can modify it again.
  * `PASS_WARN_AGE`: The window of days prior to expiration during which the system displays a countdown warning notification to users at login.

#### ⚙️ Subtask 2.2: Apply Policies to Existing Users
Because global template directives inside `/etc/login.defs` only apply out of the box to *new* users, manually align your pre-existing sandbox identities using the user aging tool:
```bash
sudo chage -M 90 -m 7 -W 14 testuser1
sudo chage -M 90 -m 7 -W 14 testuser2
```
Audit the configuration attributes to confirm modification:
```bash
sudo chage -l testuser1
```

#### ⚙️ Subtask 2.3: Lock Inactive Accounts
Configure system account boundaries to automatically trigger an explicit freeze lock sequence on accounts if their credentials stay expired and inactive for more than 30 consecutive days:
```bash
sudo useradd -D -f 30
```

---

### 📂 Task 3: Test Password Policies

#### ⚙️ Subtask 3.1: Test Password Complexity
Attempt to modify the user credentials and observe the policy enforcement engine in action:
```bash
sudo passwd testuser1
```
* Try configuring a simple credential block (such as `password123`). The system engine will intentionally throw a validation warning and block implementation.
* Provide a high-entropy string (such as `SecurePass123!`). The change will complete successfully.

#### ⚙️ Subtask 3.2: Test Password Aging
Inspect expiration timestamps:
```bash
sudo chage -l testuser1
```
Simulate an immediate password expiration scenario by resetting the system's tracking epoch metric on the user target:
```bash
sudo chage -d 0 testuser1
```
* **Expected Outcome:** The underlying tracking parameters flag the credential as immediate stale data. The target account user is forced to perform a complete password reset workflow during their very next connection session.

#### ⚙️ Subtask 3.3: Test Account Locking
Simulate a stale, unmaintained user workspace scenario by applying strict expiration limits:
```bash
sudo chage -I 30 -E \$(date -d "+30 days" +%Y-%m-%d) testuser2
```
Verify that the security flags align properly within the account ledger tracking array:
```bash
sudo chage -l testuser2
```

---

## 🏁 Conclusion
By successfully executing this security infrastructure deployment, you have mastered:
* Installing and customizing validation layers using the `libpwquality` configuration tree.
* Manipulating account tracking metadata profiles and default configuration bounds.
* Mitigating access vectors by defining automated user lock criteria for stale profiles.

Implementing baseline measures like these helps minimize risk, improves audit postures, and reinforces credential rotation workflows across core Linux networks.

### 💡 Troubleshooting Tips
* **Policies Failing to Restrict Changes:** Confirm that the authentication framework lines match up properly inside the local PAM system profile configuration ledger `/etc/pam.d/system-auth`. Ensure `pam_pwquality.so` is accurately specified.
* **Enforcement Overrides / Drift:** Verify your running security context state via SELinux if configuration rules are failing to apply globally.
* **Account Locking Discrepancies:** Track down security logging traces and verify user connection errors directly within the standard identity authorization records file: `/var/log/secure`.
* **Complexity Failures:** Verify the syntax strings inside `/etc/security/pwquality.conf` and guarantee that the package engine is actively installed on the system node via `rpm -q libpwquality`.

### 🧹 Cleanup (Optional)
To delete your testing user frameworks and clear residual data records, execute the following commands:
```bash
sudo userdel -r testuser1
sudo userdel -r testuser2
```
