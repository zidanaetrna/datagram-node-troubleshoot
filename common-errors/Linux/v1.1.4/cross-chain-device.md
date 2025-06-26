# Datagram Node Error: Cross-Device Link Panic

**Problem Found and Resolved by:** aetrna

---

## ❗ Problem

This issue can occur across different types of Datagram installations — whether you're running the binary directly, using a systemd service, or deploying on a VPS using official install scripts.

Example error when attempting to run the node:

```bash
/datagram-cli-x86_64-linux run -- -key 96d.........d322d
```

Output:

```
App version: 1.1.4
Downloading...
panic: rename /tmp/download-25..41748.tmp /home/.datagram/conference/binaries/datagram-conference-cli-x86_64-linux: invalid cross-device link
```

---

## ✅ Solution

This issue is caused by a cross-device link error, and you can fix it with the following steps:

1. **Create a local temporary folder:**

```bash
mkdir -p ~/tmp && chmod +x ~/tmp
```

2. **Re-run the node using the custom `TMPDIR`:**

```bash
TMPDIR=~/tmp ./datagram-cli-x86_64-linux run -- -key YOUR_LICENSE_KEY
```

> Replace `YOUR_LICENSE_KEY` with your actual license key.

---

## 🔁 Make It Permanent

To avoid this issue in the future and not have to set `TMPDIR` every time:

1. Open your `~/.bashrc` file:

```bash
nano ~/.bashrc
```

2. Add this line at the very end:

```bash
export TMPDIR=~/tmp
```

3. Save the file by pressing:

**CTRL + X**, then **Y**, then **ENTER**

4. Apply the changes immediately:

```bash
source ~/.bashrc
```

Now, you can run the node without setting `TMPDIR` manually:

```bash
./datagram-cli-x86_64-linux run -- -key YOUR_LICENSE_KEY
```

This change is persistent and will also apply to:

- **Systemd installations**
- **Other types of datagram-cli setups**

For more details on installation types and environments:

- [Datagram VPS Server Setup Guide](https://doc.datagram.network/setup-datagram/partner-substrate-setup/vps-servers)
- [Datagram Local Ubuntu Setup Guide](https://doc.datagram.network/setup-datagram/partner-substrate-setup/local-machine-ubuntu-linux)

---

## 🧠 Why This Happens

The error:

```
invalid cross-device link
```

happens because the `/tmp` directory is often mounted on a **different filesystem** (like `tmpfs`) than your home directory. The `rename()` system call on Linux **does not work across filesystems** — it only supports moves within the same filesystem.

When the node attempts to move the downloaded binary from `/tmp` into your `~/.datagram` directory, it fails because they are on separate filesystems.

---

## 🔧 Why This Solution Works

By using `TMPDIR=~/tmp` (and making it permanent):

- You redirect all temporary files to a local folder within your home directory.
- This ensures **all moves and file operations happen within the same filesystem**.
- You avoid future `cross-device link` issues across all datagram-related commands.

---

## 📝 Notes

- This is a known Linux-specific issue.
- Affects many applications using temporary files + rename/move operations.
- Making `TMPDIR` persistent is a **best practice** for consistency across reboots, updates, or automated node relaunches.