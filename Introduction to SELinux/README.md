# 🛡️Introduction to SELinux

## 🎯 Objectives
By the end of this lab, you will be able to:
* 📦 Verify and install SELinux policy frameworks on an enterprise Linux host.
* 🎛️ Understand and switch between SELinux operational modes (**Enforcing**, **Permissive**, **Disabled**).
* 🔍 Inspect, modify, and restore security contexts (`Z` attributes) on files and directories.

## 📋 Prerequisites
* **Operating System:** A Linux system utilizing SELinux (preferably RHEL, CentOS, or Fedora).
* **Privileges:** Full root access or administrative `sudo` execution rights.
* **Skills:** Basic familiarity with the Linux command line.

---

## 🛠️ Lab Tasks

### 📂 Task 1: Install and Configure SELinux

#### ⚙️ Subtask 1.1: Verify SELinux Installation
Most modern enterprise Linux distributions ship with SELinux active by default. Audit your system packages to verify its presence:
```bash
# Check if core SELinux utilities are installed
rpm -qa | grep selinux
```
* **Expected Outcome:** Packages such as `libselinux`, `selinux-policy`, and associated core libraries display in the terminal stream.

> 💡 **Troubleshooting Tip:** If your environment is missing these tools entirely, execute your package manager to pull down the targeted security components:
> ```bash
> sudo dnf install selinux-policy selinux-policy-targeted libselinux-utils
> ```

#### ⚙️ Subtask 1.2: Check SELinux Status
Query the real-time runtime configuration parameters of the security module:
```bash
sestatus
```
* **Expected Outcome:** The console dumps a status summary panel matching this structure:
  ```text
  SELinux status:                 enabled
  Current mode:                   enforcing
  Mode from config file:          enforcing
  Loaded policy name:             targeted
  ...
  ```

---

### 📂 Task 2: Understand SELinux Modes

SELinux restricts actions based on three core modes of behavior:
1. **Enforcing:** Policies are strictly applied, and unauthorized operations are blocked and logged (Production Default).
2. **Permissive:** Security policies are not actively enforced; violations are allowed to execute but generate alert logs (Ideal for troubleshooting).
3. **Disabled:** The SELinux kernel subsystem is completely turned off.

#### ⚙️ Subtask 2.1: Switch Between SELinux Modes

**Check Current Running Mode:**
```bash
getenforce
```

**Temporarily Change Mode:**
To lower restrictions on the fly into Permissive tracking mode:
```bash
sudo setenforce 0
```
To re-enable strict policy checking instantly:
```bash
sudo setenforce 1
```
*Note: Runtime alterations executed via `setenforce` are volatile and automatically revert back to defaults upon the next system boot cycle.*

**Permanently Change Mode:**
To save a persistent policy mode across reboots, modify the main subsystem parameter file:
```bash
sudo vi /etc/selinux/config
```
Locate the core variable assignment line and declare your target state:
```text
SELINUX=enforcing
# Options: enforcing | permissive | disabled
```
Save your changes and restart the operating system to re-initialize the kernel layers:
```bash
sudo reboot
```
* **Expected Outcome:** After initialization completes, running `getenforce` confirms your persistent structural change layout.

---

### 📂 Task 3: Check and Modify SELinux Contexts

#### ⚙️ Subtask 3.1: View File Contexts
SELinux controls interactions by labeling files, processes, and network ports with security attributes. Append the uppercase `-Z` flag to view these hidden labels:
```bash
ls -Z /etc/passwd
```
* **Expected Outcome Example:**
  ```text
  system_u:object_r:passwd_file_t:s0 /etc/passwd
  ```
  *(Parsed as: `User` : `Role` : `Type/Domain` : `Sensitivity Level`)*

#### ⚙️ Subtask 3.2: Change File Context
Simulate manual context manipulation using the context changer utility `chcon`:
```bash
# Create a fresh mock tracking web file asset
sudo touch /var/www/html/testfile.html

# Audit its natural unconfigured security labels
ls -Z /var/www/html/testfile.html

# Change the file type context specifically to allow web daemon access
sudo chcon -t httpd_sys_content_t /var/www/html/testfile.html

# Verify the modification
ls -Z /var/www/html/testfile.html
```
* **Expected Outcome:** The file's third descriptor tier successfully mutates over to display `httpd_sys_content_t`.

#### ⚙️ Subtask 3.3: Restore Default Contexts
If a manual path adjustment causes context drift or an application mismatch error, reset the security attributes back to their system database defaults using the restore utility:
```bash
sudo restorecon -v /var/www/html/testfile.html
```
* **Expected Outcome:** The console prints a notification tracking line showing that the volatile `chcon` parameters were overwritten to align with global subsystem layout policies.

---

## 🏁 Conclusion
By completing this security administration lab, you have mastered:
* Auditing infrastructure compliance settings via `sestatus` and `getenforce`.
* Changing running protection modes dynamically and writing permanent parameters into `/etc/selinux/config`.
* Reading and correcting mandatory access control contexts (`MAC`) using `chcon` and `restorecon`.

> 📈 **Key Takeaway:** SELinux secures infrastructure components by applying Mandatory Access Control (MAC). For zero-downtime debugging, always check system behaviors in Permissive mode before applying custom policies.

### 💡 Troubleshooting Tips
* **Analyzing Blocked Services:** If a web engine or storage daemon errors out unexpectedly, check your audit logs for Access Vector Cache (`AVC`) denials:
  ```bash
  sudo ausearch -m avc -ts recent
  ```
* **Compiling Policy Exceptions:** If a specific execution path is verified safe but blocked by policies, pipe recent denial logs into `audit2allow` to build an exceptional module injection wrapper:
  ```bash
  sudo ausearch -m avc -ts recent | audit2allow -M mycustompolicy
  sudo semodule -i mycustompolicy.pp
  ```

### 🚀 Next Steps
* Explore **SELinux Booleans** (`getsebool` / `setsebool`) to toggle application permissions dynamically without rebuilding policy files.
* Learn about directory context inheritance behaviors using persistent mapping keys via `semanage fcontext`.

### 📚 Additional Resources
* 📖 Manual help pages: `man selinux`, `man chcon`, `man sestatus`, `man restorecon`
* 🌐 [Official SELinux Project Documentation Wiki Reference](https://github.com)
