# 🎫 Repository and Subscription Management

A hands-on guide to registering an enterprise host with Red Hat Subscription Manager (RHSM), locking down authorized repository feeds, and deploying software under a validated subscription matrix.

## 🎯 Objectives
By the end of this lab, you will be able to:
* **Register** an active system node with Red Hat Subscription Manager (RHSM).
* **Enable and disable** targeted repositories for granular software stream management.
* **Install** stable enterprise software packages from official verified channels.

## 📋 Prerequisites
* A Red Hat Enterprise Linux (RHEL 8 or RHEL 9) installation.
* Root execution rights or explicit `sudo` administrative privileges.
* Valid Red Hat Customer Portal subscription credentials.
* Unrestricted outbound internet connectivity.
* Core utilities `subscription-manager` and `dnf` present (included as default tools).

---

## 🚀 Lab Setup

Before starting subscription registrations, verify that your local package index is initialized:
```bash
sudo dnf update -y
```

---

## 🛠️ Lab Tasks

### 🔑 Task 1: Register System with Red Hat Subscription Manager

#### 📡 Step 1.1: Verify Current Subscription Status
Audit your local machine to determine its licensing state before attempting upstream authentication:
```bash
sudo subscription-manager status
```
**Expected Output:**
```text
Overall Status: Unknown
```
*(Note: If the system has already been linked, detailed validation metrics will display here instead).*

#### 🔒 Step 1.2: Register System
Authenticate and link the computer hardware to your customer tracking dashboard. Swap out `<username>` and `<password>` with your authentic portal credentials:
```bash
sudo subscription-manager register --username=<username> --password=<password>
```

> 💡 **Troubleshooting Alternatives:**
> * **Corporate Proxy:** If traffic must route through a gateway proxy appliance, append connection flags:
>   ```bash
>   sudo subscription-manager register --username=<username> --password=<password> --proxy=<proxy_URL> --proxyuser=<proxy_user> --proxypassword=<proxy_password>
>   ```
> * **Activation Keys:** For deployment configurations inside enterprise data centers using pre-staged asset tokens:
>   ```bash
>   sudo subscription-manager register --org=<org_ID> --activationkey=<key>
>   ```

#### 📎 Step 1.3: Attach a Subscription
Instruct the platform engine to map an available entitlement entitlement pass automatically from your pool:
```bash
sudo subscription-manager attach --auto
```
Verify the licensing bindings have safely synced with the host:
```bash
sudo subscription-manager list --consumed
```

---

### ⚙️ Task 2: Manage Repositories

#### 📋 Step 2.1: List Available Repositories
Enumerate every upstream distribution software channel assigned to your current account layer:
```bash
sudo subscription-manager repos --list
```

#### 🟢 Step 2.2: Enable a Repository
Activate a developer resource channel (such as the CodeReady Builder repository) for your native hardware architecture:
```bash
sudo subscription-manager repos --enable=codeready-builder-for-rhel-9-$(arch)-rpms
```

#### 🔴 Step 2.3: Disable a Repository
Turn off unneeded external channels to tighten security boundaries and minimize configuration overhead:
```bash
sudo subscription-manager repos --disable=codeready-builder-for-rhel-9-$(arch)-rpms
```

#### ✅ Step 2.4: Verify Enabled Repositories
Query the active distribution engine to map which target storage feeds are fully readable:
```bash
sudo dnf repolist enabled
```

---

### 📦 Task 3: Install Software from Repositories

#### 🔍 Step 3.1: Search for a Package
Scan your synchronized software channels for container engine options, like Podman:
```bash
sudo dnf search podman
```

#### 🚀 Step 3.2: Install the Package
Download and install the virtualization software using the automated flag skip:
```bash
sudo dnf install -y podman
```

#### 🏁 Step 3.3: Verify Installation
Confirm that the compiled engine binary works properly on the command line:
```bash
podman --version
```
**Expected Output:**
```text
podman version 4.x.x
```

---

## 💡 Key Concepts
* **Subscription Management:** The compulsory validation gateway required to open connections toward official Red Hat enterprise update feeds.
* **Repository:** A trusted, structured network storage endpoint housing signed RPM binaries and software configurations.
* **DNF:** The high-speed processing package platform engine underpinning modern RHEL systems.

---

## 🩺 Troubleshooting Tips

* **Registration Errors:** Verify basic network routing paths to upstream services and re-test password configurations on the web portal interface.
* **Missing Repositories:** Ensure that the attachment pool parsing phase finished error-free (`subscription-manager attach --auto`).
* **Package Not Found:** Forcefully purge old cached repository indexes via `sudo dnf clean all`, then review available endpoints using `dnf repolist`.

### 🗺️ Next Steps
* Learn to deploy custom software sources across your network utilizing the flexible `dnf config-manager` command utility extension.
* Build external tracking file targets for offline repository sync tools.
