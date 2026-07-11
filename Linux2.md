# ----💻 Command - `rsync`

**`rsync`** is one of the most powerful and commonly used command-line tools in Linux/Unix systems for **synchronizing and transferring files** between locations (local-to-local or local-to-remote over SSH). Let me break it down fully for you:

#### 🔹 What is `rsync`?

* **rsync** = "remote sync"
* It copies and synchronizes files/directories efficiently.
* It only transfers **differences** between source and destination instead of copying everything again (using a delta-transfer algorithm).
* Often used for  **backups, deployments, and mirroring** .

#### 🔹 Basic Syntax

```bash
rsync [options] source destination
```

Examples:

```bash
# Copy a file to another directory
rsync file.txt /backup/

# Copy a directory recursively
rsync -r myproject/ /backup/myproject/

# Copy from local to remote over SSH
rsync -avz myproject/ user@server:/var/www/myproject/
```

#### 🔹 Commonly Used Options

* `-a` → **archive mode** (preserves permissions, symlinks, timestamps, etc.)
* `-v` → **verbose** (shows details of what’s happening)
* `-z` → **compress** data during transfer (faster over network)
* `-r` → **recursive** (for directories)
* `-P` → shows **progress** and allows resuming partial transfers
* `--delete` → deletes files in the destination if they don’t exist in source (useful for syncing)

Example:

```bash
rsync -avzP --delete myproject/ user@server:/var/www/myproject/
```

✅ This will make sure the remote directory matches exactly the local one.

#### 🔹 Key Features

1. **Delta Transfer Algorithm**

   Transfers only the changed parts of files, saving time and bandwidth.
2. **Resuming Transfers**

   If interrupted, you can rerun the command, and it will pick up where it left off.
3. **Preserves Metadata**

   File ownership, permissions, timestamps, etc.
4. **Local and Remote Support**

   Works locally (copying directories on the same system) or remotely via SSH.

#### 🔹 Use Cases

* **Backups**
  ```bash
  rsync -a /home/user/ /mnt/backup/
  ```
* **Deployment of web apps**
  ```bash
  rsync -avzP ./build/ user@server:/var/www/myapp/
  ```
* **Mirroring Directories**
  ```bash
  rsync -av --delete /source/ /destination/
  ```
* **Copy files between servers directly**
  ```bash
  rsync -avz user1@server1:/data/ user2@server2:/backup/
  ```

#### 🔹 Quick Comparison with `scp`

* `scp` (secure copy) → always copies entire files.
* `rsync` → smarter, copies only changed parts, supports resume, more efficient for sync.

---

# ----💻 Command - `ssh-agent`

`ssh-agent -s` is a command used in **SSH key authentication** setups. Let me break it down for you:

#### 🔹 What is `ssh-agent`?

* `ssh-agent` is a background program that  **stores your private SSH keys in memory** .
* It allows you to authenticate with SSH servers  **without having to type your passphrase every single time** .
* You load your private key into the agent using `ssh-add`.

#### 🔹 What does the `-s` option do?

* `-s` means  **output commands for the Bourne shell (`sh`, `bash`, `zsh`)** .
* When you run:

  ```bash
  eval $(ssh-agent -s)
  ```

  it:

  1. Starts `ssh-agent` in the background.
  2. Prints environment variable exports in shell syntax, like:
     ```bash
     SSH_AUTH_SOCK=/tmp/ssh-abc123/agent.1234; export SSH_AUTH_SOCK;
     SSH_AGENT_PID=12345; export SSH_AGENT_PID;
     echo Agent pid 12345;
     ```
  3. `eval` executes these export commands in your current shell, so your shell knows how to communicate with the agent.

#### 🔹 Typical Workflow

1. Start the agent:
   ```bash
   eval $(ssh-agent -s)
   ```
2. Add your key:
   ```bash
   ssh-add ~/.ssh/id_rsa
   ```
3. Connect without entering passphrase every time:
   ```bash
   ssh user@server.com
   ```

#### 🔹 Related Options

* `ssh-agent -c` → outputs commands for  **C shell (csh, tcsh)** .
* `ssh-agent -k` → kills the agent and cleans up environment variables.

#### ✅  **In short** :

`ssh-agent -s` starts the SSH agent and sets up environment variables (like `SSH_AUTH_SOCK` and `SSH_AGENT_PID`) so your shell can talk to the agent and use stored SSH keys without repeatedly entering passphrases.

---

# ----💻 Command - `ssh-keyscan`

Example--

```bash
ssh-keyscan -H $EC2_HOST >> ~/.ssh/known_hosts
```

#### 1. `ssh-keyscan`

* A utility that fetches the **public SSH host keys** of a remote server.
* Normally, when you connect to a server for the first time with `ssh`, it asks:

  *“The authenticity of host can't be established... Do you want to continue?”*
* `ssh-keyscan` avoids that manual prompt by grabbing the server’s host key automatically.

#### 2. `-H`

* This tells `ssh-keyscan` to **hash the hostnames** in the output.
* Why? For **privacy/security** — so if someone looks inside your `~/.ssh/known_hosts`, they can’t directly see which servers you connect to.
* Without `-H`, the file will contain plain hostnames like `ec2-11-22-33-44.compute.amazonaws.com`.
* With `-H`, they appear as hashed strings.

#### 3. `$EC2_HOST`

* This is an **environment variable** holding the hostname or IP address of your EC2 instance.

  Example:

  ```bash
  export EC2_HOST=ec2-11-22-33-44.compute.amazonaws.com
  ```

#### 4. `>> ~/.ssh/known_hosts`

* The `>>` appends the fetched host key into your  **SSH known hosts file** .
* That file (`~/.ssh/known_hosts`) is where SSH keeps fingerprints of all the servers you’ve connected to before.
* Next time you run:

  ```bash
  ssh -i key.pem ubuntu@$EC2_HOST
  ```

  SSH will check this file and **won’t ask for confirmation** — because it already recognizes the server’s host key.

#### ✅ **In simple terms:**

This command tells your system:

*"Here’s the server’s fingerprint. Trust it from now on, so when I connect via SSH, don’t bother me with security confirmation prompts."*

---
