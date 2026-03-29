# 🧹 Mac Storage Cleanup & Optimization Guide (Developer Edition)

A complete, safe, and practical guide to reclaim disk space on macOS — especially useful for developers dealing with large projects, caches, and system data bloat.

---

## 📊 Problem Overview

If your Mac shows:

* ⚠️ Storage almost full
* ⚠️ “System Data” taking **100+ GB**
* ⚠️ Slow performance / lag

Then the issue is usually caused by:

* Cache accumulation
* Developer artifacts (`node_modules`, builds, logs)
* Hidden system files
* Spotlight indexing

---

## 🧠 Understanding “System Data”

“System Data” is not a single thing. It includes:

* Cache files
* Logs
* Temporary files
* Xcode / developer data
* iOS backups
* Spotlight index
* App support files

👉 This is why it can grow very large.

---

## 🔍 Step 1: Analyze Disk Usage (IMPORTANT)

### Find largest folders in your home directory:

```bash
sudo du -hxd1 ~ | sort -hr | head -20
```

### Find largest folders system-wide:

```bash
sudo du -hxd1 / | sort -hr | head -20
```

👉 Always analyze before deleting blindly.

---

## 🧹 Step 2: Clean Cache Files

### Remove user cache:

```bash
rm -rf ~/Library/Caches/*
```

If permission issues:

```bash
sudo rm -rf ~/Library/Caches/*
```

---

## 🧹 Step 3: Remove Developer Junk (BIG SPACE SAVER)

### Delete all `node_modules`:

```bash
find ~ -type d -name "node_modules" -prune -exec rm -rf {} +
```

👉 Safe: removes only dependencies (reinstallable)

---

### Delete Python virtual environments:

```bash
find ~ -type d \( -name "venv" -o -name ".venv" \) -prune -exec rm -rf {} +
```

---

### Clear npm & cache:

```bash
npm cache clean --force
rm -rf ~/.npm
rm -rf ~/.cache
```

---

## 🧹 Step 4: Clean Xcode & iOS Data

```bash
rm -rf ~/Library/Developer/Xcode/DerivedData
rm -rf ~/Library/Developer/Xcode/Archives
rm -rf ~/Library/Developer/Xcode/iOS\ DeviceSupport
```

---

### Remove iOS backups:

```bash
rm -rf ~/Library/Application\ Support/MobileSync/Backup/*
```

---

## 🧹 Step 5: Clean Logs & Temp Files

```bash
rm -rf ~/Library/Logs/*
sudo rm -rf /private/var/log/*
sudo rm -rf /private/var/tmp/*
```

---

## 🧹 Step 6: Remove VS Code Cache

```bash
rm -rf ~/Library/Application\ Support/Code/Cache
rm -rf ~/Library/Application\ Support/Code/CachedData
```

---

## 🧹 Step 7: Remove Leftover App Data (Example: Browser)

```bash
rm -rf ~/Library/Application\ Support/<AppName>
rm -rf ~/Library/Preferences/com.<app>.plist
rm -rf ~/Library/Logs/<AppName>
```

---

## 🔄 Step 8: Rebuild Spotlight Index

```bash
sudo mdutil -E /
```

👉 Fixes incorrect “System Data” reporting

---

## 🔁 Step 9: Restart Your Mac

⚠️ Required — macOS recalculates storage **only after reboot**

---

## 📈 Expected Results

After cleanup:

* ✅ 20–100+ GB freed
* ✅ “System Data” reduced significantly
* ✅ Faster system performance
* ✅ Cleaner development environment

---

## ⚠️ Safety Notes

### Safe to delete:

* `node_modules`
* caches
* logs
* temp files
* build artifacts

### Will require reinstall:

```bash
npm install
```

---

### NOT affected:

* macOS system files
* personal files (docs, photos)
* applications

---

## 🧠 Best Practices (Going Forward)

* Clean `node_modules` periodically
* Avoid storing large projects in `Downloads`
* Use `.gitignore` properly
* Clear caches monthly
* Monitor storage with:

```bash
du -sh *
```

---

## 🚀 Pro Tip (Optional Script)

Create a quick cleanup script:

```bash
#!/bin/bash
rm -rf ~/Library/Caches/*
rm -rf ~/.npm
rm -rf ~/.cache
find ~ -type d -name "node_modules" -prune -exec rm -rf {} +
echo "Cleanup complete!"
```

---

## 🎯 Conclusion

Most macOS storage issues (especially “System Data”) are caused by:

* Developer environments
* Cached data
* Logs and temporary files

With the steps above, you can safely:

* Reclaim massive storage
* Keep your system clean
* Maintain a fast development workflow

---

💡 *Tip: Run this cleanup every few weeks if you're actively coding.*
