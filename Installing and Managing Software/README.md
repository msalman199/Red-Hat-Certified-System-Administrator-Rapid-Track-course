# 📦 Software Management in RHEL Lab

A hands-on guide to mastering software installation, updating, removal, repository configuration, and dependency management using `dnf` and `yum` on Red Hat Enterprise Linux.

## 🎯 Objectives
By the end of this lab, you will be able to:
* **Install, update, and remove** software packages using `dnf` or `yum`.
* **Configure** additional software repositories for enterprise package scaling.
* **Resolve** and manage complex system package dependencies.

## 📋 Prerequisites
* A Red Hat Enterprise Linux (RHEL 8 or 9) system, or an alternative enterprise distribution like Rocky Linux or AlmaLinux.
* Root or `sudo` administrative access.
* Active internet connectivity for repository synchronization.

---

## 🚀 Lab Tasks

### 🔍 Task 1: Installing Packages with yum or dnf

#### 💻 Subtask 1.1: Check Available Package Managers
Modern RHEL distributions use `dnf` (Dandified YUM) as the default manager, with `yum` pointing to it via a symbolic link. 

Verify the system path bindings for both commands:
```bash
which dnf yum
```
**Expected Output:**
```text
/usr/bin/dnf  
/usr/bin/yum  
```
Check the underlying framework tool versions:
```bash
dnf --version
```

#### 📦 Subtask 1.2: Install a Package
Install the interactive `htop` utility while automatically answering yes to confirmation prompts:
```bash
sudo dnf install htop -y
```
**Expected Output:** Detailed metadata download screens, followed by dependency tracking trees and a final installation confirmation.
> 💡 **Troubleshooting Tip:** If the package lookup operation fails completely, check your outbound network traffic using `ping -c 3 google.com`, or inspect your active software storage targets with `sudo dnf repolist`.

---

### 🔄 Task 2: Updating and Removing Packages

#### 🚀 Subtask 2.1: Update All Packages
Synchronize repository feeds and pull down security fixes, bug patches, and package updates:
```bash
sudo dnf update -y
```
**Expected Output:** A breakdown list of upgradable packages, ending with a clean transaction confirmation summary.

#### 🗑️ Subtask 2.2: Remove a Package
Uninstall the `htop` utility from your local operating system layer:
```bash
sudo dnf remove htop -y
```
**Expected Output:** Verification of package metadata removal and file deletion paths.
> 💡 **Key Concept:** Running `sudo dnf autoremove` cleans up orphaned software dependencies that were originally dragged in by other utilities but are no longer required by any active system service.

---

### ⚙️ Task 3: Configuring Repositories and Managing Dependencies

#### 📋 Subtask 3.1: List Enabled Repositories
Identify which upstream distribution platforms are currently linked to your package system:
```bash
sudo dnf repolist
```
**Expected Output:** Active base operating system streams, typically listing core endpoints like `BaseOS` and `AppStream`.

#### ➕ Subtask 3.2: Add the EPEL Repository
Inject the Extra Packages for Enterprise Linux (EPEL) catalog to access a wider pool of community-supported open-source development software:
```bash
sudo dnf install epel-release -y
```
Verify the newly injected software streams are loaded into the operational engine matrix:
```bash
sudo dnf repolist | grep epel
```
**Expected Output:** Terminal rows confirming that the `epel` repository target is active.

#### 🔗 Subtask 3.3: Resolve Dependencies
Deploy an application with an intricate nested dependency graph, like the Nginx web engine:
```bash
sudo dnf install nginx -y
```
**Expected Output:** Automatic parsing and sequential resolution of prerequisite network utilities and supporting libraries before finalizing the installation.
> 💡 **Troubleshooting Tip:** If dependency validation loops error out or get stuck on dead package paths, flush your local storage caches completely using `sudo dnf clean all` and restart the setup command.

---

## 🏁 Conclusion
You have successfully completed the following package management milestones:
* Standardized software installations, bulk package revisions, and system uninstalls via `dnf`.
* Integrated third-party configurations like EPEL into your system channels.
* Cleared broken dependencies using structural tracking loops.

### 🗺️ Next Steps
* **Audit Trails:** Execute `dnf history` to review and undo previous package changes.
* **Low-Level Inspections:** Use the `rpm` command toolchain for querying local configurations.

### ✅ Verification Check
Run this terminal query to confirm your lab state:
```bash
dnf list installed | grep -E 'nginx|htop'
```
**Expected Output:** Prints metadata layout fields for either of the test modules if they remain on your storage drives.
