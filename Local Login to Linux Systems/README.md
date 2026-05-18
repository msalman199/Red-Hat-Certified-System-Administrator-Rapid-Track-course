# 🖥️ Local Login to Linux Systems

## 🎯 Objectives
By the end of this lab, you will be able to:
* 🔌 Access a Linux system physically via terminal or console.
* 🔑 Log in using a standard username and password.
* 🔄 Explore and switch between multiple virtual terminals.

## 📋 Prerequisites
* 💻 A physical or virtual machine running a Linux distribution (e.g., Fedora, CentOS, Ubuntu).
* 👤 A non-root user account with a configured password.
* 🐚 Basic familiarity with command-line interfaces.

---

## 🛠️ Lab Tasks

### 📂 Task 1: Access the Linux System Physically

#### ⚙️ Subtask 1.1: Power On the System
* Turn on your hardware system. 
* If you are utilizing a virtual machine manager (e.g., VirtualBox, KVM, VMware), choose your instance and click start.

#### ⚙️ Subtask 1.2: Reach the Login Prompt
* Allow the system to complete its boot cycle to display the login screen.
* If your system defaults directly into a graphical login screen, hit `Ctrl` + `Alt` + `F2` to break out and drop down into a pure text-based terminal session (`tty2`).

* **Expected Outcome:** You should see a text-only system login prompt looking similar to this:
  ```text
  Fedora 38 (Workstation Edition)  
  Kernel 6.2.15-200.fc38.x86_64 on an x86_64 (tty2)  

  localhost login: _  
  ```

> 💡 **Troubleshooting Tip:** If your environment lands on a GUI interface but you strictly require terminal-only access, press keys `Ctrl` + `Alt` + `F2` through `F6` to map into alternate virtual consoles.

---

### 📂 Task 2: Log In Using Username and Password

#### ⚙️ Subtask 2.1: Enter Username
At the active blinking terminal prompt, type your account name and press `Enter`:
```text
localhost login: user1
```

#### ⚙️ Subtask 2.2: Enter Password
Type your account password when prompted and hit `Enter`:
```text
Password: 
```
*(Note: As a security measure, the console will not print or echo any asterisk or placeholder characters while you type your password).*

* **Expected Outcome:** A successful login clears the system banner message and yields access to the bash shell prompt:
  ```bash
  [user1@localhost ~]$  
  ```

> 💡 **Troubleshooting Tip:** If authentication drops or errors out, double-check your spelling or Cap Lock orientation. If your profile becomes completely locked out, you must request a system root administrator to manually clear or reset your credentials.

---

### 📂 Task 3: Explore Virtual Terminals

#### ⚙️ Subtask 3.1: Switch Between Virtual Terminals
Linux kernels traditionally open up multiple parallel local virtual terminals (`tty1` through `tty6`).
* Press `Ctrl` + `Alt` + `F3` to jump directly into console `tty3`.
* Complete your authentication parameters again if requested on the clean console interface.
* Return right back to your prior primary task session by executing `Ctrl` + `Alt` + `F2`.

#### ⚙️ Subtask 3.2: Check Active Terminal
Verify exactly which standard file-descriptor console path your login session holds by typing:
```bash
tty  
```
* **Expected Output:**
  ```text
  /dev/tty2  
  ```

#### ⚙️ Subtask 3.3: List Logged-in Users
To dump a summary tracking index of all active established local terminal connections, run:
```bash
who  
```
* **Expected Output:**
  ```text
  user1   tty2         2026-05-18 14:30  
  user1   tty3         2026-05-18 14:35  
  ```

> 💡 **Troubleshooting Tip:** If shifting consoles via hotkeys fails to change screens, ensure your local configuration supports standard terminal mapping (though nearly all vanilla Linux instances have this default behavior enabled out of the box).

---

## 🏁 Conclusion
By completing this practical exercise, you have mastered:
* Interacting with a Linux terminal environment natively via local console interfaces.
* Handling hidden security fields and managing credential responses during initialization.
* Moving between background execution spaces using active system virtual terminal lanes (`tty`).

These fundamental skills are important steps toward mastering local Linux server configuration, administration, and bare-metal troubleshooting methodologies.

### 🚀 Next Steps
* Learn to access the terminal over a local or remote network using an **SSH client connection**.
* Learn to navigate through core storage pathways using standard file utilities (`ls`, `pwd`, `cd`).
