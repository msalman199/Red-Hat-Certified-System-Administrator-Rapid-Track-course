# 🎩 Accessing Red Hat Support

## 🎯 Objectives
By the end of this lab, you will be able to:
* 📊 Collect comprehensive system diagnostic data using open-source tools.
* 🎫 Understand how to draft and submit a formal support case to Red Hat.
* 🔍 Explore and navigate Red Hat's extensive internal Knowledgebase resources.
* 📈 Learn the proper operational process for escalating an active support case.

## 📋 Prerequisites
* **Operating System:** A system running RHEL 8 or 9 (or CentOS Stream as an open-source substitute for lab purposes).
* **Account:** A valid Red Hat customer account (or a free Red Hat Developer profile).
* **Network:** Active internet connectivity.
* **Skills:** Basic command-line proficiency.

> 📝 **Note:** For this laboratory workspace, we will use CentOS Stream as an open-source alternative where RHEL-specific diagnostic utilities are not fully available.

---

## 🛠️ Lab Tasks

### 📂 Task 1: Collect System Diagnostic Data

#### ⚙️ Subtask 1.1: Gather Basic System Information
Open a terminal workspace and execute the following commands to map out your underlying environment structure:
```bash
# Install and run a comprehensive system information utility
sudo dnf install -y inxi
inxi -Fxz

# Collect core Kernel and OS deployment details
uname -a
cat /etc/os-release

# Inspect active hardware configurations, memory state, and block devices
lscpu
free -h
lsblk
```
* **Expected Outcome:** Comprehensive and structured system environment parameters print directly into your terminal output buffer.

> 💡 **Troubleshooting Tip:** If your package manager returns an error that `inxi` is missing, initialize the Extra Packages for Enterprise Linux (EPEL) repository path first:
> ```bash
> sudo dnf install -y epel-release
> ```

#### ⚙️ Subtask 1.2: Collect System Logs
Compile and isolate system runtime records to assist support engineering analysis:
```bash
# Create and enter a dedicated workspace folder for diagnostic data
mkdir ~/system_diagnostics
cd ~/system_diagnostics

# Filter and capture system daemon records spanning the last 24 hours
journalctl --since "1 day ago" > system_journal.log

# Extract low-level kernel ring buffer streams
dmesg > dmesg_output.log

# Dump an indexed file listing of every single installed system package
rpm -qa > installed_packages.list

# Compress and package all gathered metrics into a date-stamped archive
tar -czvf system_diagnostics_\$(date +%Y%m%d).tar.gz *
```
* **Expected Outcome:** A clean, compressed tarball (`system_diagnostics_[YYYYMMDD].tar.gz`) containing raw log tracks is successfully written to disk.

---

### 📂 Task 2: Submit a Support Case to Red Hat

#### ⚙️ Subtask 2.1: Access the Red Hat Customer Portal
1. Launch your web browser and navigate directly to the [Red Hat Customer Portal](https://access.redhat.com).
2. Log in using your registered subscription credentials.
3. *If you need an account, create a free profile at the [Red Hat Developer Portal](https://developers.redhat.com/register).*

#### ⚙️ Subtask 2.2: Create a New Support Case
1. Inside the portal interface header, select **Support** → **Open a Case**.
2. Populate the required case generation fields:
   * **Product:** Select your matching environment platform (e.g., `Red Hat Enterprise Linux`).
   * **Severity:** Choose **3 - Moderate** for standard lab simulation training.
   * **Subject / Description:** Provide an explicit summary description detailing the technical blocker.
3. Click the upload field to attach the compressed `system_diagnostics` tarball created during Task 1.
4. Click **Submit**.
* **Expected Outcome:** The system accepts the issue, generates a unique support case index number tracking line, and dispatches an automated verification email.

> 💡 **Troubleshooting Tip:** For sandbox lab testing environments without an actual infrastructure platform failure, you can practice completing the text entries up to the final page without hitting the submission button.

---

### 📂 Task 3: Explore Red Hat's Knowledgebase and Escalate a Case

#### ⚙️ Subtask 3.1: Search the Knowledgebase
1. From the portal header navigation, select the **Knowledgebase** link tool.
2. Type an inquiry search string to research common enterprise anomalies (e.g., *“Podman container startup failure”*).
3. Evaluate the ranked technical engineering solution layouts.

You can simulate a standard workspace verification test locally:
```bash
podman run --rm hello-world
```
* **Expected Outcome:** High-utility configuration fixes, verified articles, and internal dependency workaround guides display on screen.

#### ⚙️ Subtask 3.2: Escalate a Support Case
If a ticket criteria line shifts and demands rapid resolution context:
1. Navigate directly to **Support** → **My Cases** and open your active ticket tracking index.
2. Use the **Add Comment** window component to submit updated operational details or data logs.
3. Click **Request Escalation**, provide clear business logic justification explaining why the priority needs adjustment, and submit.
* **Expected Outcome:** The support case tracking record dynamically transitions states to reflect your formal escalation workflow request.

---

## 🏁 Conclusion
By completing this practical lab module, you have mastered:
* Leveraging system queries to gather log outputs and build isolated troubleshooting archives.
* The structure for registering technical complaints and formatting server tickets on the portal dashboard.
* Researching resolutions to operational issues inside the customer support portal interface.
* Modifying ticket priorities via formal product escalation routines.

These competencies are essential for safely managing and maintaining production platforms, particularly when debugging container dependencies like Podman or orchestration layers like OpenShift.

### 🧹 Clean Up
Once your exercises are complete, delete the diagnostic directories and files to free up disk storage space:
```bash
rm -rf ~/system_diagnostics
```

### 📚 Additional Resources
* 📖 [Red Hat Customer Portal Knowledgebase Engine](https://redhat.com)
* 🌐 [Official Red Hat Support Management Engagement Guide](https://redhat.com)
* 🐳 [Podman Container Troubleshooting Guide Reference](https://podman.io)
