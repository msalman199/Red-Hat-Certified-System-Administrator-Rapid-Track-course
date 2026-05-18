# 📂 File and Directory Management

## 🎯 Objectives
By the end of this lab, you will be able to:
* 🛠️ Manage files and directories with confidence using core command-line tools.
* 💾 Create, delete, move, and copy files and directories cleanly.
* 🧭 Use relative and absolute paths effectively for precise file manipulation.

## 📋 Prerequisites
* **Operating System:** A Linux-based operating system (Ubuntu, CentOS, or Fedora recommended).
* **Skills:** Basic familiarity with a command-line interface.
* **Access:** Terminal application access.

---

## 🏗️ Setup Requirements
Open a terminal window and establish a dedicated testing workspace:
```bash
# Verify your current working directory location
pwd

# Create a dedicated laboratory directory and switch into it
mkdir file_management_lab
cd file_management_lab
```

---

## 🛠️ Lab Tasks

### 📂 Task 1: Create and Delete Files and Directories

#### ⚙️ Subtask 1.1: Creating Files and Directories
Generate a new organizational folder:
```bash
mkdir documents
```
Create three empty storage files simultaneously using the `touch` utility:
```bash
touch file1.txt file2.txt file3.txt
```
Verify the successful generation of your assets:
```bash
ls -l
```
* **Expected Output:** The directory stream prints the new `documents/` directory alongside the three `.txt` tracking files.

#### ⚙️ Subtask 1.2: Deleting Files and Directories
Remove a standalone file block from storage:
```bash
rm file3.txt
```
Attempt to purge the `documents` folder using the basic directory removal utility:
```bash
rmdir documents
```
> 💡 **Troubleshooting Tip:** The `rmdir` command will intentionally fail here because directories must be completely empty before removal.

To wipe out the directory along with any files stored inside it, apply a recursive force instruction:
```bash
rm -r documents
```
* **🧠 Key Concept:** The `-r` (or `--recursive`) flag instructs the file utility engine to dive down and remove all internal contents and subdirectories sequentially.

---

### 📂 Task 2: Move and Copy Files

#### ⚙️ Subtask 2.1: Moving Files
Re-initialize the mock workspace structure for testing:
```bash
mkdir documents backups
touch file1.txt file2.txt
```
Relocate `file1.txt` directly into the `documents` subdirectory path:
```bash
mv file1.txt documents/
```
Verify that the file changed locations successfully:
```bash
ls documents/
```

#### ⚙️ Subtask 2.2: Copying Files
Duplicate `file2.txt` and place the copy inside the `backups` directory folder:
```bash
cp file2.txt backups/
```
Generate an independent mirror copy inside your current working directory:
```bash
cp file2.txt file2_backup.txt
```
Verify that both copies exist across your storage folders:
```bash
ls backups/ && ls
```

---

### 📂 Task 3: Using Relative and Absolute Paths

#### ⚙️ Subtask 3.1: Relative Path Navigation
Navigate down into the `documents` folder using a relative path pathing statement:
```bash
cd documents
```
Step backward exactly one level to return to your parent directory space:
```bash
cd ..
```
List the contents of the `backups` folder relatively from your current anchor position:
```bash
ls backups/
```

#### ⚙️ Subtask 3.2: Absolute Path Operations
Determine the absolute, full-length system route of your current directory node:
```bash
pwd
# Example Output format: /home/user/file_management_lab
```
Generate a file explicitly by specifying its full absolute system pathway:
```bash
touch /home/user/file_management_lab/absolute_example.txt
```
Copy that file between directories by explicitly defining both absolute routes:
```bash
cp /home/user/file_management_lab/absolute_example.txt /home/user/file_management_lab/backups/
```

---

## 🚀 Advanced Exercise
Generate a complex, nested project tree hierarchy using a single structured path command:
```bash
mkdir -p projects/{src,doc,bin}
```
Bulk-move all remaining loose `.txt` documents directly into the nested `doc` directory:
```bash
mv *.txt projects/doc/
```
Verify the structural alignment mapping on your system screen:
```bash
tree
```
> 💡 **Troubleshooting Tip:** If your environment throws a `tree: command not found` error, quickly pull down the missing open-source visualization package:
> ```bash
> sudo apt install tree     # Debian / Ubuntu
> sudo dnf install tree     # RHEL / CentOS / Fedora
> ```

---

## 🏁 Conclusion
By completing these filesystem exercises, you have mastered:
* Creating, organizing, and recursively removing files and subdirectories.
* Relocating or mirroring files using core storage utilities (`mv`, `cp`).
* Applying target modifications accurately using relative shortcuts and absolute system pathways.

### 🔍 Final Verification
To audit your full directory structure and verify task completions, run:
```bash
find . -type d -print
```

### 🧹 Cleanup
To clean up your workspace and remove all testing directories, step backward and delete the laboratory folder:
```bash
cd ..
rm -rf file_management_lab
```

### 📚 Additional Resources
* 📖 [Official GNU Coreutils Manual Resource](https://www.gnu.org/software/coreutils/manual/)
* 🌐 [Linux Filesystem Hierarchy Standard Reference (FHS)](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html)
