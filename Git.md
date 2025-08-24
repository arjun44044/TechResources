# ---- sparse-checkout and --no-checkout

**Example--**

```bash
git clone --no-checkout <your-repo-url>
cd Project-Fitlab
git sparse-checkout init --cone
git sparse-checkout set Backend package.json package-lock.json
git checkout main
```

#### Explanation (line by line):

##### 1. `git clone --no-checkout <your-repo-url>`

* Normally, `git clone` downloads the repository **and checks out** all files into the working directory.
* Here, `--no-checkout` tells Git:
  * **Download the repo history & metadata** (the `.git` folder and objects).
  * **Don’t place any files into the working directory yet.**
* So after this, you have the repo but no actual files on disk.

>
> `git --no-checkout` isn’t a standalone Git command — it’s actually an **option** used with `git clone`.
>
> 👉 Full form:
>
> ```bash
> git clone --no-checkout <repo-url>
> ```
>
> ##### What it does:
>
> Normally, when you run `git clone <repo-url>`, Git:
>
> 1. Copies the repository’s **objects** (history, commits, branches, etc.) into a new `.git` directory.
> 2. Checks out the default branch (usually `main` or `master`) into your working directory, meaning all files appear on disk.
>
> But with `--no-checkout`, Git  **skips step 2** .
>
> That means:
>
> * The full repository history is cloned.
> * But no files are actually written into your working directory yet.
> * Your working directory remains empty (except `.git`).
>
> ##### Why use it?
>
> This is useful when:
>
> 1. **Partial checkouts** : You want the repo but don’t want all files right away (e.g., a huge repo, you’ll use sparse-checkout later).
> 2. **Custom workflows** : You want to immediately checkout a different branch, commit, or tag instead of the default.
> 3. **Automation/scripts** : Saves time and bandwidth by not doing an unnecessary initial checkout.
>
> ##### Example
>
> ```bash
> git clone --no-checkout https://github.com/example/repo.git
> cd repo
> ```
>
> At this point, the repo is cloned, but your directory looks empty:
>
> ```bash
> ls
> # nothing, except .git
> ```
>
> Now if you run:
>
> ```bash
> git checkout main
> ```
>
> It will then populate files.
>
> ✅ In short:
>
> `git clone --no-checkout` clones the repository metadata/history but **does not populate the working directory with files** until you explicitly checkout a branch.
>

##### 2. `cd Project-Fitlab`

* Moves into your project directory (created by `git clone`).

##### 3. `git sparse-checkout init --cone`

* Enables  **sparse checkout mode** .
* Sparse checkout means: you don’t need the full repo locally — you choose which folders/files you want.
* The `--cone` option makes it simpler: instead of complex patterns, you can specify just directories or files.

> #### `git sparse-checkout`
>
> Normally, when you `git clone` a repo, Git checks out the **entire working directory** with all files. But sometimes repos are  **huge** , and you only care about a subset (like one folder).
>
> **`git sparse-checkout`** allows you to **partially check out** only the paths/folders you need, while keeping the repo history intact.
>
> This is very useful for:
>
> * Monorepos (lots of unrelated projects in one repo).
> * Large repos where you only want a subdirectory.
> * Reducing disk space and speeding up operations.
>
> ##### 🔹 How it works
>
> 1. Enable sparse checkout mode:
>
>    ```bash
>    git sparse-checkout init
>    ```
>
>    → This tells Git you only want certain paths in the working directory.
> 2. Define the paths:
>
>    ```bash
>    git sparse-checkout set path/to/folder
>    ```
>
>    → Now only files from that folder will appear locally.
> 3. You can add/remove paths later:
>
>    ```bash
>    git sparse-checkout add another/folder
>    git sparse-checkout set new/folder   # replaces the previous ones
>    ```
>
> ##### 🔹 Common Flags
>
> ###### `git sparse-checkout init`
>
> * Initializes sparse-checkout mode.
> * By default, it uses the **cone mode** (faster + easier to specify top-level directories).
> * Flags:
>   * `--cone` → (default) optimized for directory-based patterns.
>   * `--no-cone` → allows arbitrary patterns but slower.
>
> Example:
>
> ```bash
> git sparse-checkout init --cone
> ```
>
> ###### `git sparse-checkout set`
>
> * Sets the sparse patterns (which folders/files to keep).
> * Replaces existing settings.
>
> Example:
>
> ```bash
> git sparse-checkout set src/ docs/
> ```
>
> → Only `src/` and `docs/` remain in the working directory.
>
> ###### `git sparse-checkout add`
>
> * Adds paths **without replacing** existing ones.
>
> Example:
>
> ```bash
> git sparse-checkout add tests/
> ```
>
> → Keeps `src/`, `docs/`, and now adds `tests/`.
>
> ###### `git sparse-checkout disable`
>
> * Turns off sparse-checkout.
> * Restores the **full working directory** (all files appear again).
>
> Example:
>
> ```bash
> git sparse-checkout disable
> ```
>
> ###### `git sparse-checkout list`
>
> * Shows which paths are currently included.
>
> Example:
>
> ```bash
> git sparse-checkout list
> ```
>
> ##### 🔹 Example Workflow
>
> ```bash
> # Clone repo without checkout
> git clone --no-checkout https://github.com/user/repo.git
> cd repo
>
> # Enable sparse checkout in cone mode
> git sparse-checkout init --cone
>
> # Checkout only the "backend" folder
> git sparse-checkout set backend/
>
> # Add "frontend" folder later
> git sparse-checkout add frontend/
> ```
>
> Now, only `backend/` and `frontend/` exist locally. The rest is hidden.
>

##### 4. `git sparse-checkout set Backend package.json package-lock.json`

* This command tells Git  **which paths you want in your working directory** .
* In your case:
  * `Backend/` folder
  * `package.json`
  * `package-lock.json`
* Git will now ensure **only these files/folders are visible and downloaded** to your machine.
* The rest of the repo stays hidden (but still exists in history inside `.git`).

##### 5. `git checkout main`

* Finally, you checkout the branch `main`.
* Git will now **download and place only the sparse checkout paths** into your working directory (not the entire repo).
* So, you end up with a **lightweight local copy** that only has:
  * `Backend/`
  * `package.json`
  * `package-lock.json`

#### ⚡ Why use this?

* If your repo is large (say it has Frontend, Backend, docs, infra, etc.) but you only want to work on  **Backend** , you don’t waste time, bandwidth, or disk downloading the entire repo.
* Super useful in **CI/CD pipelines** (like GitLab runner on EC2), where you want to fetch only required files.

✅ End Result:

Your local project folder will look like this:

```
Project-Fitlab/
  ├── Backend/
  ├── package.json
  ├── package-lock.json
```

---
