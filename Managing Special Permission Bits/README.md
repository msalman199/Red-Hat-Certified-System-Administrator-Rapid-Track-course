# 🔒 Managing Special Permission Bits

## 🎯 Objectives
By the end of this lab, you will be able to:
* 📁 Understand and apply the **Sticky Bit** on shared directories to prevent accidental file deletion in multi-user environments.
* ⚙️ Configure **SUID (Set User ID)** and **SGID (Set Group ID)** permissions on executables to safely delegate elevated privileges.
* 🧪 Verify the real-world operational effectiveness of these special permission bits within a secure Linux environment.

## 📋 Prerequisites
* **System:** A Linux-based operating system (e.g., Fedora, CentOS, RHEL, or Ubuntu).
* **Privileges:** Administrative root access or full `sudo` execution rights.
* **Skills:** Basic familiarity with standard Linux file permissions (`chmod`, `ls -l`).

---

## 🛠️ Lab Tasks

### 📂 Task 1: Set the Sticky Bit on a Shared Directory

The Sticky Bit is a security mechanism used on shared directories. It ensures that even if multiple users have full write access to a directory, a user can only delete or rename files that they explicitly own (or if they are the root administrator).

#### ⚙️ Subtask 1.1: Create a Shared Directory
Open a terminal and provision a public scratch folder inside the `/tmp` hierarchy:
```bash
mkdir /tmp/shared_dir
```
Grant open read, write, and execute permissions across all user classes:
```bash
chmod 777 /tmp/shared_dir
```
Audit the open permissions configuration state:
```bash
ls -ld /tmp/shared_dir
```
* **Expected Output Example:**
  ```text
  drwxrwxrwx 2 user user 4096 May 18 10:00 /tmp/shared_dir
  ```

#### ⚙️ Subtask 1.2: Apply the Sticky Bit
Inject the special sticky bit restriction using symbolic notation flags:
```bash
chmod +t /tmp/shared_dir
```
Verify the directory structural attribute change:
```bash
ls -ld /tmp/shared_dir
```
* **Expected Output Example:**
  ```text
  drwxrwxrwt 2 user user 4096 May 18 10:00 /tmp/shared_dir
  ```
*Note: The presence of the lowercase letter **`t`** at the very end of the permission string confirms the Sticky Bit is active.*

#### ⚙️ Subtask 1.3: Test the Sticky Bit
Create a baseline verification file using your current account profile context:
```bash
touch /tmp/shared_dir/user1_file
```
Simulate an alternate unprivileged user account (`nobody`) generating a secondary file inside the directory boundary:
```bash
sudo -u nobody touch /tmp/shared_dir/nobody_file
```
Attempt to break cross-user boundaries by deleting the original file using the alternate unprivileged user context:
```bash
sudo -u nobody rm /tmp/shared_dir/user1_file
```
* **Expected Outcome:** The file engine blocks execution and returns an explicit error message: `rm: cannot remove '/tmp/shared_dir/user1_file': Permission denied`.

---

### 📂 Task 2: Apply setuid and setgid on Executables

The SUID and SGID permissions alter the standard kernel execution behavior. When applied, the binary or script runs with the underlying operational privileges of the file's **owner** (SUID) or **group** (SGID), rather than the user who executes it.

#### ⚙️ Subtask 2.1: Create a Test Executable
Write a basic shell script that checks and reveals the real-time running Effective User ID (`EUID`) context:
```bash
echo '#!/bin/bash
echo "Effective UID: \$EUID"' > /usr/local/bin/show_euid
```
Apply standard execution permission bits to the file:
```bash
chmod +x /usr/local/bin/show_euid
```

#### ⚙️ Subtask 2.2: Apply setuid
Configure the SUID bit on your newly deployed binary shell script:
```bash
sudo chmod u+s /usr/local/bin/show_euid
```
Verify the extended user execution flag changes:
```bash
ls -l /usr/local/bin/show_euid
```
* **Expected Output Example:**
  ```text
  -rwsr-xr-x 1 root root 45 May 18 10:05 /usr/local/bin/show_euid
  ```
*Note: The **`s`** character replacing the owner's traditional `x` slot confirms SUID is active.*

Execute a verification test using an unprivileged background system account:
```bash
sudo -u nobody /usr/local/bin/show_euid
```
* **Expected Output:** `Effective UID: 0` *(Confirming execution successfully escalated to match the root owner).*

#### ⚙️ Subtask 2.3: Apply setgid
Create a dedicated secondary tracking group identity and shift the file's group boundary ownership:
```bash
sudo groupadd testgroup
sudo chown :testgroup /usr/local/bin/show_euid
```
Inject the matching SGID attribute flag:
```bash
sudo chmod g+s /usr/local/bin/show_euid
```
Audit the final combined permission mapping status:
```bash
ls -l /usr/local/bin/show_euid
```
* **Expected Output Example:**
  ```text
  -rwsr-sr-x 1 root testgroup 45 May 18 10:05 /usr/local/bin/show_euid
  ```
*Note: The inclusion of an **`s`** character within the group context triad flags active SGID configuration.*

---

### 📂 Task 3: Verify the Effectiveness of Permissions

#### ⚙️ Subtask 3.1: Check Sticky Bit Enforcement
Ensure that loose public workspaces (like `/tmp/shared_dir`) continue blocking standard cross-user deletions while maintaining data collaboration paths.

#### ⚙️ Subtask 3.2: Verify setuid/setgid Execution
Ensure tasks that require specific root tasks (like password changes or disk operations) execute with their targeted parent system rights when SUID/SGID properties are configured.

---

## 🏁 Conclusion
By completing this advanced special permissions laboratory session, you have mastered:
* Applying the Sticky Bit (`+t`) to isolate files within multi-user directories.
* Injecting SUID (`u+s`) and SGID (`g+s`) parameters to control privilege delegation.
* Auditing infrastructure file flags using standard tools and validating access blocks.

These methods are essential tools for securing Linux multi-tenant nodes, structuring secure application execution runtimes, and managing access rules safely.

### 💡 Troubleshooting Tips
* **SUID Actions Failing to Escalate:** Modern Linux distributions often intentionally ignore the SUID bit on interpreted text scripts (like raw bash blocks) for security reasons. If the escalation fails, test the mechanism using compiled binary targets.
* **Filesystem Blocks:** Verify if your target filesystem partition rules are overriding your bits by running a mount check. Look out for safety flags like `nosuid`:
  ```bash
  mount | grep nosuid
  ```
* **Deep Permission Inspection:** For a granular metadata layout breakdown of absolute octal weights and context blocks, query your files using the `stat` engine utility:
  ```bash
  stat /usr/local/bin/show_euid
  ```
