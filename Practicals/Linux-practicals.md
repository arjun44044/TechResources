# 🧮 **"How to count the number of files and folders in your home directory?"**

Here are a few easy ways:

##### ✅ **1. Count only files and directories (not recursively):**

```bash
ls -1 ~ | wc -l
```

**Explanation:**

* `ls -1 ~` lists all items in your home directory, one per line.
* `wc -l` counts the number of lines → hence, the number of items.

##### ✅ **2. Count all files and directories recursively (including inside subfolders):**

```bash
find ~ | wc -l
```

This includes:

* every file,
* every folder,
* and even symbolic links, recursively.

##### ✅ **3. Count only files (recursively):**

```bash
find ~ -type f | wc -l
```

##### ✅ **4. Count only directories (recursively):**

```bash
find ~ -type d | wc -l
```

---

# 🧮 **"Move `file1.txt` to `SubFolder`."**

🔴 Problem:

You ran:

```bash
mv 'file1.txt' /SubFolder
```

This tells Linux:

> "Move `file1.txt` into the directory `/SubFolder` at the **root** of the filesystem (`/`)".

And since your user  **doesn't have write permission to `/`** , it says:

> `Permission denied`

### ✅ Solution:

Use the **relative path** instead of the  **absolute path** .

You are already in:

```bash
~/Documents/practise
```

So just run:

```bash
mv file1.txt SubFolder/
```

or with full relative path:

```bash
mv ./file1.txt ./SubFolder/
```

### 🧠 Tip:

* `/SubFolder` → absolute path at the **root**
* `SubFolder/` → relative path in your **current folder**

---

# 🧮"Create a file with  **today’s date in its filename"**

✅ Command:

```bash
touch "report-$(date +%F).txt"
```

### 🔍 Explanation:

* `touch` → creates an empty file
* `$(date +%F)` → inserts today’s date in `YYYY-MM-DD` format (e.g., `2025-07-17`)
* The whole command becomes:

```bash
touch "report-2025-07-17.txt"
```

This creates a file like:

```
report-2025-07-17.txt
```

### 🧠 Bonus Formats:

* `$(date +%Y-%m-%d)` → same as `%F` → `2025-07-17`
* `$(date +%d-%m-%Y)` → `17-07-2025`
* `$(date +%Y%m%d_%H%M%S)` → for unique timestamps → `20250717_194300`

---

# 🧮 "Print even numbers from **1 to 20** using a `while` loop"

```bash
#!/bin/bash

# Print even numbers from 1 to 20 using a while loop
num=1
echo "Printing even numbers from 1 to 20 using a while loop"

while [[ $num -le 20 ]]; do
    if (( num % 2 == 0 )); then
        echo $num
    fi
    ((num++))
done

```
