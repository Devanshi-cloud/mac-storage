# 🧠 Mac Storage Cleanup Journey (UTM, System Data & Dev Cleanup)

A practical walkthrough of how I diagnosed and fixed massive storage usage on my Mac — including what I learned, what commands I used, and how I safely freed **80+ GB**.

---

## 🚨 Initial Problem

* Mac storage almost full
* **System Data ~135 GB** ❌
* Overall usage ~243 GB / 245 GB

👉 System was slow and cluttered

---

## 🔍 Step 1: Understanding the Problem

I learned that **“System Data”** includes:

* Cache files
* Logs
* Developer data
* Virtual machines
* App containers

👉 It’s not one thing — it’s a mix of hidden storage.

---

## 🔎 Step 2: Finding Large Folders

Used this command to identify what’s taking space:

```bash
sudo du -hxd1 ~ | sort -hr | head -20
```

### 📊 Key Findings:

* `~/Library` → **55 GB**
* `.gradle` → 5.5 GB
* `.cache` → 2.2 GB
* `.vscode` → 5.7 GB

👉 Conclusion: Hidden system + dev files were the issue.

---

## 🔎 Step 3: Deep Dive into Library

```bash
du -hxd1 ~/Library | sort -hr | head -20
```

### Found:

* `Containers` → **28 GB**
* `Application Support` → **17 GB**

---

## 🔎 Step 4: Identify Exact Culprits

```bash
du -hxd1 ~/Library/Containers | sort -hr | head -15
```

### 🚨 Major storage consumers:

* UTM → **17 GB**
* Docker → **9.1 GB**

---

## 🧠 What I Learned About UTM

Inside UTM I found:

```bash
ls ~/Library/Containers/com.utmapp.UTM/Data/Documents
```

Output:

* `macOS.utm`
* `macOS 2.utm`

👉 These are **full virtual macOS systems**
👉 Each `.utm` file = complete OS image

💡 That’s why it was taking **17 GB**

---

## 🧹 Step 5: Cleanup Actions

### ✅ 1. Removed all `node_modules`

```bash
find ~ -type d -name "node_modules" -prune -exec rm -rf {} +
```

✔️ Deleted dependencies (safe, reinstallable)

---

### ✅ 2. Cleared system caches

```bash
rm -rf ~/Library/Caches/*
sudo rm -rf ~/Library/Caches/*
```

---

### ✅ 3. Cleaned developer caches

```bash
rm -rf ~/.gradle/caches
rm -rf ~/.cache/*
rm -rf ~/.npm
```

---

### ✅ 4. Cleaned VS Code cache

```bash
rm -rf ~/Library/Application\ Support/Code/Cache
rm -rf ~/Library/Application\ Support/Code/CachedData
```

---

### ✅ 5. Cleaned container caches

```bash
rm -rf ~/Library/Containers/*/Data/Library/Caches/*
```

---

### ✅ 6. Removed UTM virtual machines (BIG WIN)

```bash
rm -rf ~/Library/Containers/com.utmapp.UTM
```

💥 Freed ~17 GB instantly

---

### ✅ 7. Optional Docker cleanup

```bash
docker system prune -a --volumes
```

---

## 🔄 Step 6: Fix System Data Calculation

```bash
sudo mdutil -E /
```

👉 Rebuilt Spotlight index
👉 Fixed incorrect storage reporting

---

## 🔁 Step 7: Restart

👉 Important for macOS to recalculate storage

---

## 📉 Final Results

| Metric      | Before  | After   |
| ----------- | ------- | ------- |
| Total Used  | ~243 GB | ~194 GB |
| Free Space  | ~2 GB   | ~50+ GB |
| System Data | ~135 GB | ~56 GB  |

---

## 🧠 Key Learnings

### 💡 1. System Data is misleading

* It hides many types of files
* Needs manual investigation

---

### 💡 2. Virtual machines are heavy

* Tools like UTM store full OS images
* Can silently consume **10–20+ GB**

---

### 💡 3. Developer environments bloat quickly

* `node_modules`
* `.gradle`, `.cache`
* Logs and temp files

---

### 💡 4. Always analyze before deleting

```bash
du -hxd1 <path>
```

---

### 💡 5. Cache cleanup is safe

* Everything regenerates automatically

---

## ⚠️ Safety Rules I Followed

* ❌ Did NOT delete system folders blindly
* ❌ Did NOT remove entire `Library`
* ✅ Targeted only large, safe-to-delete items
* ✅ Took backups when unsure

---

## 🧹 Maintenance Routine (Going Forward)

### Monthly:

```bash
rm -rf ~/Library/Caches/*
rm -rf ~/.cache
rm -rf ~/.npm
```

---

### Dev cleanup:

```bash
find ~ -type d -name "node_modules" -prune -exec rm -rf {} +
```

---

## 🎯 Conclusion

This cleanup helped me:

* 🚀 Free **80+ GB space**
* ⚡ Improve system performance
* 🧠 Understand macOS storage deeply

---

💡 *Biggest takeaway: Most storage issues come from hidden developer data and unused tools — not actual files.*
