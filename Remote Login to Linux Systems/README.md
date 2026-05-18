# 🌐 Remote Login to Linux Systems

## 🎯 Objectives
By the end of this lab, you will be able to:
* 🔒 Securely log in to a Linux system remotely using SSH.
* 🔑 Generate and configure SSH key pairs for robust authentication.
* ⚙️ Understand basic SSH server configuration parameters.

## 📋 Prerequisites
* **Local Machine:** A Linux/macOS machine (or Windows configured with WSL2/OpenSSH).
* **Remote Machine:** A remote Linux system with an active SSH server (e.g., Ubuntu, CentOS).
* **Access:** Terminal access on both machines and basic command-line familiarity.

---

## 🛠️ Lab Tasks

### 📂 Task 1: Generate SSH Key Pair

#### ⚙️ Subtask 1.1: Check for Existing SSH Keys
Open a terminal on your local machine and verify if any cryptographic keys already exist:
```bash
ls ~/.ssh/
```
* If files named `id_rsa` and `id_rsa.pub` are present, you may skip key generation or proceed to overwrite them.
* If the `.ssh` directory does not exist, move to Subtask 1.2.

#### ⚙️ Subtask 1.2: Generate a New Key Pair
Execute the generation tool (replace `your_email@example.com` with your actual email address):
```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
* `-t rsa`: Specifies the key algorithm type (RSA).
* `-b 4096`: Sets the key length to a secure 4096 bits.
* `-C`: Appends a descriptive tracking comment.

**Configuration Prompts:**
1. Press `Enter` to save the key to the default file location (`~/.ssh/id_rsa`).
2. Type a secure passphrase to encrypt the key locally, or press `Enter` to leave it blank.

Verify the successful creation of your keys:
```bash
ls ~/.ssh/
```
* **Expected Output:** You should see `id_rsa` (your private key) and `id_rsa.pub` (your public key).

> 💡 **Troubleshooting Tip:** If your terminal shows an error that `ssh-keygen` is not found, install the OpenSSH client package:
> ```bash
> sudo apt install openssh-client   # Debian/Ubuntu
> sudo yum install openssh-clients  # CentOS/RHEL
> ```

---

### 📂 Task 2: Configure SSH Server on the Remote Machine

#### ⚙️ Subtask 2.1: Install SSH Server (if not present)
Log into your remote server machine natively via a local console or an existing access session. Ensure the server software is running:
```bash
sudo apt install openssh-server   # Debian/Ubuntu
sudo yum install openssh-server   # CentOS/RHEL
```
Start the service daemon and configure it to initialize automatically at system boot time:
```bash
sudo systemctl start sshd
sudo systemctl enable sshd
```

#### ⚙️ Subtask 2.2: Copy Public Key to Remote Machine
From your **local machine**, push your public key credentials across the network to the server host:
```bash
ssh-copy-id username@remote_server_ip
```
*(Provide the remote user account password when requested by the utility).*

**Alternative Manual Approach:** If the automated script tool is unavailable, manually pipe the key entry over standard inputs:
```bash
cat ~/.ssh/id_rsa.pub | ssh username@remote_server_ip "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"
```

#### ⚙️ Subtask 2.3: Secure SSH Server Configuration
Open the primary daemon configuration file on the remote server using a text editor:
```bash
sudo nano /etc/ssh/sshd_config
```
Locate and change the following operational properties to tighten access security:
```text
PasswordAuthentication no         # Disable password-based login
PermitRootLogin no                # Block direct administrative root access
PubkeyAuthentication yes          # Expressly permit key-based validation
```
Save the text modification, close the editor, and reload the background service to enforce changes:
```bash
sudo systemctl restart sshd
```

> 💡 **Troubleshooting Tip:** If `ssh-copy-id` fails to connect initially, ensure that `PasswordAuthentication` remains enabled on the server temporarily until keys are placed. Also check your local network firewall permissions:
> ```bash
> sudo ufw allow 22   # Ubuntu firewall rule change
> ```

---

### 📂 Task 3: Log in Remotely Using SSH

#### ⚙️ Subtask 3.1: Test SSH Login
Initiate an encrypted terminal connection from your local terminal workspace:
```bash
ssh username@remote_server_ip
```
*(If a local passphrase was specified during key creation, enter it now).*
* **Expected Outcome:** You should instantly reach the secure remote shell workspace without being prompted for the server account password.

#### ⚙️ Subtask 3.2: Verify Connection
Run diagnostic commands to confirm you are executing shell environment scripts on the remote hardware host:
```bash
whoami    # Confirms remote context account username
hostname  # Displays the remote machine identity node name
```

> 💡 **Troubleshooting Tip:** If connection authorization fails, confirm file storage access controls are locked down securely on the remote server:
> ```bash
> chmod 700 ~/.ssh
> chmod 600 ~/.ssh/authorized_keys
> ```

---

## 🏁 Conclusion
You have successfully implemented a secure modern remote administration environment by:
* Creating unique high-entropy asymmetric cryptographic SSH keys.
* Transitioning server policies from weak password logins to key authentication protocols.
* Verifying passwordless remote control workflows across systems.

### 🚀 Next Steps
* Configure an authentication manager script via `ssh-agent` to store passphrases in volatile runtime memory.
* Explore **SSH Port Forwarding** techniques to safely tunnel application traffic streams.
