
# 🛠️ Troubleshooting SELinux Lab

A hands-on guide to identifying Security-Enhanced Linux (SELinux) denials, creating custom policy modules, and using `audit2allow` to resolve access issues securely.

## 🎯 Objectives
By the end of this lab, you will be able to:
* **Identify** SELinux denials in system logs.
* **Create** custom SELinux policies to resolve access issues.
* **Use** `audit2allow` to generate and apply SELinux policy rules.

## 📋 Prerequisites
* A Linux system with SELinux enabled (RHEL, CentOS, or Fedora preferred).
* Root or `sudo` privileges.
* Basic understanding of Linux commands and SELinux concepts.
* Required packages installed:
```bash
sudo dnf install -y policycoreutils-python-utils setools-console audit
```

---

## 🚀 Lab Tasks

### 🔍 Task 1: Identify SELinux Denials Using Logs

#### 💻 Subtask 1.1: Check SELinux Status
Verify that SELinux is enabled and running in enforcing mode:
```bash
sestatus
```
**Expected Output:**
```text
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Current mode:                   enforcing
```
If SELinux is not set to enforcing, enable it dynamically:
```bash
sudo setenforce 1
```

#### 📂 Subtask 1.2: Analyze SELinux Denials in Audit Logs
Search the audit logs for recent Access Vector Cache (AVC) denials:
```bash
sudo ausearch -m avc -ts recent
```
**Expected Output:**
```text
time->[timestamp]
type=AVC msg=audit(1234567890.123:456): avc: denied { read } for pid=1234 comm="nginx" name="index.html" dev="sda1" ino=123456 scontext=system_u:system_r:httpd_t:s0 tcontext=unconfined_u:object_r:default_t:s0 tclass=file
```
Alternatively, inspect the raw audit log file directly:
```bash
sudo grep "avc:.*denied" /var/log/audit/audit.log
```
> 💡 **Troubleshooting Tip:** If no denials appear, manually trigger one. For example, move a file to `/var/www/html/` without restoring its proper web context, then try to access it via your web server.

---

### 🛠️ Task 2: Create Custom SELinux Policies

#### ⚙️ Subtask 2.1: Generate a Custom Policy Module Using audit2allow
Extract the specific denial logs and compile them into a custom policy module named `mypolicy`:
```bash
sudo ausearch -m avc -ts recent | audit2allow -M mypolicy
```
**Expected Output:**
```text
Generating type enforcement file: mypolicy.te
Compiling policy: checkmodule -M -m -o mypolicy.mod mypolicy.te
semodule_package -o mypolicy.pp -m mypolicy.mod
```
Apply and inject the newly generated policy module into the kernel:
```bash
sudo semodule -i mypolicy.pp
```

#### ✅ Subtask 2.2: Verify the Policy is Loaded
Confirm that your custom module is successfully active in the system:
```bash
sudo semodule -l | grep mypolicy
```
**Expected Output:**
```text
mypolicy
```
> 💡 **Troubleshooting Tip:** If the service still fails, repeat the process. SELinux blocks operations sequentially; resolving a `read` restriction may expose a subsequent `write` restriction.

---

### ⚙️ Task 3: Use audit2allow to Generate and Apply SELinux Rules

#### 📝 Subtask 3.1: Generate Rules from Audit Logs
View the human-readable Type Enforcement (`.te`) rules that `audit2allow` recommends based on log history:
```bash
sudo ausearch -m avc -ts recent | audit2allow
```
**Expected Output:**
```text
#============= httpd_t ==============
allow httpd_t default_t:file read;
```
To automatically generate and apply a refined policy file (e.g., `mynewpolicy`):
```bash
sudo ausearch -m avc -ts recent | audit2allow -M mynewpolicy
sudo semodule -i mynewpolicy.pp
```

#### 🧪 Subtask 3.2: Test the Applied Policy
1. Retry the operation that originally triggered the denial.
2. Verify that no new denials are generated in the logs:
```bash
sudo ausearch -m avc -ts recent
```
> 💡 **Troubleshooting Tip:** Creating custom policies should be a last resort. Always consider fixing file labels permanently using `chcon` or `restorecon` before modifying global policies.

---

## 🏁 Conclusion
You have successfully completed the following milestones:
* Identified hidden SELinux restrictions using `ausearch` and audit logs.
* Generated, compiled, and injected custom system policies using `audit2allow`.
* Validated policy enforcement states.

### 🗺️ Next Steps
To further build your security administration skills, try exploring:
* **SELinux Booleans:** Toggle built-in features using `getsebool` and `setsebool`.
* **Context Restoration:** Practice setting persistent file labels using `semanage fcontext` and `restorecon`.
