# Vim Editor

Let's break down  **Vim** , one of the most powerful and widely-used text editors on Unix/Linux systems, especially useful in server environments and scripting workflows.

## ---------- Vim-1 ----------

### 🧠 What Is Vim?

**Vim** (Vi IMproved) is an advanced text editor based on the original **Vi** editor found on Unix systems. It is:

* Terminal-based (keyboard-driven)
* Lightweight but extremely powerful
* Perfect for editing config files, code, and shell scripts

### 🚦 Basic Modes in Vim

Vim is  **modal** , meaning it operates in different modes:

| Mode                      | Purpose                                    | How to Enter                                  |
| ------------------------- | ------------------------------------------ | --------------------------------------------- |
| 🟢**Normal mode**   | Default mode for navigation & commands     | Press `Esc`                                 |
| ✏️**Insert mode** | Used to insert (type) text                 | Press `i`,`I`,`a`, `A`, `o`, `O` |
| 🔍**Visual mode**   | Used to select text                        | Press `v`,`V`, or `Ctrl+v`              |
| 💻**Command mode**  | Used to run commands (`:w`,`:q`, etc.) | Type `:`from Normal mode                    |

### 🚀 Getting Started with Vim

🔹 Open a file:

```bash
vim filename.txt
```

🔹 Enter Insert Mode (to type):

* Press `i` → (insert before cursor)
* Press `I` → (insert at the begining of that line)
* Or `a` → (insert after cursor)
* Or `A` → (insert at the end of  that line)
* Or `o` → (insert newline BELOW (Opens newline)) ie the next line would be empty to write
* Or `o` → (insert newline ABOVE (Opens newline)) ie the next line would be empty to
* Then type as usual

🔹 Return to Normal Mode:

* Press `Esc`

### 💾 Basic Commands (From Normal Mode):

| Action              | Command                                                                             |
| ------------------- | ----------------------------------------------------------------------------------- |
| Save                | `:w`                                                                              |
| Quit                | `:q` If anywthing written it must be discraded and hence used `q!` in that case |
| Save & Quit         | `:wq`or `ZZ`                                                                    |
| Quit without saving | `:q!`                                                                             |
| Open file (new)     | `:e filename`                                                                     |
| View open buffers   | `:ls`or `:buffers`                                                              |
| Switch buffer       | `:b2`(buffer #2)                                                                  |

### 🧭 Navigation

| Movement                 | Command                                                                                                                                                                                                   |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Move cursor              | Arrow keys or `h` (left) `l` (right) `k` (up) `j` (down)                                                                                                                                       |
| Beginning of line        | `0`                                                                                                                                                                                                     |
| End of line              | `$`                                                                                                                                                                                                     |
| Next word                | `w` (`W` is for next word even if _or- , etc present in between that word<br /> coz `w` won't consider _or- , etc as part of a single word rather thinks that its the begining of the next word)   |
| Previous word            | `b` (` B` is for prev word even if _or- , etc present in between that word<br />coz `b` won't consider _or- , etc as part of a single word rather thinks that its the begining of the prev word) |
| end of word              | `e`                                                                                                                                                                                                     |
| Top of file              | `gg`                                                                                                                                                                                                    |
| Bottom of file           | `G`                                                                                                                                                                                                     |
| Show current line number | `Ctrl+g`                                                                                                                                                                                                |
| Line number              | `:25`                                                                                                                                                                                                   |

### ✂️ Editing Commands (in Normal Mode)

| Action                | Command                    |
| --------------------- | -------------------------- |
| Delete character      | `x`                      |
| Delete word           | `dw`                     |
| Delete line           | `dd`                     |
| Copy (yank) line      | `yy`                     |
| Paste                 | `p`(after),`P`(before) |
| Undo                  | `u`                      |
| Redo                  | `Ctrl+r`                 |
| Replace one character | `r<char>`                |
| Search word           | `/word`+`Enter`        |
| Repeat last search    | `n`(next),`N`(prev)    |

### 🔍 Visual Mode (Selection)

| To Select...        | Press...                                        |
| ------------------- | ----------------------------------------------- |
| Characters (normal) | `v`                                           |
| Whole lines         | `V`                                           |
| Block/column        | `Ctrl+v` ie for multiple lines simultaneously |

Then press:

* `y` to copy ( y for yanking - meaning --to pull something forcefully with a quick movement; Eg- He tripped over the cord and yanked the plug out. She yanked open the cupboard door and everything fell out.);  `yy` and `Y`
* copies whole line
* `d` to cut
* `p` to paste- if done `p` at the begining of line, pastes in a newline; `P` pastes in newline up to current line
* `c` to change; `cc` to change a line; These are same as deleting but in this case you simultaneouly goes into insert mode automatically, but not in case of deleting; `C` changes the whole line from the current position; `ciw` (change in word) changes the entire word even if you are in between the word currently
* `d` to delete; `dw` deletes whole word; `dd` deletes whole line; `D` deletes the whole line from the current position; `diw` (delete in word) deletes the entire word even if you are in between the word currently; One can make custome command like `di"` ie delete in "" so that whatever between the "" only can be deleted specifically (Can be used in `console.log()`) -- Same can be done in case of change ie `ci"`
  > ✅ `d` = **Delete and Cut**
  >
  > 🧠 Think of it this way:
  >
  >> **`d` deletes the selected text and stores it in a register** (like clipboard) — so it's essentially a  **cut** .
  >>
  >
* `r` to replace

### 🚀 Jumping

-- `%` on the starting curly bracket for example of a function jumps the cursor to its corresponding ending bracket. Can be done in any closing/opening element

So `d%` can be therefore to delete everything inside the element including the element for example curly brackets in this case

-- `t*` (t for till) jumps the cursor before the next * . Can be anything instead of the * . ` T` jumps the cursor before the prev * .Hence  `dt(` mean delete till the next opening bracket.

-- `f*` (f for find) jumps the cursor onto the next * . Can be anything instead of the * . ` F` jumps the cursor onto the prev *

### ⚡ Auto Intendation

✅ Usage:

| Command     | Meaning                                          |
| ----------- | ------------------------------------------------ |
| `==`      | Indent the**current line**                 |
| `5==`     | Indent**5 lines**starting from current one |
| `=G`      | Indent from cursor to**end of file**       |
| `gg=G`    | Indent**entire file**                      |
| `=}`      | Indent till**next paragraph**              |
| `V`+`=` | Visual line mode → indent selected lines        |

🧠 How It Works

It relies on:

* Your filetype (e.g., Python, C, HTML)
* Your Vim settings (`shiftwidth`, `expandtab`, etc.)
* Plugins or indent scripts (for smart behavior)

###### Make sure indentation is configured:

```vim
filetype plugin indent on
set autoindent
set smartindent
set shiftwidth=4
set expandtab
```

### 📌 **Line Number Display in Vim**

| Feature                              | Command                        | Description                               |
| ------------------------------------ | ------------------------------ | ----------------------------------------- |
| **Show line numbers**          | `:set number`or `:set nu`  | Displays absolute line numbers            |
| **Hide line numbers**          | `:set nonumber`              | Turns off line numbers                    |
| **Show relative line numbers** | `:set relativenumber`        | Shows line numbers relative to the cursor |
| **Absolute + relative**        | `:set number relativenumber` | Shows both (absolute on cursor line)      |
| **Disable relative only**      | `:set norelativenumber`      | Keeps only absolute numbers               |

Below in the image relative number is being set. **IT ALSO SHOWS ACTUAL LINE NUMBER TO THE LEFT**

![1752675568303](image/Linux_ShellScripting/1752675568303.png)

### 🔢 **Using Numbers in Vim for Repetition**

In Vim, you can **prefix most commands with a number** to repeat them that many times.

🧠 Format:

```
<number><command>
```

✅ Examples

| Command        | Meaning                                                |
| -------------- | ------------------------------------------------------ |
| `5j`         | Move**down 5 lines**                             |
| `3w`         | Move**3 words forward**                          |
| `2x`         | Delete**2 characters**                           |
| `4dd`        | Delete**4 lines**                                |
| `10G`        | Go to**line 10**                                 |
| `3p`         | Paste**3 times**                                 |
| `100i-<Esc>` | Insert**100 hyphens**(use insert mode, then Esc) |
| `5>>`        | Indent**5 lines**to the right                    |
| `3u`         | Undo**3 times**                                  |
| `3ctrl+r`    | Redo 3 times                                           |

> #### 🧠 **Advanced Examples**
>
> Combine with movement:
>
> ```vim
> d3w     ← delete next 3 words  
> 5d3w    ← 5 times delete next 3 words  
> y5j     ← yank next 5 lines  
>>10G    ← indent lines from cursor to line 10
> d$	← delete till the end of the line from the current position
> ```
>
> Combine with search:
>
> ```vim
> /hello  ← search "hello"  
> 5n      ← jump to 5th next match  
> ```
>
> #### 💡 Why This is Powerful
>
> Vim’s design allows you to:
>
>> **"Do X, N times"** — not just press a key repeatedly.
>>
>
> You can:
>
> * Delete 10 lines in a blink: `10dd`
> * Insert 20 stars: `20i*<Esc>`
> * Jump 30 lines down: `30j`
>
> All without touching the mouse or arrow keys multiple times.

### ⚙️ Configuration (Optional)

* This is used do that you dont have to manually set all the settings for the editor like setting line number, tabstop, color-scheme, everytime
  * That's because each time Vim opens, it opens up in the raw settings witout any rememberance of any previous settings
* Vim config file is located in: `~/.vimrc`
* Example settings:

```vim
syntax on
set number
set tabstop=4
set autoindent
```

## 🏁 Final Tip

> 🔥 Vim is tough at first — but once you get it, it becomes  **incredibly fast and efficient** .

Would you like me to give you a *practice file* or walk you through a hands-on example session to learn it quickly?

.........................................................................................................................................................................................................................................

## ---------- Vim-2 ----------

### **Vim Settings** To Customize Behavior, Appearance, and Usability

These are all **Vim settings** that customize behavior, appearance, and usability — especially for those who edit code or config files. Here's a clear breakdown:

#### 🖱️ 1. `mouse`

🔹 What it does:

Enables **mouse support** in Vim — lets you click, scroll, and select with the mouse.

🔹 Common values:

```vim
:set mouse=a     " enable mouse in all modes
:set mouse=n     " enable only in normal mode
:set mouse=      " disable mouse (empty string)
```

| Value | Meaning           |
| ----- | ----------------- |
| `n` | Normal mode only  |
| `v` | Visual mode only  |
| `i` | Insert mode only  |
| `a` | All modes         |
| `r` | Replace mode      |
| `c` | Command-line mode |

✅ Recommended:

```vim
:set mouse=a
```

#### 🔠 2. `tabstop`

🔹 What it does:

Defines how many **spaces a `Tab` character** shows as.

🔹 Example:

```vim
:set tabstop=4
```

This means a real tab character (`\t`) will visually align as  **4 spaces** .

🧠 Note: This does **not insert spaces** — it just affects how tabs look.

🔄 Related Settings:

| Setting         | What it affects                                              |
| --------------- | ------------------------------------------------------------ |
| `tabstop`     | Width of actual `Tab`characters                            |
| `softtabstop` | How many spaces to use when pressing `Tab`(in insert mode) |
| `shiftwidth`  | Indent/unindent size when using `>>`,`<<`,`==`         |
| `expandtab`   | Replace `Tab`with spaces                                   |

#### ➡️ 3. `shiftwidth`

🔹 What it does:

Controls how many spaces to use when **auto-indenting** or shifting lines.

🔹 Example:

```vim
:set shiftwidth=4
```

So, when you press:

```vim
>>   ← Indent line
<<   ← Un-indent line
```

…it adds or removes 4 spaces.

#### 🎨 4. `colorscheme`

🔹 What it does:

Changes Vim's **color theme** — affects syntax highlighting and interface colors.

🔹 Example:

```vim
:colorscheme desert
:colorscheme elflord
:colorscheme gruvbox     " (if installed)
```

🔹 How to see available themes:

```vim
:colorscheme <Tab>
```

🔹 How to set permanently:

Add to `~/.vimrc`:

```vim
syntax on
colorscheme desert
```

You can also download extra themes to `~/.vim/colors`.

#### 🧠 5. `autoindent`

🔹 What is `autoindent`?

`autoindent` is a Vim setting that tells Vim to **automatically copy the indentation of the previous line** when you start a new line.

So when you're writing code, instead of having to press `Tab` or space every time, Vim will keep the same indentation for you.

🧪 Example:

If you have:

```c
int main() {
    printf("Hello");
```

and you press `Enter` after the second line, with `autoindent` enabled, the new line will start at the same indent level like this:

```c
int main() {
    printf("Hello");
    |
```

(The `|` indicates the cursor position — notice it's indented automatically.)

✅ How to Enable It:

In Vim:

```vim
:set autoindent
```

🔄 Related Settings:

| Setting         | Purpose                                                     |
| --------------- | ----------------------------------------------------------- |
| `autoindent`  | Keeps the same indent as the previous line                  |
| `smartindent` | Adds**basic C-style indenting**(e.g., after `{`)    |
| `cindent`     | Provides**advanced indenting**based on C syntax       |
| `smarttab`    | Makes `<Tab>`smarter with respect to indentation levels   |
| `shiftwidth`  | Controls how much to indent when using `>>`or auto indent |
| `expandtab`   | Converts tabs to spaces                                     |

### 🛠️ Sample `.vimrc` Config:

```vim
set number
set mouse=a
set tabstop=4
set shiftwidth=4
set autoindent
set expandtab
syntax on
colorscheme desert
```

This setup:

* Auto indents your code
* Inserts 4 spaces per indentation
* Converts tabs into spaces (better for code consistency)

> #### 🎨 `syntax on` in Vim
>
> ###### 🔹 What it does:
>
> The `syntax on` command **enables syntax highlighting** in Vim. That means:
>
> * **Keywords** ,  **strings** ,  **comments** , etc., in programming files will be **color-coded**
> * Makes code easier to  **read** ,  **debug** , and **navigate**
>
> ###### 🧪 Example:
>
> Without `syntax on`, a Python file might look like this:
>
> ```python
> def hello():
>     print("Hello, world!")
> ```
>
> With `syntax on`, you'd see:
>
> * `def` in **blue**
> * `"Hello, world!"` in **green**
> * Functions/keywords in different colors depending on your colorscheme
>
> #### ✅ How to Use
>
> In Vim command mode:
>
> ```vim
> :syntax on
> ```
>
> Or to  **make it permanent** , add to your `~/.vimrc`:
>
> ```vim
> syntax on
> ```
>
> To  **turn it off** :
>
> ```vim
> :syntax off
> ```
>
> ##### 🧠 Notes:
>
> * `syntax on` works based on the **file type** detected (e.g., `.py`, `.js`, `.html`)
> * Vim automatically detects filetype when you open a file
> * You can see what filetype is set using:
>   ```vim
>   :set filetype?
>   ```
>
> ##### 🛠️ Troubleshooting
>
> If syntax highlighting doesn't work:
>
> * Check that the file has the right extension (e.g., `.js`, `.html`)
> * Run `:filetype detect`
> * Make sure your terminal or GUI supports colors
> * Ensure your `~/.vimrc` contains:
>   ```vim
>   syntax on
>   filetype plugin indent on
>   ```

### 🔍 **Basic Searching**

✅ Search Forward

```
/pattern
```

* Press `/`, then type the text you want to search.
* Press `Enter` to jump to the next match.

✅ Search Backward

```
?pattern
```

* Like `/`, but searches in the **opposite direction** (up).

##### 🔁 Navigating Search Results

| Key   | Action                                              |
| ----- | --------------------------------------------------- |
| `n` | Jump to**next**match (same direction)         |
| `N` | Jump to**previous**match (opposite direction) |

##### 🛠️ Examples

```vim
/foo     → search forward for "foo"
?main    → search backward for "main"
n        → jump to next "foo"
N        → jump to previous "foo"
```

##### 🔠 Case Sensitivity

By default, Vim is **case-sensitive** when searching.

To ignore case:

```vim
:set ignorecase
```

But still match case when pattern has capitals:

```vim
:set smartcase
```

Or just use:

| Search       | Behavior                                                  |
| ------------ | --------------------------------------------------------- |
| `/hello`   | Matches only `hello`                                    |
| `/Hello`   | Matches only `Hello`                                    |
| `/\cHello` | Match `hello`,`Hello`, etc. (case-insensitive inline) |
| `/\Chello` | Match**only** `hello`(force case-sensitive)       |

##### 🎯 Search for Whole Words Only

```vim
/\<word\>
```

* `\<` → start of word
* `\>` → end of word

✅ Example:

```vim
/\<main\>
```

…matches only full word "main", not "mainly".

##### 📁 Search with Regex (Very Powerful)

```vim
/\d\+      → matches one or more digits
/^[A-Z]    → matches lines starting with capital letter
/foo.*bar  → match lines with "foo" followed by "bar"
```

##### Optional: Show Matches While Typing

```vim
:set incsearch
```

This shows partial matches while you type the search pattern.

### 🔍 Searching a token where the cursor is onto

 In Vim, `#` and `*` are **powerful search shortcuts** used for quickly jumping to matches of the word under the cursor.

##### 🔹 `*` (Asterisk) — Search **Forward**

* Searches **forward** for the **word under the cursor**
* Matches **whole words only** (not substrings)

🧪 Example:

If your cursor is on the word `main`, pressing `*` will search forward and highlight every other `main` in the file.

##### 🔹 `#` (Hash) — Search **Backward**

* Searches **backward** for the **word under the cursor**
* Same as `*` but in reverse

##### 🧭 After pressing `*` or `#`, you can navigate results with:

| Key   | Action                                              |
| ----- | --------------------------------------------------- |
| `n` | Jump to**next**match (same direction)         |
| `N` | Jump to**previous**match (opposite direction) |

##### 📌 Example:

In this text:

```c
int main() {
    int maintain = 1;
}
```

* Place your cursor on `main`
* Press `*` → only matches `main`, **not** `maintain`

### ⭐ Marking in Vim

Marking in Vim is a powerful feature that lets you **bookmark specific positions** in a file so you can quickly jump back to them. Think of it like placing a sticky note on a line of code.

##### 🔖 What Are Marks?

Marks are labels (like `a`, `b`, `'`) you assign to positions (line + column) in your file.

They allow you to:

* Jump back and forth between locations
* Yank/delete between marks
* Use them across files or within one file

##### 📌 How to Set a Mark

In  **Normal mode** , press:

```
m{letter}
```

* `m` stands for **mark**
* `{letter}` is **any lowercase letter** you choose

### Example:

```
ma
```

👉 Marks the **current cursor position** as mark `a`.

##### 🚀 How to Jump to a Mark

* `'a` → Jump to the **beginning of the line** where mark `a` is.
* a `→ Jump to the **exact column and line** of mark`a`.

> Tip: Use backtick (`) for  **precise position** , apostrophe (') for  **line start** .

##### 📚 Mark Types

| Type                | Description                                  |
| ------------------- | -------------------------------------------- |
| Lowercase `a-z`   | Local marks (valid in the current file only) |
| Uppercase `A-Z`   | Global marks (can be used across files)      |
| `'`(single quote) | Previous jump position (very handy)          |

##### 🔎 View All Marks

To list all marks:

```vim
:marks
```

##### 🧠 Common Use Case

1. You're editing code at the top.
2. Set a mark with `ma`.
3. Scroll way down and work on something else.
4. Instantly return to mark `a` with `'a` or a`.

### 💤 `zz` in Vim — Center the Line

The command `zz` is used to **scroll the screen** so that the **current line (where your cursor is)** moves to the **center** of the screen.

🧠 Meaning:

> "`zz` recenters the view — not the cursor!"

It doesn’t move your cursor’s position within the file, but **adjusts the window** so that the current line appears in the **middle** of the screen.

✅ Example:

Let’s say you’re editing a file, and your cursor is on line 80 of a 100-line file, but it's near the bottom of your screen.

Pressing:

```
zz
```

👉 will scroll the screen so  **line 80 appears in the middle** , improving visibility.

##### 📌 Common Use Case:

* You search for something using `/pattern`
* Cursor lands on the line, but it's at the bottom
* Press `zz` to bring it  **to the center** , so you can read better

### 🔍 Search And Replace

 In Vim, **search and replace** is done using the powerful `:substitute` command (`:s`).

🧠 Basic Syntax

```vim
:[range]s/search_pattern/replacement/[flags]
```

✅ Simple Example

```vim
:s/foo/bar/
```

* Replaces the **first occurrence of `foo`** with `bar`  **on the current line** .

🔁 Replace All on Current Line

```vim
:s/foo/bar/g
```

* `g` = replace **all matches** in that line

🌍 Replace in the Entire File

```vim
:%s/foo/bar/g
```

* `%` = entire file
* `g` = all occurrences per line

##### 📌 Common Flags

| Flag  | Meaning                                  |
| ----- | ---------------------------------------- |
| `g` | Replace all matches in the line or range |
| `c` | Confirm before replacing each match      |
| `i` | Case-insensitive match                   |

🔥 Examples:

* Replace  **with confirmation** :
  ```vim
  :%s/foo/bar/gc
  ```
* Replace  **case-insensitively** :
  ```vim
  :%s/foo/bar/gi
  ```

##### 🛠 Confirm Each Change

When using `c` flag:

```vim
:%s/foo/bar/gc
```

You'll be prompted:

```
replace with bar (y/n/a/q/l/^E/^Y)?
```

* `y` = yes
* `n` = no
* `a` = all
* `q` = quit
* `l` = last one
* `Ctrl+E/Y` = scroll screen

### 🔁 **Repeat the last change** (edit, delete, paste, etc.)

 In Vim, the **`.` (dot)** command means: **Repeat the last change** (edit, delete, paste, etc.)

✅ What it does:

The `.` command **repeats** your **last editing action** — not movement, but actual **modification** to the text.

📌 Example 1: Delete and repeat

```vim
x      " deletes one character
.      " deletes the next character (same as 'x' again)
```

📌 Example 2: Change and repeat

```vim
cwfoo⏎   " change a word to 'foo'
.        " repeat: changes next word to 'foo'
```

##### 🔂 Repeatable commands include:

* `x`, `dd`, `cw`, `r`, `J`, `>>`, `<<`
* `a`, `i`, `o`, etc., after they insert something

##### ⚠️ Note:

It repeats **exactly** what you did — including the motion or operator you used.

##### 🔥 Pro tip:

You can also combine `.` with **macros or markers** to repeat changes  **very efficiently** .

### Registers in Vim

### Macros in Vim

---
