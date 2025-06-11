# timeshift-helyreallitas
Timeshift snapshot törlés, visszaállítás és rendszerkarbantartás lépésenként

# Timeshift Snapshot Recovery & Cleanup Guide (Linux)

This repository contains a practical, step-by-step guide for Linux system administrators to safely handle Timeshift snapshot issues — especially when a deletion is incomplete, fails silently, or when no snapshots are found.

---

## What This Covers

- How to detect and remove partial or ghost snapshots
- How to verify Timeshift snapshot existence and configuration
- How to reset and reinitialize Timeshift
- Best practices for disk cleanup and safety

---

## Step-by-Step Recovery Workflow

### 1. Check if any snapshot directory exists:

```bash
sudo find / -type d -name snapshots 2>/dev/null
```

Expected path (if exists): `/timeshift/snapshots/`

---

### 2. Check if Timeshift is actively running:

```bash
ps aux | grep timeshift
```

If a process is running (e.g., `/usr/bin/timeshift --delete ...`), kill it:

```bash
sudo kill -9 [PID]
```

---

### 3. Check for lock or PID file:

```bash
ls /var/run/timeshift/
```

If any file exists there (like `timeshift.pid`), remove it:

```bash
sudo rm -rf /var/run/timeshift/*
```

---

### 4. Confirm snapshot mount config (optional):

```bash
cat /etc/fstab | grep timeshift
```

---

### 5. Clean up system disk space:

```bash
sudo apt clean
sudo apt autoremove
sudo journalctl --vacuum-time=7d
```

---

### 6. Reinitialize Timeshift:

```bash
sudo timeshift --check
sudo timeshift --create --comments "Fresh snapshot after cleanup" --tags D
```

---

## Outcome

After these steps, your system should:

- Have no leftover snapshots or Timeshift locks
- Regain disk space
- Be ready to use Timeshift again cleanly and safely

---

## Repo Purpose

This repo serves as a **reference and quick-reaction guide** for real-life Linux admin troubleshooting scenarios where Timeshift behaves unexpectedly.

---

## How to Use This Repo on GitHub

1. Create a new repo on GitHub:
   - Name: `timeshift-recovery-guide`
   - Description: `Step-by-step Linux Timeshift snapshot cleanup and recovery`

2. Clone your new GitHub repo locally:
   ```bash
   git clone https://github.com/YOUR_USERNAME/timeshift-recovery-guide.git
   cd timeshift-recovery-guide
   ```

3. Place this `README.md` in the folder and push:
   ```bash
   git add README.md
   git commit -m "Initial commit: Timeshift recovery guide"
   git push origin main
   ```

4. Done! You now have a professional-grade Timeshift recovery guide on your GitHub.

---
