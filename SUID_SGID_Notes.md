# 🔐 **Linux Permissions - SUID and SGID Concepts**

## 🛠️ **SUID (Set User ID)**

**What is SUID?**  
Imagine you have a toolbox that only you (as the owner) have the key to. Now, instead of giving the key to someone else, you let them use the tools in the box when they need them. That's what **SUID** (Set User ID) does—it lets a regular user **use a program with the permissions of the program's owner** (usually root) without giving them full access to everything.

### **Real-World Example: `passwd` Command**
- The `passwd` command is like a locked door (the `/etc/shadow` file) that only the admin (root) can open.
- Regular users can't modify this file.
- **But**—with **SUID** set on `passwd`, any user can run the command to change their password, because the program temporarily acts like the admin.

### **How is SUID Different from `visudo`?**
- **`visudo`** lets you use a program as root, but you still need to **authenticate** (like using a password to access something important).
- **SUID** makes a program always run with the admin’s powers, **without asking for any password**.

### **How to Set SUID:**
1. **Create a script:**
   You first create a script (think of it like a short instruction manual).
   ```bash
   # touch script.sh
   ```

2. **Set SUID on the script:**
   You then give that script a special permission to always run as root (even though you are not root).
   ```bash
   # chmod u+s script.sh
   ```

3. **Remove SUID:**
   If you want to stop this privilege, you can remove it like this:
   ```bash
   # chmod u-s script.sh
   ```

## 🔒 **SGID (Set Group ID)**

**What is SGID?**  
Imagine you're in a team at work, and the whole team has access to certain tools (like a shared computer). With **SGID**, whenever someone in your team uses a program, it doesn’t use their personal permissions—it uses **the team's** permissions.

### **Real-World Example:**
- You create a script called `test_script.sh` (like a shared document for the team).
- If SGID is set, anyone from your team (group) who runs it will use the team's permissions, not just their own.

### **How SGID Works on Files and Directories:**
- **On Files:** When someone runs a file, it runs with the **group's permissions**, not the individual user's.
- **On Directories:** If SGID is set on a folder, anything created inside that folder will **belong to the group**, not the individual creator.

### **Real-World Example of SGID on Files:**
1. **Create a group and add users to it** (like setting up a new team):
   ```bash
   # sudo groupadd devgroup
   ```

2. **Create a script (`test_script.sh`) and give it to the group**:
   ```bash
   # vi test_script.sh
   ```
   Inside this script, you can write a simple message like:
   ```bash
   #!/bin/bash
   echo "This script is being executed by the group: $(groups)"
   ```

3. **Give execute permissions** (so the team can use it):
   ```bash
   # chmod +x test_script.sh
   ```

4. **Set SGID on the file:**
   This tells the system, "Whoever runs this script, run it with the group’s permission."
   ```bash
   # chmod g+s test_script.sh
   ```

5. **Now, test with users**:
   > Even though user1 and user2 run the script, it will run with the permissions of `devgroup`.

### **SGID on Directories:**

1. **Create a shared folder for the team:**
   ```bash
   # mkdir shared_dir
   ```

2. **Set SGID on that folder:**
   ```bash
   # chmod g+s shared_dir
   ```

3. **Create a file inside the folder**:
   When a user creates a file in this folder, it will belong to the team group, not the individual user.

### **Summary of Key Concepts:**
- **SUID on files**: Runs programs with **admin’s permissions** (only for that program).
- **SGID on files**: Runs programs with the **group's permissions**.
- **SGID on directories**: Makes sure **new files in the directory belong to the group** (not the individual user).
