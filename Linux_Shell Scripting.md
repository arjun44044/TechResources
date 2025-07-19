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
* Or `O` → (insert newline ABOVE (Opens newline)) ie the next line would be empty to
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

* `y` to copy ( y for yanking - meaning --to pull something forcefully with a quick movement; Eg- He tripped over the cord and yanked the plug out. She yanked open the cupboard door and everything fell out.);  `yy` and `Y` copies whole line
* `d` to cut
* `p` to paste- if done `p` at the begining of line, pastes in a newline else from current position; `P` pastes in newline up to current line
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

......................................................................................................................

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

......................................................................................................................

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

......................................................................................................................

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

......................................................................................................................

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

......................................................................................................................

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

......................................................................................................................

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

......................................................................................................................

### Registers in Vim

In Vim, `:reg` or `:registers` is a command used to **view the contents of registers** — which are like temporary storage areas used by Vim to hold copied, deleted, or recorded text.

##### 🧠 What Is a Register in Vim?

Think of a **register** as a clipboard — but Vim gives you  **multiple clipboards** , each with its own name (like `"a`, `"b`, etc.).

Registers are used when you:

* Yank (copy) text: `y`
* Delete text: `d`
* Change text: `c`
* Paste text: `p`
* Record macros: `q`

##### 🧾 `:reg` Command

```vim
:reg         → shows contents of all registers
:reg a       → shows contents of register `a`
```

##### 📋 Types of Registers

| Register     | Usage / Meaning                                             |
| ------------ | ----------------------------------------------------------- |
| `"`        | **Unnamed register**(default one for most operations) |
| `0`        | Last yank                                                   |
| `1`–`9` | Delete history (most recent in `1`)                       |
| `a`–`z` | Named registers (manually used by you)                      |
| `+`        | System clipboard (Ctrl+C / Ctrl+V)                          |
| `*`        | Primary selection clipboard (X11 on Linux)                  |
| `%`        | Current file name                                           |
| `:`        | Last executed command                                       |
| `.`        | Last inserted text                                          |
| `#`        | Alternate file name                                         |
| `=expr`    | Evaluate expression (e.g.`:put =2+2`)                     |

##### 🧪 Example Usage

###### Yank into a named register:

```vim
"ayw       → yank a word into register `a`
```

###### Paste from a register:

```vim
"ap        → paste from register `a`
```

###### Check it:

```vim
:reg a
```

##### Use Case: Saving Multiple Copies

* Yank 3 different lines into `a`, `b`, and `c`
* Paste them later selectively with `"ap`, `"bp`, `"cp`

##### 🔄 Repeat with `.`

After doing any yank or delete, use `.` to repeat that operation using the same register.

##### 📌 Tip:

If you don't specify a register, Vim uses the `"` unnamed register automatically.

......................................................................................................................

### Macros in Vim

Macros in Vim are one of its most powerful features — they let you **record and replay a sequence of commands** to automate repetitive tasks.

##### 🎥 What is a Macro in Vim?

A **macro** is a recorded sequence of keystrokes (commands) that you can replay anytime.

It’s perfect for:

* Editing multiple lines in the same way
* Automating text transformations
* Avoiding repetitive typing

##### 🧠 Basic Commands

| Command    | Description                                 |
| ---------- | ------------------------------------------- |
| `q{a-z}` | Start recording into register `{a-z}`     |
| `q`      | Stop recording                              |
| `@{a-z}` | Play the macro stored in register `{a-z}` |
| `@@`     | Replay the**last run**macro           |

##### ✅ Example: Uppercase the first word of several lines

1. Go to the first line
2. Start recording into register `a`:

   ```
   qa
   ```
3. Type commands:

   ```
   0w~q
   ```

   * `0` → go to beginning of line
   * `w` → move to first word
   * `~` → change case of character
   * `q` → stop recording
4. Move to the next line
5. Play macro:

   ```
   @a
   ```
6. Repeat macro:

   ```
   @@
   ```

##### 🔁 Repeat Macro N Times

```vim
10@a   → play macro `a` 10 times
```

##### 🔍 View Macros

```vim
:reg a
```

→ shows the contents of macro `a`.

##### 🛠 Save Complex Edits

You can even record:

* Insert mode typing
* Visual selections
* Delete, change, replace, paste actions

Everything except  **mouse input** .

##### 📌 Tips

* You can use **named registers** (`a`–`z`) to store different macros
* Use **capital letters** to append to an existing macro:
  ```
  qA   → appends to macro in register `a`
  ```

##### 🧪 Advanced: Play on a Visual Selection

Use a plugin like [`vim-macrobatics`](https://github.com/jesseleite/vim-macrobatics), or use the [`:normal`](https://vimhelp.org/various.txt.html#:normal) command:

```vim
:'<,'>normal @a
```

(Plays macro `a` on each line in the visual selection.)

---

---

# 🗺️ Shell Scripting

**Shell scripting** is the process of writing a series of commands for a Unix-based shell (like  **bash** ,  **sh** ,  **zsh** , etc.) to automate tasks that you would otherwise execute manually in a terminal. These commands are written in a plain text file, typically with a `.sh` extension, and can be executed by the shell interpreter.

### 🔹 What is a Shell?

A **shell** is a command-line interpreter that provides a user interface for accessing the services of the operating system.

* **Common shells:**
  * `sh` – Bourne shell
  * `bash` – Bourne Again Shell (most common in Linux)
  * `zsh` – Z Shell
  * `csh` – C Shell
  * `ksh` – Korn Shell

### 🔹 What is Shell Scripting?

A **shell script** is a **text file** containing a sequence of shell commands. When run, the shell reads and executes the commands  **line by line** .

### 🔹 Structure of a Shell Script

```bash
#!/bin/bash         # Shebang - tells OS to use bash shell to interpret the script

echo "Hello, World" # Command
```

> #### 🔹 What is a **Shebang** ?
>
> The **shebang** is the **first line** in a script that tells the operating system **which interpreter** to use to run the file.
>
> 📌 Syntax:
>
> ```bash
> #!/path/to/interpreter
> ```
>
> 🧠 Example:
>
> ```bash
> #!/bin/bash
> echo "Hello from bash!"
> ```
>
> Here:
>
> * `#!` → is the **shebang**
> * `/bin/bash` → is the **path to the Bash interpreter**
>
> So this tells the OS: **“Use the bash program to run this script.”**
>
> ##### 🔹 Why is it called **"shebang"?**
>
> The name **shebang** is a blend of:
>
> * **"sh"** → short for **shell**
> * **"bang"** → slang for the **exclamation mark (!)**
>
> So:
>
> ```bash
> #!   → "sh-bang"
> ```
>
> Over time, "sh-bang" was slurred together as " **shebang** ".
>
> 🔹 Common Shebangs
>
> | Shebang                    | Interpreter  | Use Case                               |
> | -------------------------- | ------------ | -------------------------------------- |
> | `#!/bin/bash`            | Bash Shell   | Default in Linux                       |
> | `#!/bin/sh`              | Bourne Shell | More portable                          |
> | `#!/usr/bin/env python3` | Python 3     | Uses `env`to find Python in `PATH` |
> | `#!/usr/bin/perl`        | Perl         | Perl scripts                           |
>
> Using `/usr/bin/env` is often preferred for portability, especially across different environments:
>
> ```bash
> #!/usr/bin/env bash
> ```
>
> ##### 🔹 What Happens Without a Shebang?
>
> * If you run the script like: `bash script.sh`, it uses `bash` regardless.
> * But if you run: `./script.sh`, and there’s  **no shebang** , the OS uses the **default shell** (usually `/bin/sh`), which may not behave as expected.

### 🔹 Features and Capabilities

Shell scripts can include:

1. **Variables**
   ```bash
   name="Arun"
   echo "Hello, $name"
   ```
2. **Conditional Statements**
   ```bash
   if [ $age -ge 18 ]; then
     echo "Adult"
   else
     echo "Minor"
   fi
   ```
3. **Loops**
   ```bash
   for i in 1 2 3
   do
     echo "Number: $i"
   done
   ```
4. **Functions**
   ```bash
   greet() {
     echo "Hi $1"
   }
   greet "Arun"
   ```
5. **Reading Input**
   ```bash
   read -p "Enter name: " username
   echo "Welcome $username"
   ```
6. **Command Substitution**
   ```bash
   current_date=$(date)
   echo "Today is: $current_date"
   ```

### 🔹 Benefits of Shell Scripting

* **Automation** : Automate repetitive tasks (e.g., backups, monitoring)
* **System Admin Tasks** : Manage users, disk space, services, etc.
* **Customization** : Personalized environments, login scripts
* **Batch Processing** : Run multiple commands or processes in one go

### 🔹 How to Run a Shell Script

1. Make it executable:
   ```bash
   chmod +x script.sh
   ```
2. Execute:
   ```bash
   ./script.sh
   ```

Or run directly with:

```bash
bash script.sh
```

### 🔹 Real-World Use Cases

* Auto backups
* Log file monitoring
* Server startup scripts
* Cron jobs for scheduled tasks
* Deployment automation
* Network scanning with tools like `nmap`

### 🔹 Drawbacks

* Not suitable for complex GUIs or very large applications
* Portability issues across different shells or OSes
* Less powerful than general-purpose programming languages for large-scale applications

---

# ----Comments

🔹 What are **Comments** in Shell Scripting?

**Comments** are lines in a shell script that are **not executed** by the shell. They are written **only for humans** — to explain what the script does, why a command is used, or to temporarily disable parts of code.

### 🔸 How to Write Comments

```bash
# This is a comment
echo "Hello"  # This is an inline comment
```

* Anything after a `#` on a line is a comment.
* Comments can be on their own line or after a command (inline).

🔸 Rules for Comments

1. **Start with `#`** (hash symbol)
2. **Everything after `#` is ignored** by the shell (except in special cases like the shebang `#!`)
3. **No multi-line comment syntax** like in some other languages (e.g., `/* ... */` in C)

🔸 Example

```bash
#!/bin/bash

# This script prints the current date and time

echo "The current time is:"  # A heading
date                        # Prints the system date and time
```

### 🔸 Simulating Multi-Line Comments (Not native)

Shell doesn’t have real multi-line comments, but you can simulate them using a **`here document`** (not recommended for actual commenting):

```bash
: << 'END_COMMENT'
This is a fake multi-line comment.
It won't be executed because it's inside a here document
END_COMMENT
```

### 🔸 Bad vs Good Practice

❌ **Bad (no comments):**

```bash
#!/bin/bash
x=42
y=13
z=$((x + y))
echo $z
```

✅ **Good (with comments):**

```bash
#!/bin/bash

# Set variables
x=42
y=13

# Add two numbers
z=$((x + y))

# Output the result
echo $z
```

---

# ----`echo` and `print` in Shell Scripting (Bash)

In  **shell scripting** , the most commonly used command to display text/output is:

✅ `echo`

There is **no `print` command** built into **bash** like in Python or PHP. In Bash, we **use `echo`** (and sometimes `printf`, which is more powerful).

### 🔸 `echo` Command

The `echo` command is used to  **display messages** , variable values, or command outputs to the terminal.

✅ Basic Usage:

```bash
echo "Hello, World"
```

✅ Example with Variables:

```bash
name="Arun"
echo "My name is $name"
```

### 🔸 Options with `echo`

| Option | Description                                 | Example                    |
| ------ | ------------------------------------------- | -------------------------- |
| `-n` | Don’t add newline at the end               | `echo -n "Hello"`        |
| `-e` | Enable escape characters like `\n`,`\t` | `echo -e "Line1\nLine2"` |
| `-E` | Disable escape interpretation (default)     | `echo -E "Line1\nLine2"` |

✅ Example with `-e`:

```bash
echo -e "Hello\nWorld"
```

Output:

```
Hello
World
```

### 🔸 `printf` (Advanced Alternative to `echo`)

While `echo` is simple and great for basic printing, `printf` gives you **formatted output** just like in C or Python.

✅ Example:

```bash
printf "Name: %s\nAge: %d\n" "Arun" 25
```

Output:

```
Name: Arun
Age: 25
```

✅ Key Advantages of `printf`:

* More predictable (echo can behave differently in different shells)
* Supports formatting (like `%s`, `%d`, etc.)

### 🔸 What About `print`?

There is  **no native `print` command in Bash** . If you try:

```bash
print "Hello"
```

❌ You’ll get:

```
bash: print: command not found
```

🔸 `print` exists in some other shells like **KornShell (ksh)** or  **Z Shell (zsh)** , but not in bash.

### 🔸 Summary

| Feature     | `echo`           | `printf`             | `print`       |
| ----------- | ------------------ | ---------------------- | --------------- |
| Simplicity  | ✅ Very simple     | ❌ Slightly complex    | ❌ Not in bash  |
| Formatting  | ❌ Limited         | ✅ Supports formatting | ❌              |
| Portability | ✅ Mostly portable | ✅ Highly consistent   | ❌ Not portable |

---

# ----Variables

🔹 What is a Variable in Shell?

A **variable** in shell scripting is used to  **store and manipulate data** , such as strings, numbers, file paths, user inputs, etc.

You can think of a variable as a **label for a piece of information** that you want to use and reuse in your script.

### 🔸 Declaring Variables

✅ Syntax:

```bash
variable_name=value
```

⚠️ Important Rules:

* **No spaces** around `=`
  ```bash
  name=Arun      # ✅ Correct
  name = Arun    # ❌ Wrong
  ```
* Variable names can contain letters, digits, and underscores: `A-Z`, `a-z`, `0-9`, `_`
* Variable names **cannot start with a number**

### 🔸 Accessing (Using) Variables

Use the **`$` sign** to access the value of a variable.

```bash
name="Arun"
echo $name         # Output: Arun
```

### 🔸 Quoting Variables

Using quotes when printing variables is good practice.

```bash
greeting="Hello, World"
echo "$greeting"   # ✅ Best practice
```

### 🔸 Types of Variables

##### 1. **User-defined (custom) variables**

Set by you in the script or terminal.

```bash
count=5
echo $count
```

##### 2. **Environment variables**

Available system-wide (e.g., `HOME`, `USER`, `PATH`)

```bash
echo $HOME
echo $USER
```

You can also export your own:

```bash
export myvar="visible in child shells"
```

### 🔸 Read User Input into a Variable

```bash
read -p "Enter your name: " name
echo "Hello, $name"
```

### 🔸 Arithmetic with Variables

Use `$(())` or `let` for math.

```bash
a=10
b=5
sum=$((a + b))
echo "Sum is: $sum"
```

OR

```bash
let sum=a+b
echo $sum
```

> #### `let`
>
> * ✅ `let` is a  **built-in command** ,  **not a keyword** .
> * It's used  **only for integer arithmetic operations** .
> * It **does not declare a variable** — it just evaluates an arithmetic expression.

### 🔸 String Operations

```bash
str="Hello"
echo "${str}World"   # HelloWorld
len=${#str}
echo "Length: $len"  # Length: 5
```

### 🔸 Special Variable Forms

| Syntax              | Meaning               |
| ------------------- | --------------------- |
| `$var`            | Get value,            |
| `${var}`          | Safe way to reference |
| `${#var}`         | Length of string      |
| `${var:-default}` | Use default if unset  |

> #### 🔴 Caution:
>
> ```bash
> echo "$(str)World"
> len=$(#str)
> ```
>
> ❌ Both of these are **incorrect** syntax in Bash. Here's why:
>
> ##### 🔸 1. `echo "$(str)World"` ❌
>
> **Why it's wrong:**
>
> * `$(...)` is  **command substitution** , not variable substitution.
> * You're telling Bash to  **run a command named `str`** , not use a variable.
>
> ```bash
> str="Hello"
> echo "$(str)World"
> ```
>
> This will try to execute a command called `str`, and you'll get:
>
> ```
> bash: str: command not found
> ```
>
> ✅ Correct version (variable substitution):
>
> ```bash
> str="Hello"
> echo "${str}World"
> ```
>
> This outputs:
>
> ```
> HelloWorld
> ```

### 🔸 Variable Scope

* By default, variables are **local** to the shell.
* Use `export` to make them  **available to child processes** .

```bash
export name="Arun"
```

### 🔸 Unsetting Variables

To delete a variable:

```bash
unset name
```

After this, `echo $name` gives nothing.

### 🔸 Examples

```bash
#!/bin/bash

# User-defined variable
course="Shell Scripting"

# Environment variable
echo "User: $USER"

# String manipulation
message="Welcome to $course"
echo "$message"

# Arithmetic
a=10
b=20
result=$((a * b))
echo "Multiplication: $result"
```

---

# Read Command

 Let's dive **deep into the `read` command** in shell scripting — one of the most important tools for interactive input.

🔹 What is `read` in Shell Scripting?

The `read` command is used to **take input from the user** and store it in  **variables** .

✅ Basic Syntax:

```bash
read variable_name
```

### 🔸 Example: Basic Input

```bash
echo "Enter your name:"
read name
echo "Hello, $name!"
```

🟢 Output (if user types `Arun`):

```
Enter your name:
Arun
Hello, Arun!
```

### 🔸 Syntax Variants

```bash
read [options] variable1 variable2 ...
```

* If multiple variables are provided, input is  **split by spaces** .
* The **first word** goes to the first variable, the second to the second, etc.

✅ Example:

```bash
echo "Enter your first and last name:"
read first last
echo "First: $first"
echo "Last: $last"
```

Input:

```
Arun Sharma
```

Output:

```
First: Arun
Last: Sharma
```

### 🔹 Common Options with `read`

| Option   | Description                            | Example                          |
| -------- | -------------------------------------- | -------------------------------- |
| `-p`   | Prompt message inline                  | `read -p "Enter age: " age`    |
| `-s`   | Silent (no echo, good for passwords)   | `read -s -p "Password: " pass` |
| `-n N` | Read**N characters only**        | `read -n 1 key`                |
| `-t N` | Timeout after N seconds                | `read -t 5 input`              |
| `-r`   | Don’t allow backslash escapes (`\`) | `read -r line`                 |

### 🔸 Examples of Each Option

🔹 1. Prompting the User (`-p`)

```bash
read -p "Enter your city: " city
echo "City: $city"
```

🔹 2. Silent Input for Passwords (`-s`)

```bash
read -s -p "Enter your password: " password
echo -e "\nPassword saved."
```

🔹 3. Fixed Character Count (`-n`)

```bash
read -n 1 -p "Press any key to continue..." key
```

Reads **only 1 character** (useful for confirmation prompts).

🔹 4. Timeout Input (`-t`)

```bash
read -t 5 -p "Enter something within 5 seconds: " response
echo "You typed: $response"
```

If user doesn’t type within 5 seconds, it moves on.

🔹 5. Reading Raw Text (`-r`)

```bash
read -r line
```

Prevents backslashes (`\`) from being interpreted (important for paths and escape characters).

### 🔸 Reading Multiple Lines

To read input line by line (e.g., in a loop):

```bash
while read line
do
  echo "Line: $line"
done < myfile.txt
```

### 🔸 Assigning Default Values (manually)

Bash doesn't support default values  **inside `read`** , but you can do:

```bash
read -p "Enter username: " username
username=${username:-guest}  # If empty, use 'guest'
echo "Logged in as: $username"
```

### 🔹 Best Practices

* Always  **quote variables** : `"$var"` to prevent word splitting
* Use `-p` for clear prompting
* Use `-s` for secure input (like passwords)
* Use fallback values with `:-` to avoid errors

---

# ----Exporting Variables

Let’s break down  **why `export` is needed** , its  **real-world use cases** , and show **clear examples** so it makes total sense.

### 🔹 What is `export` in Shell?

When you define a variable in a shell (like bash), by default, it is only available in the **current shell session** (not to scripts or commands you run from it).

The `export` command **marks a variable to be inherited by child processes** (like subshells, scripts, commands).

### 🔸 Example Without `export` (Child doesn't see the variable)

```bash
name="Arun"
bash -c 'echo "Hello, $name"'    # No output (empty)
```

🔴 Output:

```
Hello,
```

The `name` variable was  **not exported** , so the new bash process (child shell) doesn't see it.

✅ With `export`:

```bash
export name="Arun"
bash -c 'echo "Hello, $name"'   # Now works
```

> #### Explanation for:
>
> ```bash
> bash -c 'echo "Hello, $name"'
> ```
>
> ###### 🔹 What This Line Does:
>
> You're telling your system:
>
>> “Start a new  **bash shell** , and **run** the command `echo "Hello, $name"` inside it.”
>>
>
> ###### 🔹 Breakdown of Each Part:
>
> | Part                      | Meaning                                                                                 |
> | ------------------------- | --------------------------------------------------------------------------------------- |
> | `bash`                  | Starts a**new instance**of the bash shell (a subshell)                            |
> | `-c`                    | Stands for**command**— tells bash to execute the string that follows             |
> | `'echo "Hello, $name"'` | The**command to be executed** , written as a string inside**single quotes** |

🟢 Output:

```
Hello, Arun
```

✅ Now the variable is **available to child shells** and external scripts/commands.

### 🔸 Why and When Do We Use `export`?

✅ Use Case Scenarios:

| Use Case                                                    | Description                                                         |
| ----------------------------------------------------------- | ------------------------------------------------------------------- |
| 🌍**Environment Variables**                           | Needed by system or apps (e.g.,`PATH`,`JAVA_HOME`,`NODE_ENV`) |
| 🧬**Passing values to subshells/scripts**             | When you call scripts from your current shell                       |
| ⚙️**Configuring build tools or deployment scripts** | e.g.,`export DB_USER=root`                                        |
| 🔐**Sensitive values in deployment**                  | like `export SECRET_KEY=xyz`                                      |

### 🔸 Real-World Example: Setting `PATH`

```bash
PATH="$PATH:/opt/myapp/bin"   # Just modifies it in the current shell
export PATH                   # Makes it visible to any child script that uses it
```

### 🔸 Real-World Example: Deployment Script

You want to write a script that runs with your custom database settings:

```bash
export DB_HOST="localhost"
export DB_USER="admin"
export DB_PASS="secret"

./run-deploy.sh
```

Then in `run-deploy.sh`:

```bash
#!/bin/bash
echo "Connecting to DB at $DB_HOST as $DB_USER"
```

✅ This works only because you **exported** the variables.

### 🔸 `export` with Variable Declaration (Short Form)

```bash
export name="Arun"
```

Instead of:

```bash
name="Arun"
export name
```

Both are valid.

### 🔸 To See Exported Variables

```bash
export      # Lists all exported variables
```

---

# ----Quoting and its Types

Quoting is **very important** in shell scripting to control  **how the shell interprets text** , especially when dealing with  **variables, spaces, special characters, and commands** .

Let’s break down the three main types of quoting in shell scripting:

### 🔹 1. **Single Quotes** `' '`

✅ Meaning:

* **Literal quoting** .
* Everything between single quotes is  **taken literally** .
* No  **variable** ,  **command** , or **escape sequence** is expanded or interpreted.

🔸 Example:

```bash
name="Arun"
echo 'Hello $name'
```

**Output:**

```
Hello $name
```

🔸 Use when:

* You want to  **preserve the exact text** .
* Avoid accidental expansion of variables or special characters.

### 🔹 2. **Double Quotes** `" "`

✅ Meaning:

* **Soft quoting** .
* Variables (`$var`) and command substitutions (`$(...)`, ``...``) are  **evaluated** .
* But most **special characters** are preserved (like spaces, newlines, tabs).

🔸 Example:

```bash
name="Arun"
echo "Hello $name"
```

**Output:**

```
Hello Arun
```

🔸 Use when:

* You want to  **expand variables or commands** , but preserve whitespace or formatting.

### 🔹 3. **Backticks**  (Command Substitution - Old Style)

### ✅ Meaning:

* Run a  **command** , and substitute its **output** in place.

### 🔸 Example:

```bash
echo "Today is `date`"
```

**Output (example):**

```
Today is Thu Jul 17 21:00:00 IST 2025
```

> Same as:

```bash
echo "Today is $(date)"
```

✅ The **modern and recommended** form is: `$(...)` instead of backticks ``...``.

### ❗ Backticks vs `$()` (Why prefer `$()`):

| Feature         | Backticks ``...`` | `$(...)`    |
| --------------- | ----------------- | ------------- |
| Nested usage    | Hard to nest      | Easy to nest  |
| Readability     | Less readable     | More readable |
| Modern standard | ❌ No             | ✅ Yes        |

Nested Example (backticks – messy):

```bash
echo "`echo \`date\``"
```

Nested Example (`$()` – clean):

```bash
echo "$(echo $(date))"
```

## 🔸 Summary

| Quote Type    | Allows Variable Expansion? | Allows Command Substitution? | Preserves Whitespace? | Example                          |
| ------------- | -------------------------- | ---------------------------- | --------------------- | -------------------------------- |
| `'single'`  | ❌ No                      | ❌ No                        | ✅ Yes                | `'Hello $USER' → Hello $USER` |
| `"double"`  | ✅ Yes                     | ✅ Yes                       | ✅ Yes                | `"Hello $USER" → Hello Arun`  |
| ``backticks`` | ✅ N/A                     | ✅ Yes                       | ✅ Yes                | ``date ` → Thu Jul 17...`     |

---

# ----Positional Parameters

🧭 What are Positional Parameters in Shell Scripting?

**Positional parameters** are special variables in shell scripts that allow you to access  **arguments passed to the script or a function** .

They are called *"positional"* because they correspond to the **position** of the arguments.

### 🔹 Basic Syntax

| Parameter                 | Meaning                                 |
| ------------------------- | --------------------------------------- |
| `$0`                    | Name of the script itself               |
| `$1`,`$2`, ...,`$9` | The 1st, 2nd, ..., 9th arguments        |
| `${10}`,`${11}`...    | 10th, 11th, etc. (must use braces)      |
| `$#`                    | Total number of arguments passed        |
| `$@`                    | All arguments (preserves quoting)       |
| `$*`                    | All arguments (as a single word/string) |

### 🧪 Example

Let’s say you have a script called `test.sh`:

```bash
#!/bin/bash

echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "All args (\$@): $@"
echo "All args (\$*): $*"
echo "Total args: $#"
```

You run it like this:

```bash
bash test.sh Hello World "Shell Scripting"
```

**Output:**

```
Script name: test.sh
First argument: Hello
Second argument: World
All args ($@): Hello World Shell Scripting
All args ($*): Hello World Shell Scripting
Total args: 3
```

### 🔍 Difference Between `$@` and `$*`

Let’s loop through them:

Using `$*`:

```bash
for arg in "$*"; do
  echo "$arg"
done
```

**Output (single string):**

```
Hello World Shell Scripting
```

Using `$@`:

```bash
for arg in "$@"; do
  echo "$arg"
done
```

**Output (separate values):**

```
Hello
World
Shell Scripting
```

✅ `$@` is more commonly used in scripts when looping through arguments.

### 🔸 Accessing More than 9 Arguments

Once you go beyond 9 arguments, you  **must use braces** :

```bash
echo "$10"      # ❌ Incorrect (gets $1 and appends 0)
echo "${10}"    # ✅ Correct
```

### 🔧 Use Case: A Script with Parameters

```bash
#!/bin/bash

greet() {
  echo "Hello, $1! Today is $2."
}

greet "$1" "$(date +%A)"
```

Run:

```bash
bash greet.sh Arun
```

**Output:**

```
Hello, Arun! Today is Thursday.
```

> Let's clarify **exactly what is happening** in this line:
>
> ```bash
> greet "$1" "$(date +%A)"
> ```
>
> One can ask: **“There is no 2nd argument, so how come it’s printing Thursday?”**
>
> The key point here is:
>
>> You're passing **two arguments** to the function `greet`, even if the script itself received only one.
>>
>
> `$1`, `$2`, etc. **outside of any function** refer to the **positional parameters passed to the script** when you run it.
>
> But  **inside a function** , `$1`, `$2`, etc. refer to the **arguments passed to that function** — *not* the original script arguments.
>
> ##### 🧠 Think of It Like This:
>
> #### 🔹 Script-level (Global Scope):
>
> When you run a script like:
>
> ```bash
> bash script.sh one two three
> ```
>
> Then:
>
> * `$1` → `one`
> * `$2` → `two`
> * `$3` → `three`
>
> These are the **global** positional parameters of the script.
>
> ##### 🔹 Function-level (Local Scope):
>
> If your script has a function:
>
> ```bash
> greet() {
>   echo "Hello, $1! Today is $2."
> }
> ```
>
> Then when you call it like this inside the script:
>
> ```bash
> greet "Alice" "Friday"
> ```
>
> Inside `greet`, `$1` → `Alice`, `$2` → `Friday`.
>
> They **don’t** refer to the script's `$1` and `$2`. Function arguments **override** that.
>
> ✅ Example to Prove This
>
> ```bash
> #!/bin/bash
>
> echo "Script args: \$1=$1, \$2=$2"
>
> greet() {
>   echo "Function args: \$1=$1, \$2=$2"
> }
>
> greet "Hello" "World"
> ```
>
> Run it like:
>
> ```bash
> bash test.sh Arun Thursday
> ```
>
> **Output:**
>
> ```
> Script args: $1=Arun, $2=Thursday
> Function args: $1=Hello, $2=World
> ```
>
> So clearly:
>
> * **Script arguments** are separate from
> * **Function arguments**

### 🔸 Bonus: `shift` Command

`shift` moves all positional parameters  **one position to the left** :

```bash
echo "$1"   # Hello
shift
echo "$1"   # World (Hello is gone)
```

Useful in loops when processing multiple args.

### ✅ Summary

| Symbol            | Meaning                          |
| ----------------- | -------------------------------- |
| `$0`            | Script name                      |
| `$1`to `${N}` | Positional arguments             |
| `$#`            | Total number of arguments        |
| `$@`            | All args as separate words       |
| `$*`            | All args as one string           |
| `shift`         | Shifts all arguments to the left |

---

# ----Default Parameters

In shell scripting, you can **set default values for input parameters or variables** using simple syntax. This is useful when the user  **doesn't pass an argument** , and you want your script to use a fallback value instead.

✅ Syntax for Default Values

```bash
${variable:-default}
```

* If `variable` is set and  **not null** , use its value.
* If `variable` is  **unset or empty** , use `"default"` instead.

### 🔹 Example 1: Default for Positional Parameters

```bash
#!/bin/bash

name=${1:-"Guest"}
echo "Hello, $name!"
```

🔸 Run the script:

```bash
bash script.sh Alice   # Output: Hello, Alice!
bash script.sh         # Output: Hello, Guest!
```

### 🔹 Example 2: Default for a regular variable

```bash
#!/bin/bash

read -p "Enter your city: " city
echo "You are from ${city:-Unknown}"
```

If you just press Enter without typing anything, the output will be:

```
You are from Unknown
```

### 🔹 Extended: Assigning default permanently using `=`

```bash
name=${1:="Guest"}
```

This **assigns** `"Guest"` to `name` if it's unset or empty.

> Difference:
>
> * `:-` just **uses** default.
> * `:=` **uses and sets** default.

## 🧠 Summary

| Syntax          | Behavior                                                           |
| --------------- | ------------------------------------------------------------------ |
| `${var:-val}` | Use `val`if `var`is unset or empty (but don't assign)          |
| `${var:=val}` | Use `val`if `var`is unset or empty**and also assign it** |
| `${var:+val}` | Use `val` **only if var is set**                           |
| `${var:?msg}` | Show `msg`and exit if `var`is unset or empty                   |

---

# Date Format Specifier

Let's take an example --

```bash
#!/bin/bash

greet() {
  echo "Hello, $1! Today is $2."
}

greet "$1" "$(date +%A)"

```

In shell scripting, the `%A` used in:

```bash
date +%A
```

is a **format specifier** passed to the `date` command, which tells it  **how to display the current date/time** .

### ✅ What does `%A` mean?

* `%A` → **Full weekday name**
  * Example output: `Monday`, `Tuesday`, `Wednesday`, etc.

So, this command:

```bash
date +%A
```

will output:

```
Thursday   # if today is Thursday
```

And this line:

```bash
greet "$1" "$(date +%A)"
```

will call the `greet` function with:

* `$1` as the **first argument**
* `$(date +%A)` as the  **second argument** , which will be the current **day name**

### ✅ Combined Format Specifier Table

| Format | Meaning                         | Mnemonic / Stands For         | Example Output |
| ------ | ------------------------------- | ----------------------------- | -------------- |
| `%A` | Full weekday name               | **A**ll of weekday      | Monday         |
| `%a` | Abbreviated weekday name        | **A**bbreviated weekday | Mon            |
| `%d` | Day of the month (01–31)       | **D**ay of month        | 17             |
| `%B` | Full month name                 | **B**ig month name      | July           |
| `%b` | Abbreviated month name          | **B**rief month name    | Jul            |
| `%m` | Month number (01–12)           | **M**onth (numeric)     | 07             |
| `%Y` | Full year                       | **Y**ear full           | 2025           |
| `%y` | Last two digits of year         | **Y**ear short          | 25             |
| `%H` | Hour in 24-hour format (00–23) | **H**our (24-hr)        | 14             |
| `%I` | Hour in 12-hour format (01–12) | **I**n 12-hour format   | 02             |
| `%M` | Minutes (00–59)                | **M**inutes             | 05             |
| `%S` | Seconds (00–59)                | **S**econds             | 07             |
| `%p` | AM or PM                        | **P**eriod of day       | PM             |
| `%T` | Time as `HH:MM:SS`            | **T**ime composite      | 14:05:07       |
| `%F` | Date as `YYYY-MM-DD`          | **F**ull date format    | 2025-07-17     |

🔹 Example 1:

```bash
date "+%A, %B %d, %Y"     # Output: Thursday, July 17, 2025
date "+%I:%M:%S %p"       # Output: 02:05:07 PM
date "+Today is %a (%A)"  # Output: Today is Thu (Thursday)
```

🔹 Example 2:

```bash
#!/bin/bash

echo "Today is: $(date +%A), $(date +%d-%B-%Y)"
```

**Output:**

```
Today is: Thursday, 17-July-2025
```

---

# ----`if`, `else`, `elif` Control Structures

 In  **shell scripting (Bash)** , control statements like `if`, `else`, and `elif` let you make decisions in your script — just like in other programming languages.

### 🔹 **1. `if` Statement Syntax**

```bash
if [ condition ]; then
    # commands if condition is true
fi
```

### 🔹 **2. `if...else` Statement**

```bash
if [ condition ]; then
    # commands if condition is true
else
    # commands if condition is false
fi
```

> `fi`: **Ends an `if` block** (like closing `{}` in other languages)
>
> ##### 🔹 What is `fi`?
>
> In Bash, every `if` block must be closed with `fi` (which is just `if` spelled backward).
>
> 🔁 Think of it as:
>
> ```bash
> if [ condition ]; then
>     # code
> fi  # closes the if block
> ```
>
> It works like `{}` in C, JavaScript, etc.

### 🔹 **3. `if...elif...else` Chain**

```bash
if [ condition1 ]; then
    # commands if condition1 is true
elif [ condition2 ]; then
    # commands if condition2 is true
else
    # commands if none of the above are true
fi
```

### 🔍 **Conditions Are Usually Inside `[ ]` or `[[ ]]`**

Examples of conditions:

```bash
[ "$name" == "admin" ]
[ $age -ge 18 ]
[[ -f file.txt ]]
[[ -z "$input" ]]
```

### ✅ **Example 1: Basic if-else**

```bash
#!/bin/bash

read -p "Enter your age: " age

if [ "$age" -ge 18 ]; then
    echo "You're eligible to vote."
else
    echo "You're not eligible yet."
fi
```

### ✅ **Example 2: Multiple conditions with elif**

```bash
#!/bin/bash

read -p "Enter a number: " num

if [ "$num" -gt 0 ]; then
    echo "Positive number"
elif [ "$num" -lt 0 ]; then
    echo "Negative number"
else
    echo "Zero"
fi
```

### 🧠 Common Operators

| **Type** | **Operator** | **Meaning**                 |
| -------------- | ------------------ | --------------------------------- |
| Numeric        | `-eq`            | equal to                          |
|                | `-ne`            | not equal                         |
|                | `-gt`            | greater than                      |
|                | `-lt`            | less than                         |
|                | `-ge`            | greater than or equal             |
|                | `-le`            | less than or equal                |
| String         | `==`             | equal                             |
|                | `!=`             | not equal                         |
|                | `-z`             | string is empty                   |
| File           | `-f`             | file exists and is a regular file |
|                | `-d`             | directory exists                  |

### ✅ More Examples

🔹 1. `-eq` (Numeric Equality)

Checks if two numbers are equal.

```bash
#!/bin/bash

a=10
b=10

if [ "$a" -eq "$b" ]; then
    echo "a is equal to b"
fi
```

🔹 2. `-z` (String is Empty)

Checks if a variable is empty (zero length).

```bash
#!/bin/bash

read -p "Enter your name: " name

if [ -z "$name" ]; then
    echo "You didn't enter anything."
else
    echo "Hello, $name!"
fi
```

🔹 3. `-f` (File Exists and is a Regular File)

Checks if the file exists and is not a directory or special file.

```bash
#!/bin/bash

file="myfile.txt"

if [ -f "$file" ]; then
    echo "File '$file' exists."
else
    echo "File '$file' does not exist."
fi
```

🔹 4. `-d` (Directory Exists)

Checks if the given path is a directory.

```bash
#!/bin/bash

dir="/home/$USER/Documents"

if [ -d "$dir" ]; then
    echo "Directory '$dir' exists."
else
    echo "Directory '$dir' does not exist."
fi
```

### Single square brackets vs Double square brackets for condtions

##### ✅ Bash allows **two styles** of `if` condition syntax:

###### 1.  **Single square brackets** : `[` ... `]`

* This is the original POSIX/Bourne shell syntax.
* It's actually a **command** (a program named `test`).

```bash
if [ "$a" -eq "$b" ]; then
    echo "Equal"
fi
```

###### 2.  **Double square brackets** : `[[` ... `]]`

* This is **Bash-specific** (not POSIX standard).
* Safer, more powerful, and preferred in modern Bash scripting.
* Allows things like regex (`=~`), and no need to quote variables in some cases.

```bash
if [[ $a -eq $b ]]; then
    echo "Equal"
fi
```

##### 🔍 Why I used `[ ... ]` in examples:

* I wanted to keep the syntax **portable** and compatible with older shells (`sh`, `dash`, etc.).
* But you're right — in  **Bash** , using `[[ ... ]]` is often better.

##### 📌 When to prefer `[[ ... ]]`:

| Feature                  | `[ ... ]` | `[[ ... ]]` |
| ------------------------ | ----------- | ------------- |
| Bash-only features       | ❌          | ✅            |
| Regex with `=~`        | ❌          | ✅            |
| Safer with unquoted vars | ❌          | ✅            |
| POSIX compatibility      | ✅          | ❌            |

##### 🔁 Example Comparison:

Using `[ ... ]`:

```bash
name=""
if [ -z "$name" ]; then
    echo "Empty name"
fi
```

Using `[[ ... ]]` (safer in Bash):

```bash
name=""
if [[ -z $name ]]; then  # quotes not strictly needed
    echo "Empty name"
fi
```

---

# ----Case Statement

The `case` statement in Bash is similar to `switch` in other languages like C, Java, or JavaScript. It’s used when you want to compare one variable or expression against multiple patterns. It makes the script cleaner when you have many `if-elif-else` conditions.

🔹 Syntax:

```bash
case <expression> in
    pattern1)
        # commands
        ;;
    pattern2)
        # commands
        ;;
    *)
        # default commands (like else)
        ;;
esac
```

> 🔸 `esac` is `case` spelled backward — it marks the end of the `case` block.
>
> 🔸 Each pattern ends with `)`
>
> 🔸 Each command block ends with `;;`
>
> 🔸 `*` acts like "default" or "else"

### ✅ Example 1: Simple menu

```bash
#!/bin/bash

echo "Enter a number (1-3): "
read number

case $number in
    1)
        echo "You selected One"
        ;;
    2)
        echo "You selected Two"
        ;;
    3)
        echo "You selected Three"
        ;;
    *)
        echo "Invalid option"
        ;;
esac
```

### ✅ Example 2: Check file extension

```bash
#!/bin/bash

read -p "Enter a filename: " filename

case $filename in
    *.txt)
        echo "It's a text file"
        ;;
    *.jpg|*.png)
        echo "It's an image file"
        ;;
    *.sh)
        echo "It's a shell script"
        ;;
    *)
        echo "Unknown file type"
        ;;
esac
```

### ✅ Example 3: Days of the week

```bash
#!/bin/bash

day=$(date +%A)

case $day in
    Monday)
        echo "Start of the work week!"
        ;;
    Friday)
        echo "Almost weekend!"
        ;;
    Saturday|Sunday)
        echo "It's weekend!"
        ;;
    *)
        echo "A regular weekday."
        ;;
esac
```

---

# ----Test command and [ ... ]

 The `test` command and `[ ... ]` are essential for **evaluating conditions** in shell scripts, especially in `if`, `while`, and other control structures.

### 🔹 1. What is `test`?

`test` is a command-line utility used to  **evaluate expressions** .

It returns:

* `0` (true) if the condition is **satisfied**
* `1` (false) otherwise

✅ Example:

```bash
test 5 -eq 5
echo $?   # prints 0 (true)
```

### 🔹 2. `[ ... ]` is a synonym for `test`

The square brackets `[ ... ]` are just **another form** of the `test` command.

```bash
[ 5 -eq 5 ]
echo $?   # 0 (true)
```

✅ So:

```bash
test "$a" = "$b"
```

is the same as:

```bash
[ "$a" = "$b" ]
```

> ⚠️ **Spaces are very important.**
>
> `[ "$a" = "$b" ]` is correct.
>
> `["$a" = "$b"]` will cause an error.

### 🔹 3. Common `test` operators

##### 🧮 Numeric comparisons:

| Operator | Meaning            | Example               |
| -------- | ------------------ | --------------------- |
| `-eq`  | equal              | `[ "$a" -eq "$b" ]` |
| `-ne`  | not equal          | `[ "$a" -ne "$b" ]` |
| `-lt`  | less than          | `[ "$a" -lt "$b" ]` |
| `-le`  | less than or equal | `[ "$a" -le "$b" ]` |
| `-gt`  | greater than       | `[ "$a" -gt "$b" ]` |
| `-ge`  | greater or equal   | `[ "$a" -ge "$b" ]` |

##### 📝 String comparisons:

| Operator       | Meaning             | Example              |
| -------------- | ------------------- | -------------------- |
| `=`or `==` | strings are equal   | `[ "$a" = "$b" ]`  |
| `!=`         | strings not equal   | `[ "$a" != "$b" ]` |
| `-z`         | string is empty     | `[ -z "$a" ]`      |
| `-n`         | string is NOT empty | `[ -n "$a" ]`      |

### 📁 File conditions:

| Operator | Checks if...               | Example                 |
| -------- | -------------------------- | ----------------------- |
| `-f`   | file exists and is regular | `[ -f file.txt ]`     |
| `-d`   | directory exists           | `[ -d /path/to/dir ]` |
| `-e`   | file or directory exists   | `[ -e file.txt ]`     |
| `-s`   | file is not empty          | `[ -s file.txt ]`     |
| `-r`   | file is readable           | `[ -r file.txt ]`     |
| `-w`   | file is writable           | `[ -w file.txt ]`     |
| `-x`   | file is executable         | `[ -x script.sh ]`    |

### 🔹 4. Using with `if`

```bash
#!/bin/bash

read -p "Enter a filename: " file

if [ -f "$file" ]; then
    echo "$file is a regular file."
else
    echo "$file does not exist or is not a regular file."
fi
```

### 🔹 5. Double square brackets `[[ ... ]]` (Advanced)

`[[ ... ]]` is an **enhanced** test syntax available in Bash:

* Allows pattern matching (e.g. `[[ $str == a* ]]`)
* No need to quote variables in many cases
* Safer and more flexible

```bash
if [[ "$name" == "Arun" ]]; then
  echo "Welcome Arun!"
fi
```

---

# ----Logical Operators

In shell scripting, **logical operators** are used to combine multiple conditions in `if`, `while`, or other control statements.

### 🔹 1. Logical Operators in Shell

| Operator | Meaning                | Usage with `[ ]`                | Usage with `[[ ]]`(Bash)          |
| -------- | ---------------------- | --------------------------------- | ----------------------------------- |
| `-a`   | AND (both true)        | `[ "$a" -gt 0 -a "$b" -lt 10 ]` | ✅`[ "$a" -gt 0 -a "$b" -lt 10 ]` |
| `-o`   | OR (either true)       | `[ "$a" -lt 5 -o "$b" -gt 8 ]`  | ✅`[ "$a" -lt 5 -o "$b" -gt 8 ]`  |
| `!`    | NOT (negate condition) | `[ ! -f file.txt ]`             | ✅`[ ! -f "file.txt" ]`           |

> ⚠️ These are used with  **single square brackets `[ ]`** , and are POSIX-compliant.

🔸 Example with `[ ]`:

```bash
a=5
b=8

if [ "$a" -lt 10 -a "$b" -gt 5 ]; then
  echo "Both conditions are true"
fi
```

### 🔹 2. Logical Operators in Bash (`[[ ... ]]`)

Bash offers a better syntax using  **`[[ ... ]]`** , which allows:

| Operator | Meaning | Example                         |
| -------- | ------- | ------------------------------- |
| `&&`   | AND     | `[[ $a -lt 10 && $b -gt 5 ]]` |
| `        |         | `                               |
| `!`    | NOT     | `[[ ! -f file.txt ]]`         |

### 🔹 4. AND operator `&&`

🔸 Example :

```bash
a=7
b=12

if [[ $a -gt 5 && $b -lt 15 ]]; then
  echo "Both conditions are true"
fi
```

### 🔹 4. NOT operator `!`

Example:

```bash
if [ ! -d /etc ]; then
  echo "/etc is NOT a directory"
fi

if [[ ! -f "data.txt" ]]; then
  echo "File does not exist"
fi
```

### 🔹 4. OR operator `||`

In Bash, when using `[[ ... ]]`, the logical **OR** operator is:   `||` (double pipe symbol)

✅ Example:

```bash
age=25

if [[ $age -lt 13 || $age -gt 19 ]]; then
  echo "Not a teenager"
else
  echo "Teenager"
fi
```

🔹 Explanation:

* If `age` is less than 13 **or** greater than 19 → `"Not a teenager"`
* If `age` is between 13 and 19 inclusive → `"Teenager"`

✅ Another Example with Strings:

```bash
read -p "Enter a character: " char

if [[ $char == "y" || $char == "Y" ]]; then
  echo "You said yes!"
else
  echo "You did not say yes."
fi
```

### 🔹 3. Key Differences: `[ ]` vs `[[ ]]`

| Feature                 | `[ ]`                | `[[ ]]`(Bash only)      |
| ----------------------- | ---------------------- | ------------------------- |
| Logical ops like `&&` | ❌ Not allowed         | ✅ Allowed directly       |
| Safer with strings      | ❌ (quotes needed)     | ✅ (less quoting needed)  |
| Pattern matching        | ❌                     | ✅ e.g.`[[ $x == a* ]]` |
| Recommended?            | ✔️ for POSIX scripts | ✔️ for Bash scripts     |

### ✅ Summary

| Task               | Example                           |
| ------------------ | --------------------------------- |
| AND with `[ ]`   | `[ "$a" -gt 0 -a "$b" -lt 10 ]` |
| OR with `[ ]`    | `[ "$a" -lt 5 -o "$b" -gt 8 ]`  |
| NOT with `[ ]`   | `[ ! -f "file.txt" ]`           |
| AND with `[[ ]]` | `[[ $a -gt 0 && $b -lt 10 ]]`   |
| OR with `[[ ]]`  | `[[ $a -gt 0 \|\| $b -lt 10 ]]`   |
| NOT with `[[ ]]` | `[[ ! -f "file.txt" ]]`         |
| Pattern matching   | `[[ $name == A* ]]`             |

---

# ----Arithmetic Operators

Arithmetic operations in shell are performed using:

* `let`
* `(( ))` (preferred)
* `expr` (older)
* `$(( ))` (for inline arithmetic)

### 🔹 **Basic Arithmetic Operators**

| Operator | Description         | Example (`a=10`,`b=3`) | Result |
| -------- | ------------------- | -------------------------- | ------ |
| `+`    | Addition            | `echo $((a + b))`        | 13     |
| `-`    | Subtraction         | `echo $((a - b))`        | 7      |
| `*`    | Multiplication      | `echo $((a * b))`        | 30     |
| `/`    | Division            | `echo $((a / b))`        | 3      |
| `%`    | Modulus (remainder) | `echo $((a % b))`        | 1      |
| `**`   | Exponentiation      | `echo $((a ** b))`       | 1000   |
| `++`   | Increment           | `((a++))`or `((++a))`  | 11     |
| `--`   | Decrement           | `((a--))`or `((--a))`  | 9      |

### 🔹 **Using Arithmetic in Bash**

##### ✅ Method 1: `(( ))` (most common)

```bash
a=5
b=3
((sum = a + b))
echo "Sum: $sum"   # Output: Sum: 8
```

##### ✅ Method 2: `$(( ))` (inline)

```bash
a=7
echo "Double: $((a * 2))"   # Output: Double: 14
```

##### ✅ Method 3: `let` command

```bash
let c=5+4
echo $c     # Output: 9
```

##### ✅ Method 4: `expr` (older, space-sensitive, avoid in modern scripts)

```bash
a=9
b=4
result=$(expr $a + $b)
echo $result   # Output: 13
```

### 🔸 Notes:

* **Use `(( ))`** for cleaner syntax and arithmetic logic.
* Inside `(( ))`, **no need to use `$`** for variable references:
  ```bash
  ((result = a * b))   # valid
  ((result = $a * $b)) # also valid, but `$` optional here
  ```

---

# ----For Loops

In Bash, `for` loops are used to iterate over a series of items or numbers. There are **two main types** of `for` loops:

### 🔹 1. **List-Style `for` Loop** (Used to iterate over items)

##### 🔸 Syntax:

```bash
for variable in item1 item2 item3 ...
do
    commands
done
```

🔸 Example:

```bash
for fruit in apple banana mango
do
    echo "I like $fruit"
done
```

🔸 Output:

```
I like apple
I like banana
I like mango
```

##### 🔸 Can be used with brace expansion:

```bash
for i in {1..5}
do
    echo "Number: $i"
done
```

### 🔹 2. **C-Style `for` Loop** (Like in C/Java)

##### 🔸 Syntax:

```bash
for (( initialization; condition; increment ))
do
    commands
done
```

🔸 Example:

```bash
for (( i=1; i<=5; i++ ))
do
    echo "Count: $i"
done
```

🔸 Output:

```
Count: 1
Count: 2
Count: 3
Count: 4
Count: 5
```

##### 🔸 You can also use:

```bash
for (( ; ; ))   # infinite loop
```

### 🔸 Notes:

* **List-style** is great for filenames, word lists, etc.
* **C-style** is better for numeric loops or controlled iteration.
* You **must** include `do` and `done` to define the loop block.

### ✅ Bonus: Looping through files

```bash
for file in *.txt
do
    echo "Processing $file"
done
```

---

# ----While Loop

The `while` loop in Bash repeatedly executes a block of code  **as long as a given condition is true** .

🔹 Syntax

```bash
while [ condition ]
do
    # commands
done
```

Or using `[[ ... ]]`:

```bash
while [[ condition ]]
do
    # commands
done
```

### 🔹 Example 1: Simple counter

```bash
count=1

while [ $count -le 5 ]
do
    echo "Count is $count"
    ((count++))  # or: count=$((count + 1))
done
```

🔸 Output:

```
Count is 1
Count is 2
Count is 3
Count is 4
Count is 5
```

### 🔹 Example 2: Reading user input

```bash
while true
do
    read -p "Enter a number (0 to quit): " num
    if [ "$num" -eq 0 ]; then
        echo "Goodbye!"
        break
    fi
    echo "You entered: $num"
done
```

### 🔹 Infinite `while` loop

```bash
while :
do
    echo "Press [CTRL+C] to stop"
    sleep 1
done
```

* `:` is a built-in no-op command that always returns true.
* `sleep 1` pauses the loop for 1 second.

### 🔹 Key Points

* `while` checks the condition **before** the loop starts each time.
* Use `break` to exit the loop early.
* Use `continue` to skip to the next iteration.

---

# ----Until loop

The `until` loop is  **similar to a `while` loop** , but with an  **opposite condition** .

🔁 **Syntax of `until` loop:**

```bash
until [ condition ]
do
    # code block
done
```

* The  **loop runs as long as the condition is false** .
* It  **stops when the condition becomes true** .

### ✅ **Example 1: Counting from 1 to 5**

```bash
#!/bin/bash

count=1

until [ $count -gt 5 ]
do
    echo "Count is: $count"
    ((count++))
done
```

📌 **Explanation:**

* The loop runs **until** `$count -gt 5` is  **true** .
* So it runs when count is  **1 to 5** , and stops when count is  **6** .

### ✅ **Example 2: Waiting for a file to exist**

```bash
#!/bin/bash

file="myfile.txt"

until [ -f "$file" ]
do
    echo "Waiting for $file to be created..."
    sleep 2
done

echo "$file is now available!"
```

📌 **Explanation:**

* The loop keeps checking if the file exists.
* It waits (sleeps) every 2 seconds.
* It exits the loop  **once the file is found** .

### 💡 `until` vs `while`

| Loop Type | Runs While...               | Stops When...                    |
| --------- | --------------------------- | -------------------------------- |
| `while` | Condition is**true**  | Condition becomes**false** |
| `until` | Condition is**false** | Condition becomes**true**  |

### ⚠️ Common mistake

Always  **include spaces inside the brackets** :

```bash
# ❌ Wrong:
until [$count -gt 5]

# ✅ Correct:
until [ $count -gt 5 ]
```

---

# ----Break and Continue

In shell scripting, `break` and `continue` are control flow statements used inside loops (`for`, `while`, `until`) to alter their normal execution.

### 🔹 `break` — Exit the loop completely

The `break` command  **immediately terminates the loop** , and control moves to the line **after** the loop.

✅ Example:

```bash
#!/bin/bash

count=1
while [ $count -le 10 ]
do
    if [ $count -eq 5 ]; then
        echo "Breaking at count = $count"
        break
    fi
    echo "Count: $count"
    ((count++))
done
```

**Output:**

```
Count: 1
Count: 2
Count: 3
Count: 4
Breaking at count = 5
```

### 🔹 `continue` — Skip the current iteration

The `continue` command **skips the rest of the loop body** for the current iteration and moves to the  **next iteration** .

✅ Example:

```bash
#!/bin/bash

for i in {1..5}
do
    if [ $i -eq 3 ]; then
        echo "Skipping $i"
        continue
    fi
    echo "Processing $i"
done
```

**Output:**

```
Processing 1
Processing 2
Skipping 3
Processing 4
Processing 5
```

### 🔹 Use with nested loops

In  **nested loops** , you can also use `break N` or `continue N` to apply the command to a specific level of nesting.

```bash
break 2   # breaks out of 2 levels of loops
continue 2   # skips to the next iteration of the 2nd outer loop
```

Here’s an example that shows **nested loops** using both `break N` and `continue N`:

🧪 **Example: Nested Loop with `break N` and `continue N`**

```bash
#!/bin/bash

for i in {1..3}; do
    echo "Outer loop i = $i"
    for j in {1..5}; do
        # Skip j = 3 in the inner loop
        if [ $j -eq 3 ]; then
            echo "  Skipping j = $j using continue 1"
            continue 1  # skips just the inner loop iteration
        fi

        # Break both loops if j = 4
        if [ $j -eq 4 ]; then
            echo "  Breaking out of both loops at j = $j using break 2"
            break 2  # breaks both outer and inner loops
        fi

        echo "  Inner loop j = $j"
    done
done

echo "Done"
```

🧾 **Output:**

```
Outer loop i = 1
  Inner loop j = 1
  Inner loop j = 2
  Skipping j = 3 using continue 1
  Breaking out of both loops at j = 4 using break 2
Done
```

**🔍 Explanation:**

* The outer loop runs `i = 1 to 3`.
* The inner loop runs `j = 1 to 5`.
* When `j = 3`, it **skips that iteration** of the inner loop using `continue 1`.
* When `j = 4`, it **exits both loops** using `break 2`.

---

# ----Looping over Strings, Files, Command output and Exit Codes

Looping is a core concept in shell scripting, and you can loop over:

* Files (e.g., every `.txt` file in a directory)
* Strings (e.g., every word in a sentence)
* Arrays (e.g., a list of values) (See in Arrays)

Let's break each of them down clearly.

### 🔁 1. **Looping Over Files**

You typically use a `for` loop with a pattern like `*.txt`:

```bash
for file in *.txt; do
  echo "Processing file: $file"
done
```

✅  **Use cases** :

* Batch rename
* Convert or process files
* File cleanup

🟠 Be cautious: Use double quotes to handle filenames with spaces:

```bash
for file in *.txt; do
  echo "File: \"$file\""
done
```

### 🔁 2. **Looping Over Strings (Words in a string)**

```bash
text="Linux Shell Scripting is powerful"

for word in $text; do
  echo "$word"
done
```

This splits the string into words based on  **spaces** .

If you want to loop over characters instead:

```bash
text="Hi!"
for (( i=0; i<${#text}; i++ )); do
  echo "${text:$i:1}"
done
```

### **🔁 3. Looping with Command Output**

```bash
for line in $(cat file.txt); do
  echo "$line"
done
```

> ⚠️ This splits on  **whitespace** , not lines. To loop line-by-line, prefer:

```bash
while IFS= read -r line; do
  echo "$line"
done < file.txt
```

### 🔁 4. Exit Codes in Loops

You can check command exit status within loops and react accordingly:

```bash
for cmd in "ls /" "ls /fake"; do
  $cmd
  if [ $? -ne 0 ]; then
    echo "Command failed: $cmd"
  fi
done
```

---

# ----Select Statement

✅ What is `select`?

The `select` statement in **Bash** is used to  **create a simple interactive menu** . It automatically:

* Displays options to the user.
* Prompts them to make a choice.
* Stores the selected value in a variable.

It works best in  **terminal-based scripts** .

🧠 Basic Syntax of `select`:

```bash
select variable in list
do
   commands
done
```

### 🔁 How `select` Works:

1. It displays a numbered list from the `list`.
2. The user inputs a **number** corresponding to an item.
3. The selected value is assigned to the `variable`.
4. The loop runs once per selection unless you use `break`.

### 📦 Example: Simple Menu Loop

```bash
#!/bin/bash

echo "Choose your favorite language:"
select lang in "Python" "JavaScript" "Bash" "Quit"
do
  case $lang in
    "Python")
      echo "You chose Python!"
      ;;
    "JavaScript")
      echo "You chose JavaScript!"
      ;;
    "Bash")
      echo "You chose Bash!"
      ;;
    "Quit")
      echo "Exiting..."
      break
      ;;
    *)
      echo "Invalid option. Try again."
      ;;
  esac
done
```

### 🧪 Output when running the script:

```
1) Python
2) JavaScript
3) Bash
4) Quit
#? 2
You chose JavaScript!
#? 4
Exiting...
```

### 🔧 Customizing `select`

* You can change the prompt:

  ```bash
  PS3="Enter your choice (number): "
  ```

  * > `PS3` stands for  **Prompt String 3** .
    >
    > It is a **special built-in variable in Bash** used specifically with the `select` loop.
    >
    > 🔍 Breakdown:
    >
    > * `PS1` → the main command prompt (what you usually see as `$` or `username@host$`)
    > * `PS2` → used when a command is multiline (default: `>`)
    > * **`PS3`** → used **only** for the `select` prompt
    > * `PS4` → used during debugging with `set -x`
    >
    > ##### ✅ In context:
    >
    > ```bash
    > PS3=">> "
    > select opt in "Add" "Remove" "Exit"
    > do
    >   echo "You chose: $opt"
    > done
    > ```
    >
    > When the menu is shown, `PS3` controls the prompt that asks for user input:
    >
    > ```
    > 1) Add
    > 2) Remove
    > 3) Exit
    >>> 1
    > You chose: Add
    > ```
    >
    > Here, `>> ` is the value of `PS3`.
    >
    > ##### 🔧 You can customize it:
    >
    > ```bash
    > PS3="Choose an option (1-3): "
    > ```
    >
    > Output:
    >
    > ```
    > 1) Add
    > 2) Remove
    > 3) Exit
    > Choose an option (1-3): 
    > ```
    >
* Example:

  ```bash
  PS3=">> "
  select opt in "Add" "Remove" "Exit"
  do
    echo "You chose: $opt"
    [[ $opt == "Exit" ]] && break
  done
  ```

> #### 💡 How it works:
>
> * `select` presents a  **menu** :
>   ```
>   1) Add
>   2) Remove
>   3) Exit
>   ```
> * It waits for you to enter a number (because of `PS3=">> "`):
>   ```
>   >> 
>   ```
>
> ##### ✅ Example Run & Output:
>
> Let's say this is the user's interaction:
>
> ```
> 1) Add
> 2) Remove
> 3) Exit
>>> 1
> You chose: Add
>>> 2
> You chose: Remove
>>> 3
> You chose: Exit
> ```
>
> Then the script exits because of:
>
> ```bash
> [[ $opt == "Exit" ]] && break
> ```
>
> ##### ⚠️ If an invalid number is entered:
>
> Let’s say you enter `5`, which isn’t in the menu:
>
> ```
>>> 5
> You chose:
> ```
>
> * `$opt` becomes empty.
> * Nothing is echoed after `You chose: `.
>
> You can handle this case by adding:
>
> ```bash
> if [[ -z "$opt" ]]; then
>   echo "Invalid option"
> fi
> ```

### ✅ Use Cases

* Menus in command-line tools
* Installers
* Simple configuration scripts
* Debug/test helpers

### ⚠️ Caveats

| Behavior                   | Note                                                  |
| -------------------------- | ----------------------------------------------------- |
| Input is a**number** | Not the value itself                                  |
| No validation              | You must handle invalid entries using `*`case       |
| Only Bash                  | `select`is not available in POSIX `sh`, only Bash |

---

# ----Arrays

Let's explore **arrays in Bash scripting** step by step with examples:

### 🔹 1. **Declaring Arrays**

✅ Indexed Arrays:

```bash
fruits=("apple" "banana" "cherry")
```

Each element gets an index starting from `0`.

✅ Declaring with explicit index:

```bash
fruits[0]="apple"
fruits[1]="banana"
fruits[2]="cherry"
```

### 🔹 2. **Accessing Elements**

* **Single Element:**

```bash
echo "${fruits[0]}"   # Output: apple
```

* **All Elements:**

```bash
echo "${fruits[@]}"   # Output: apple banana cherry
```

* **All Indices:**

```bash
echo "${!fruits[@]}"  # Output: 0 1 2
```

* **Length of array:**

```bash
echo "${#fruits[@]}"  # Output: 3
```

### 🔹 3. **Updating Elements**

```bash
fruits[1]="mango"
echo "${fruits[@]}"   # Output: apple mango cherry
```

### 🔹 4. **Deleting Elements**

```bash
unset fruits[2]
echo "${fruits[@]}"   # Output: apple mango
```

> Note: The array still has index 0 and 1; index 2 is just removed.

### 🔹 5. **Looping Through Arrays**

🔁 Using `for` loop (element-wise):

```bash
for fruit in "${fruits[@]}"; do
    echo "$fruit"
done
```

🔁 Using `for` loop with indices:

```bash
for i in "${!fruits[@]}"; do
    echo "Index $i = ${fruits[$i]}"
done
```

### 🔹 6. **Adding Elements**

You can add a new element at the next index:

```bash
fruits+=("grape")
```

### 🔹 7. **Multi-line Example: Full Usage**

```bash
#!/bin/bash

# Declare array
colors=("red" "green" "blue")

# Add new color
colors+=("yellow")

# Access specific color
echo "Second color: ${colors[1]}"

# Print all colors
echo "All colors: ${colors[@]}"

# Loop through colors with index
for i in "${!colors[@]}"; do
    echo "Index $i => ${colors[$i]}"
done

# Delete an element
unset colors[2]

# Print updated array
echo "After deletion: ${colors[@]}"
```

---

# ----Assosciative Array

🔹 What are Associative Arrays?

Associative arrays are like **dictionaries or maps** in other programming languages — instead of numeric indices (`0`, `1`, etc.), you use **named keys** (like `"name"`, `"email"`).

* They are available only in  **Bash version 4.0+** .

### 🔹 Declaring an Associative Array

```bash
declare -A my_array
```

Without `declare -A`, Bash treats it as an indexed array, so this declaration is  **mandatory** .

### 🔹 Assigning Values

```bash
my_array[name]="Arun"
my_array[age]="23"
my_array[city]="Chennai"
```

You’re assigning values using  **string keys** .

### 🔹 Accessing Values

```bash
echo "${my_array[name]}"    # Output: Arun
echo "${my_array[age]}"     # Output: 23
```

### 🔹 Looping Through Associative Arrays

### Loop through keys:

```bash
for key in "${!my_array[@]}"; do
    echo "$key => ${my_array[$key]}"
done
```

Output:

```
name => Arun
age => 23
city => Chennai
```

### 🔹 Get All Keys

```bash
echo "${!my_array[@]}"
# Output: name age city
```

### 🔹 Get All Values

```bash
echo "${my_array[@]}"
# Output: Arun 23 Chennai
```

### 🔹 Length of Associative Array

```bash
echo "${#my_array[@]}"
# Output: 3
```

### 🔹 Deleting Keys

```bash
unset my_array[age]
echo "${!my_array[@]}"  # Output: name city
```

### ✅ Full Example

```bash
#!/bin/bash

declare -A student

student[name]="Arun"
student[roll]="45"
student[course]="Linux Admin"

echo "Student Name: ${student[name]}"
echo "Roll Number: ${student[roll]}"
echo "Course: ${student[course]}"

# Print all key-value pairs
echo "Student Info:"
for key in "${!student[@]}"; do
    echo "$key => ${student[$key]}"
done
```

### ✅ When to Use Associative Arrays?

* When data is **key-value** based (like user details, settings, configs).
* When you need **named access** instead of position-based indexing.

---

# ----`declare`

✅ What is `declare` in Shell Scripting?

`declare` is a **Bash built-in command** used to define variables with specific **attributes** or **types** (like array, integer, readonly, etc.).

It helps make variables behave in certain ways, and also improves clarity, safety, and structure in your scripts.

🔹 Syntax:

```bash
declare [options] variable_name=value
```

### 🔸 Where is `declare` used?

* To **declare arrays** (indexed or associative)
* To **force variables to act as integers**
* To **make a variable readonly**
* To **export** a variable
* To **create name references (aliases)**
* To **enforce lowercase or uppercase values**

### 🔸 Examples:

##### 1. **Indexed Array**

```bash
declare -a fruits=("apple" "banana" "cherry")
echo ${fruits[1]}   # Output: banana
```

##### 2. **Associative Array**

```bash
declare -A person
person[name]="Arun"
person[age]=25
echo ${person[name]}  # Output: Arun
```

##### 3. **Integer**

```bash
declare -i num
num=5+5
echo $num       # Output: 10
```

> `declare -i` tells  **Bash to treat the variable as an integer** .
>
> ➡️ Any arithmetic operation assigned to it will be  **evaluated automatically** .
>
> ##### ✅ **Use Case Scenario: Auto Arithmetic Evaluation**
>
> 🔸 Without `declare -i`:
>
> ```bash
> a=5+2
> echo "$a"   # Output: 5+2 (it's just a string, not evaluated)
> ```
>
> 🔸 With `declare -i`:
>
> ```bash
> declare -i a
> a=5+2
> echo "$a"   # Output: 7 (evaluated automatically)
> ```
>
> ##### ✅ **Use Case Scenarios in Scripts**
>
> ###### 1. **Simpler math in scripts**
>
> Avoid writing `$(( ))` every time:
>
> ```bash
> declare -i count=0
> count+=1   # count = count + 1
> count+=5*2 # count = count + 10
> echo $count  # Output: 11
> ```
>
> ###### 2. **Avoid accidental string assignment**
>
> If someone does:
>
> ```bash
> declare -i num
> num="abc"     # Output: 0 (non-numeric value becomes 0)
> echo "$num"
> ```
>
> This helps when you're expecting only numeric values, e.g., counters or calculations.
>
> ##### ⚠️ Important Note:
>
> * You can still use `let`, `expr`, or `$(( ))` for manual control.
> * But `declare -i` makes **every assignment** auto-evaluate as arithmetic.

##### 4. **Readonly**

```bash
declare -r pi=3.14
pi=3.15     # Error: cannot assign to readonly variable
```

##### 5. **Uppercase**

```bash
declare -u shout
shout="hello"
echo $shout   # Output: HELLO
```

##### 6. **Lowercase**

Automatically converts assigned values to  **lowercase** .

```bash
declare -l name
name="HELLO"
echo $name    # Output: hello
```

##### 7. **Export**

Exports the variable to be available in child processes.

```bash
declare -x PATH="/usr/bin:$PATH"
```

Equivalent to: `export PATH=...`

##### 8. **Nameref (reference variable)**

```bash
name="Arun"
declare -n ref=name
echo $ref     # Output: Arun
ref="Ram"
echo $name    # Output: Ram
```

### 🔸 Check the type and value of a variable:

The `declare -p` command is used to **print the attributes and values** of a variable in Bash.

It helps you inspect whether a variable is an  **array, integer, readonly** , etc.

✅  **Syntax** :

```bash
declare -p variable_name
```

✅  **What it shows** :

* The type (like `-a` for array, `-A` for associative array, `-r` for readonly, etc.)
* The variable name
* The current value

##### 🔸  **Examples** :

Example 1: Simple Variable

```bash
name="Arun"
declare -p name
```

**Output:**

```bash
declare -- name="Arun"
```

➡️ `--` means it has no special attributes.

Example 2: Indexed Array

```bash
declare -a colors=("red" "blue" "green")
declare -p colors
```

**Output:**

```bash
declare -a colors='([0]="red" [1]="blue" [2]="green")'
```

➡️ `-a` indicates it's an  **indexed array** .

Example 3: Associative Array

```bash
declare -A user
user[name]="Ram"
user[age]=23
declare -p user
```

**Output:**

```bash
declare -A user='([name]="Ram" [age]="23")'
```

➡️ `-A` shows it's an  **associative array** .

##### ✅ Use Case:

This is **super useful for debugging** scripts — especially when working with arrays or when you're unsure of a variable’s type or value.

### 🔸 Important Notes:

* `declare` only works in  **Bash** , not in `/bin/sh`.
* It is mainly used in **scripts** to manage variables more clearly.
* Useful for  **type-safety** ,  **readability** , and  **debugging** .

### 🔸 Summary Table

| Flag   | Description             |
| ------ | ----------------------- |
| `-a` | Indexed array           |
| `-A` | Associative array       |
| `-i` | Integer                 |
| `-r` | Readonly (constant)     |
| `-x` | Export (to environment) |
| `-l` | Force lowercase         |
| `-u` | Force uppercase         |
| `-n` | Name reference (alias)  |

---

# ----Functions

Let's break down **functions in Bash scripting** in complete detail.

✅ What is a Function in Bash?

A **function** is a block of reusable code that performs a specific task and can be called (invoked) anywhere in your script.

### 🧱 1. **Function Declaration (2 styles)**

Style 1: Recommended

```bash
my_function() {
    echo "Hello from function!"
}
```

Style 2: With `function` keyword (also valid)

```bash
function my_function {
    echo "Hello from function!"
}
```

### 🟡 2. **Calling a Function**

Just write the name:

```bash
my_function
```

### 🧾 3. **Passing Parameters to Functions**

Bash functions receive **positional parameters** just like scripts.

```bash
greet() {
    echo "Hello $1, you are $2 years old."
}

greet "Arun" 22
# Output: Hello Arun, you are 22 years old.
```

* `$1`, `$2`, `$3`... → represent the arguments
* `$@` → all arguments
* `$#` → number of arguments

### 🔁 4. **Returning Values from Functions**

✅ Best Practice: Use `echo` to return output

```bash
add() {
    sum=$(( $1 + $2 ))
    echo "$sum"
}

result=$(add 5 3)
echo "The sum is $result"
```

### ⚠️ Note:

You  **should not use `return` to send back values** , as:

* `return` is only for **exit status (0–255)**
* Not for passing data

```bash
check_even() {
    if (( $1 % 2 == 0 )); then
        return 0  # success
    else
        return 1  # failure
    fi
}
```

Then you check with:

```bash
check_even 4
if [ $? -eq 0 ]; then
    echo "Even"
else
    echo "Odd"
fi
```

### 🌐 5. **Variable Scope in Functions**

🟢 Local Variables (Using `local`)

```bash
myfunc() {
    local x=10  # Only accessible inside this function
    echo "x = $x"
}
```

If you **don’t** use `local`, the variable is **global** (accessible anywhere).

In  **Bash** , all variables inside a function are  **global by default** , unless explicitly declared as  **`local`** .

##### 🔍 Here's how it works:

🔴 Without `local` (Global Scope):

```bash
set_name() {
    name="Arun"   # global variable
}

set_name
echo "$name"     # Output: Arun (accessible outside)
```

🟢 With `local` (Function Scope):

```bash
set_name() {
    local name="Arun"
    echo "Inside: $name"
}

set_name
echo "Outside: $name"  # Output: (empty, name doesn't exist globally)
```

##### ⚠️ Why this matters:

If you don't use `local`,  **functions can unintentionally overwrite global variables** , leading to hard-to-debug scripts.

##### 🧠 Rule of Thumb:

* Use `local` inside every function **unless** you **intentionally** want to modify a global variable.
* Prefer `local` for temporary values, counters, strings, or logic used only within the function.

### 🧪 6. **Example: All Together**

```bash
greet() {
    local name=${1:-"Guest"}
    local age=${2:-0}
  
    echo "Welcome $name!"
  
    if (( age < 18 )); then
        echo "You're a minor."
    else
        echo "You're an adult."
    fi
}

greet "Arun" 21
```

### 🔸 6. **Named Parameters (Emulated)**

Since bash doesn't support named parameters directly, learn to make it readable:

```bash
print_user_info() {
  local name="$1"
  local age="$2"
  echo "$name is $age years old"
}
```

### 🔸 7. **Function with Arrays (Pass/Return)**

```bash
print_array() {
  local arr=("$@")
  for item in "${arr[@]}"; do
    echo "Item: $item"
  done
}
print_array "one" "two" "three"
```

Returning an array is trickier (involves global vars or command substitution).

### 🔸 8. **Recursive Functions**

Bash supports recursion but with limits (stack size).

```bash
factorial() {
  local n=$1
  if (( n <= 1 )); then
    echo 1
  else
    echo $(( n * $(factorial $((n - 1))) ))
  fi
}
factorial 5  # 120
```

### 🔸 9. **Sourcing Function Libraries**

Modularize your functions:

```bash
# utils.sh
log_info() {
  echo "[INFO] $1"
}

# main.sh
source ./utils.sh
log_info "This is a message"
```

> #### 🔧 What is  *Sourcing* ?
>
> **Sourcing** means running another shell script **within the current shell** instead of launching a new shell process.
>
> ✅ You do this using:
>
> ```bash
> source file.sh
> # OR the shorthand
> . file.sh
> ```
>
> When you source a file, all its **variables, functions, and settings become available** in your current script.
>
> 🧠 Why Source Function Libraries?
>
> To  **organize** ,  **reuse** , and **maintain** your functions across multiple scripts, just like importing modules in other languages (like Python or JS).
>
> ##### ✅ Example: Creating & Sourcing a Function Library
>
> ###### Step 1: Create `utils.sh` (your function library)
>
> ```bash
> #!/bin/bash
>
> log_info() {
>   echo "[INFO] $1"
> }
>
> log_error() {
>   echo "[ERROR] $1" >&2
> }
>
> get_timestamp() {
>   date "+%Y-%m-%d %H:%M:%S"
> }
> ```
>
> ###### Step 2: Use it in your main script (`main.sh`)
>
> ```bash
> #!/bin/bash
>
> # Source the utility file
> source ./utils.sh  # or `. ./utils.sh`
>
> log_info "Script started at $(get_timestamp)"
> log_error "Something went wrong"
> ```
>
> ✅ Now, all functions from `utils.sh` are available in `main.sh`.
>
> ##### ⚠️ Important Notes
>
> | Concept               | Explanation                                                                                                                             |
> | --------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
> | `source`vs `bash` | `source`runs in **same shell** ,`bash`spawns a **new subshell** . Use `source`to keep variables/functions accessible. |
> | Path                  | Use relative (`./utils.sh`) or absolute paths depending on where the file is.                                                         |
> | Execution Permission  | **Not required**for sourced files, since they are not executed directly.                                                          |
>
> ##### 🧠 Real-World Usage
>
> Break your scripts into:
>
> * `functions/network.sh`
> * `functions/fileops.sh`
> * `functions/logging.sh`
>
> Then in your main script:
>
> ```bash
> source ./functions/logging.sh
> source ./functions/fileops.sh
>
> log_info "Starting backup..."
> backup_files
> ```

### 🔸 10. **Function Overloading / Nesting**

Shell doesn’t support overloading like in other languages.

But you can emulate logic based on args:

```bash
run() {
  if [[ "$1" == "build" ]]; then
    build_code
  elif [[ "$1" == "test" ]]; then
    run_tests
  fi
}
```

> #### ❓ Where does **overloading** happen in shell functions?
>
> #### Short Answer:
>
>> **Shell does NOT support real function overloading** (like in C++/Java where two functions can have the same name but different parameters).
>>
>
> Instead, what you’re doing is:
>
> * Defining **one function**
> * Adding **logic inside it** to handle **different numbers or types of arguments**
>
> This  **emulates overloading** , but it's not truly overloading.
>
> ##### 🔍 Example of Emulated "Overloading"
>
> ```bash
> greet() {
>   if [ $# -eq 0 ]; then
>     echo "Hello!"
>   elif [ $# -eq 1 ]; then
>     echo "Hello, $1!"
>   else
>     echo "Hello, $1 and $2!"
>   fi
> }
> ```
>
> Here:
>
> * You’re not overloading `greet` with multiple definitions.
> * Instead, you're checking how many arguments are passed and behaving accordingly.
>
> #### So, the “overloading” is  **inside the function body** , not at declaration.
>
> ##### 🧠 Why can't Bash do real overloading?
>
> Because Bash is an interpreted, loosely-typed scripting language:
>
> * Functions are identified **only by name**
> * You **can’t define multiple functions** with the same name, even if they take different arguments
>
> If you try:
>
> ```bash
> myfunc() {
>   echo "Version 1"
> }
>
> myfunc() {
>   echo "Version 2"
> }
> ```

---

# ----Error Handling and Exit Status

Let's clearly break down  **Error Handling and Exit Status in Shell Scripting** , including `$?`, `exit`, `trap`, and signal handling.

### ✅ 1. **Exit Status (`$?`)**

In shell scripting, **every command** returns an  **exit status** :

* `0` means **success**
* Any **non-zero** value means **failure**

### 🔹 Example:

```bash
ls /home/user
echo "Exit Status: $?"  # Prints 0 if ls worked
```

```bash
ls /nonexistent-folder
echo "Exit Status: $?"  # Prints non-zero (e.g., 2) if folder doesn't exist
```

You can use this to **check if the previous command succeeded** and take action:

```bash
cp file1.txt /backup/
if [[ $? -ne 0 ]]; then
  echo "Backup failed!"
fi
```

> #### ❗Do we need **to assign the result** of `ls /home/user` to a variable in order to use `$?` ?
>
> **You do *not* need to assign the result** of `ls /home/user` to a variable in order to use `$?`. Here's why:
>
> ✅ `$?` automatically stores the exit status of the  **most recently executed command** .
>
> So this works perfectly:
>
> ```bash
> ls /home/user
> echo "Exit Status: $?"
> ```
>
> * `ls /home/user` runs.
> * Immediately after that, `$?` holds its  **exit status** , not the output.
> * `echo` prints that status.
>
> ❌ But this will not work if you run another command in between:
>
> ```bash
> ls /home/user
> echo "Some message"
> echo "$?"   # This will now show the exit code of the `echo` above, not `ls`
> ```
>
> ##### ✅ Example with assignment (just for clarity):
>
> Even if you *do* store the output, the exit code is still based on the last command:
>
> ```bash
> result=$(ls /home/user)
> echo "Exit Status: $?"  # Still works, because the command inside $() is what gets run
> ```
>
> But again, you don’t need to store anything if all you care about is whether the command **succeeded** or  **failed** .
>
> ##### ⚠️ Rule of Thumb:
>
>> Use `$?` **immediately after** the command you want to check.
>>
>> Don't run any other command before checking it.
>>

### ✅ 2. **`exit` Command and Status Codes**

The `exit` command **ends a script or function** with a specific exit code.

```bash
exit 0   # Normal exit
exit 1   # General error
exit 2   # Misuse of shell built-ins
```

Use custom exit codes to communicate what went wrong:

```bash
if [[ ! -f config.txt ]]; then
  echo "Missing config file"
  exit 2
fi
```

> You can then access this exit code from the outside using `$?`.

> #### **❗ Why not use `return` instead of `exit` when handling errors or statuses in a script?**
>
> Here's a  **clear breakdown** :
>
> ##### 🔁 `return` vs `exit` – What's the Difference?
>
> | Feature               | `return`                  | `exit`                          |
> | --------------------- | --------------------------- | --------------------------------- |
> | **Used inside** | Functions only              | Entire shell script (or function) |
> | **Effect**      | Exits the**function** | Exits the**entire script**  |
> | **Sets**        | Function exit status        | Script exit status                |
> | **Scope**       | Stays inside the script     | Ends the script immediately       |
>
> ✅ Use `return`:
>
> When you're **inside a function** and want to give an **exit code back to the caller** (but continue the script).
>
> ```bash
> myFunc() {
>   echo "Inside function"
>   return 2
> }
>
> myFunc
> echo "Exit status of function: $?"   # Outputs: 2
> ```
>
> ✅ Use `exit`:
>
> When you want to  **terminate the entire script** , usually on a **fatal error** or when the job is done.
>
> ```bash
> echo "Before exit"
> exit 1
> echo "This won't print"
> ```
>
> ##### 🚫 Wrong Usage Examples:
>
> ```bash
> # Using return outside a function - ❌ Wrong
> return 1
> # Error: bash: return: can only `return' from a function or sourced script
> ```
>
> ```bash
> # Using exit inside a function when you don't want to stop the script - 🚫 Risky
> myFunc() {
>   echo "Some error"
>   exit 1  # Script will stop here!
> }
> ```

### ✅ 3. **Using `trap` and Signal Handling**

`trap` allows you to **run commands when the script receives a signal** or exits.

🔹 Syntax:

```bash
trap 'commands' SIGNAL
```

### 🔹 Common Signals:

| Signal | Name    | Description                     |
| ------ | ------- | ------------------------------- |
| 2      | SIGINT  | Ctrl+C                          |
| 15     | SIGTERM | Termination                     |
| 0      | EXIT    | When script ends (success/fail) |

### 🧪 Example: Trap Ctrl+C (SIGINT)

```bash
cleanup() {
  echo "Script interrupted. Cleaning up..."
  rm -f temp.txt
  exit 1
}

trap cleanup SIGINT

# Simulate long task
echo "Working... Press Ctrl+C to interrupt"
sleep 20
```

If you press  **Ctrl+C** , it runs `cleanup`, deletes `temp.txt`, and exits with status 1.

>> **"Trap Ctrl+C (SIGINT)"** – what does it really mean?
>>
>
> Let’s break it down clearly.
>
> ✅ What is `Ctrl + C`?
>
> When you press `Ctrl + C` in the terminal:
>
> * It sends a **signal** called `SIGINT` (Signal Interrupt) to the running process.
> * This usually **kills** the script immediately.
>
> #### 🔒 Trap to Handle Ctrl+C (`SIGINT`)
>
> With `trap`, you can **catch this signal** and tell the script what to do  **instead of just dying immediately** .
>
> ✨ Example:
>
> ```bash
> #!/bin/bash
>
> trap "echo '👋 Script interrupted by user (Ctrl+C)'; exit 1" SIGINT
>
> echo "Running a task... (press Ctrl+C to interrupt)"
> sleep 10
> echo "Task completed!"
> ```
>
> ##### 🔍 What happens here?
>
> * When you press `Ctrl + C` during the 10-second sleep:
>   * Normally, the script would stop immediately.
>   * But because we  **trapped SIGINT** , it first runs this:
>     ```bash
>     echo '👋 Script interrupted by user (Ctrl+C)'; exit 1
>     ```
>   * Then the script exits **gracefully** with status 1.
>
> ##### 🧠 Common Use Case of `trap`
>
> | Signal      | Typical Meaning                        | Usage                                 |
> | ----------- | -------------------------------------- | ------------------------------------- |
> | `SIGINT`  | User pressed Ctrl+C                    | Gracefully clean up & exit            |
> | `SIGTERM` | `kill`command sent to process        | Clean up before shutting down         |
> | `EXIT`    | Script is about to finish (any reason) | Perform general cleanup on script end |
>
> ##### 🧹 Cleanup Example with `EXIT`:
>
> ```bash
> #!/bin/bash
>
> trap "echo 'Cleaning up...'; rm -f temp.txt" EXIT
>
> touch temp.txt
> echo "Temporary file created. It will be deleted automatically."
> sleep 5
> ```
>
> This ensures the file is deleted whether the script:
>
> * Completes normally
> * Is interrupted by `Ctrl + C`
>
> ##### 🔒 What Signal Can be Trapped ?
>
> | Signal      | Meaning                    | Can Be Trapped?   | Default Action         |
> | ----------- | -------------------------- | ----------------- | ---------------------- |
> | `SIGKILL` | Immediately kill a process | ❌ No             | Terminates immediately |
> | `SIGSTOP` | Stop (pause) a process     | ❌ No             | Stops process          |
> | `SIGCONT` | Continue a stopped process | ✅ No trap needed | Resumes process        |

### 🧪 Trap on Script Exit (EXIT)

```bash
trap 'echo "Script exited with code $?."' EXIT

echo "Doing something"
exit 5
```

This prints: `Script exited with code 5.`

---

# ----`set` Command

The `set` command in Bash is used to  **change the behavior of your script or shell session** . It takes different **flags (options)** that control how errors and variables are treated. Let's go over the important ones in  **error handling and script robustness** :

### 🔹 `set -e` → **Exit immediately on error**

* If any command returns a  **non-zero exit status** , the script will stop execution.
* Useful in **deployment scripts** or **critical operations** to avoid continuing after a failure.

**Example:**

```bash
set -e

echo "Start"
cp nonexistent.txt /tmp/     # This fails, script exits here
echo "End"                   # This will not run
```

### 🔹 `set -u` → **Treat unset variables as errors**

* If your script tries to use a variable that  **has not been defined** , it will throw an error and exit.
* Prevents bugs caused by  **typos or missing arguments** .

**Example:**

```bash
set -u

echo "User is $USER"   # OK
echo "Name is $NAME"   # If NAME is not defined, error and exit
```

### 🔹 `set -o pipefail` → **Fail if any command in a pipeline fails**

* Normally, in a pipeline (`cmd1 | cmd2 | cmd3`), Bash only checks the  **exit code of the last command** .
* With `pipefail`, the **whole pipeline fails** if *any* part of it fails.
* `-o` for **options**

**Example:**

```bash
set -o pipefail

false | true             # Fails with pipefail, otherwise succeeds
echo $?                  # Shows 1 instead of 0
```

> ##### 🔸 Why only the last command by default?
>
> In a pipeline,  **each command runs in its own subprocess** , and by default Bash only returns the **exit status of the *last* command** in the pipeline (`cmd3` here). This is mainly for  **historical and practical reasons** , because usually the final output is what matters.
>
> ##### 🔸 Now The Problem Without pipefail
>
> Consider this:
>
> ```bash
> cat nonexistent.txt | grep "foo"
> ```
>
> * `cat nonexistent.txt` → fails (exit code 1)
> * `grep "foo"` → runs fine, but gets no input and exits 1 too
>
> So you might *see* the failure. But if it were:
>
> ```bash
> cat nonexistent.txt | grep "foo" | sort
> ```
>
> * `cat` fails (maybe critical), but
> * `grep` and `sort` succeed (even on empty input),
> * So `echo $?` would show `0` —  **a misleading success** !
>
> ##### 🔸 The Solution: `set -o pipefail`
>
> This tells Bash:
>
>> “If *any* command in the pipeline fails, treat the whole pipeline as failed.”
>>
>
> So now:
>
> ```bash
> set -o pipefail
> cat file.txt | grep "foo" | sort
> echo $?   # now reflects failure if grep OR cat fails
> ```
>
> ✅ This gives you accurate error detection — especially important in scripts where you rely on intermediate steps.

### 🔹 Combine them: Safe scripting setup

Many scripts start with:

```bash
set -euo pipefail
```

Equivalent to:

```bash
set -e
set -u
set -o pipefail
```

This ensures:

* ✅ You don’t ignore failed commands.
* ✅ You catch undefined variables.
* ✅ You don’t miss errors in pipes.

---

# ----Try-Catch in Bash

Bash does **not** have native `try/catch` blocks like many high-level languages (e.g., Python or JavaScript), but we can **simulate** this behavior using functions, subshells, and exit statuses. Let's break it down clearly:

### 🧪 TRY-CATCH in Bash (Simulated)

##### ✅ Method 1: Using Subshells (`( )`)

```bash
try() {
  "$@"
  return 0
}

catch() {
  echo "Caught an error!"
}

# Try block
try ls non_existing_file || catch
```

 **Explanation** :

* `try` runs a command.
* If it **fails** (`$?` is non-zero), the `catch` function is called.
* `||` handles the "if-failed" logic.

> Here's what happens step by step:
>
> 1. ✅ `try` is a **function** that simply **runs the command** passed to it:
>
>    ```bash
>    try() {
>      "$@"
>      return 0
>    }
>    ```
>
>    * `"$@"` means: run whatever arguments were passed to the function — in this case, `ls non_existing_file`.
>    * If it  **succeeds** , nothing else happens.
> 2. ❌ If the `ls non_existing_file` **fails** (i.e., returns non-zero exit code), then:
>
>    * the `try` function also returns non-zero (even though it has `return 0`, that only happens  **if the command succeeds** ).
>    * Then `|| catch` is triggered.
> 3. So `catch` will run only  **if the `try` block (i.e., the command) fails** .
>
> ##### ✅ Real-world view:
>
> You’re essentially simulating this:
>
> ```bash
> if ! ls non_existing_file; then
>    echo "Caught an error!"
> fi
> ```
>
> But writing it as:
>
> ```bash
> try ls non_existing_file || catch
> ```
>
> makes it **look cleaner and more like real try/catch logic** from other languages.

##### ✅ Method 2: Using `trap` in a Function

You can simulate more advanced flow using `trap` to "catch" failures:

```bash
try_catch() {
  {
    echo "Trying risky command..."
    false       # simulate failure
    echo "This won't print if above fails"
  } || {
    echo "Caught error in try_catch function"
  }
}

try_catch
```

This works due to `||` chaining between grouped commands `{ ... }`.

##### ✅ Method 3: Using `set -e` and `trap` (Global-level)

```bash
trap 'echo "Something went wrong at line $LINENO!"' ERR
set -e

echo "Before error"
ls nonexistent_file  # This will trigger trap
echo "After error"    # Will not be executed
```

* `set -e`: causes the script to exit on error.
* `trap ... ERR`: runs when any command fails.

---

# ----Error Logging in Bash

##### ✅ Logging a single command’s stderr:

```bash
ls /fake/path 2>>error.log
```

* `2>>error.log`: appends error (`stderr`) to `error.log`

##### ✅ Logging stderr globally in a script:

```bash
#!/bin/bash
exec 2>error.log   # All future errors go to this file

echo "Starting script"
ls /fake/path
echo "Continuing script"
```

##### ✅ Logging stdout + stderr:

```bash
some_command &>>log.txt     # Bash 4+
# or
some_command >>log.txt 2>&1 # Compatible with older shells
```

---

# ----exec

The `exec` command in shell scripting is a powerful and somewhat low-level command with  **multiple uses** , depending on how it's invoked.

Let’s break it down clearly:

### ✅ **Main Uses of `exec`**

##### 1. **Replacing the current shell process**

```bash
exec ls -l
```

* This replaces the current shell  **with the `ls -l` command** .
* After `ls -l` finishes, the shell **does not continue** — because it's been replaced.
* Useful for optimizing memory or launching a final command (like in `init` systems or Docker containers).

🔸  **Example** :

```bash
#!/bin/bash
echo "This will be shown"
exec ls
echo "This will NOT be shown"  # Never executed
```

##### 2. **Redirecting input/output**

`exec` can be used to **redirect file descriptors (FDs)** — like stdin (0), stdout (1), stderr (2).

➤ Redirecting all output of a script to a file:

```bash
exec > output.txt
echo "This will go to output.txt"
```

➤ Redirect stderr:

```bash
exec 2> errors.log
```

➤ Close file descriptor (e.g., stdin):

```bash
exec 0<&-
```

➤ Read from a file using a new FD:

```bash
exec 3< myfile.txt
read -u 3 line
echo "Read from fd 3: $line"
exec 3<&-  # Close FD 3
```

##### 3. **Running a command using a specific file descriptor**

You can use `exec` to open a file on a custom FD and pass it around.

🔸  **Example** : Writing to a log file using FD 3

```bash
exec 3> log.txt
echo "Logging to file" >&3
exec 3>&-   # Close FD 3
```

---

# **----Here Document (heredoc)**

📘 **Here Documents (`<<EOF`) in Bash**

A **Here Document (heredoc)** is a way to **pass multiple lines of input** (usually to a command like `cat`, `tee`, `ssh`, etc.) directly  **within a script or terminal** ,  **without using a separate file** .

🧠 Syntax:

```bash
command <<EOF
line 1
line 2
...
EOF
```

* `EOF` is just a **delimiter** (can be any word, e.g., `END`, `INPUT`)
* Text between `<<EOF` and `EOF` is passed as **standard input** (stdin) to the command.

### ✅ Example 1: Using `cat` to create a file

```bash
cat <<EOF > myfile.txt
This is line 1
This is line 2
EOF
```

**Creates `myfile.txt`** with the contents:

```
This is line 1
This is line 2
```

### ✅ Example 2: With a shell script

```bash
#!/bin/bash

read -p "Enter your name: " name

cat <<EOF
Hello, $name!
Welcome to the system.
EOF
```

> ##### ❓ Here does `cat <<EOF ... EOF` create any file?
>
> **No** , it does **not** create any file.
>
> 🔍 Why?
>
> Let’s understand what’s happening:
>
> * The command `cat <<EOF` is using a **"here document"** (heredoc), which **feeds multiline input directly into the `cat` command** via **stdin** (standard input).
> * `cat` just **prints** what it receives — it **doesn’t write to a file** unless you tell it to.
>
> ✅ Example:
>
> ```bash
> cat <<EOF
> This is a test
> EOF
> ```
>
> 🟢  **What happens** :
>
> * Output is printed to the terminal.
> * No file is created.
>
> ##### ✏️ Want to write to a file?
>
> Yes, you **can** write the heredoc output to a file by  **redirecting the output** :
>
> ```bash
> cat <<EOF > output.txt
> This will go into a file.
> EOF
> ```
>
> 🟢 Now, the content will be written to `output.txt`.
>
> Summary:
>
> | Command                  | File Created? | Action                        |
> | ------------------------ | ------------- | ----------------------------- |
> | `cat <<EOF`            | ❌ No         | Prints text to terminal       |
> | `cat <<EOF > file.txt` | ✅ Yes        | Writes text into `file.txt` |
>
> ##### ❓ Why `cat` used instead of `echo`
>
> ✅ **Example:**
>
> ```bash
> #!/bin/bash
> read -p "Enter your name: " name
>
> cat <<EOF
> Hello, $name! Welcome to the system.
> EOF
> ```
>
> You could indeed write:
>
> ```bash
> echo "Hello, $name! Welcome to the system."
> ```
>
> So why use `cat <<EOF`?
>
> ##### 🧠  **When `echo` is enough** :
>
> For  **one-liners or simple strings** , `echo` (or `printf`) is simpler and cleaner:
>
> ```bash
> echo "Hello, $name! Welcome to the system."
> ```
>
> ✅ Use `echo` if:
>
> * It's just one line
> * No complex formatting
> * No indentation or multiline logic
>
> ##### 🚀  **When `cat <<EOF` is better** :
>
> Use a **heredoc** (`cat <<EOF`) when you need:
>
> 1. **Multiple lines** of output:
>
>    ```bash
>    cat <<EOF
>    Hello, $name!
>    This is a multi-line welcome message.
>    - Current date: $(date)
>    EOF
>    ```
> 2. **Indentation and formatting** preserved:
>
>    * `echo` struggles with that.
>    * Heredoc keeps line breaks, tabs, spacing intact.
> 3. **Complex templates** like config files, HTML, etc.:
>
>    ```bash
>    cat <<EOF > welcome.html
>    <html>
>      <body>
>        <h1>Welcome, $name!</h1>
>      </body>
>    </html>
>    EOF
>    ```
> 4. **Avoiding command substitution issues** :
>
>    Heredocs can escape variables (`<<'EOF'`) or preserve them as needed.
>
> 🧪 Bonus: Using heredoc with commands
>
> You can also  **pipe the heredoc to another command** , which echo doesn’t do as cleanly:
>
> ```bash
> ssh user@host <<EOF
> cd /var/www
> ls -l
> EOF
> ```
>
> (Explanation for this below Example 3)
>
> Or:
>
> ```bash
> grep "error" <<EOF
> line one
> error on line two
> another line
> EOF
> ```

### ✅ Example 3: With a shell script

```bash
ssh user@host <<EOF
cd /var/www
ls -l
EOF
```

This tells the shell:

* Start the `ssh user@host` command.
* Provide the following input (until `EOF`) as the **standard input** to that SSH session.

##### 🔧 How it works:

* `ssh user@host` is run.
* Everything between `<<EOF` and the ending `EOF` is passed  **as if you typed it into that remote terminal** .

##### 🧪 What actually happens:

This will:

* Open an SSH session to the remote host.
* Run `cd /var/www`
* Then run `ls -l`
* Then exit.

✅ It’s like remotely scripting a shell session.

### Variables inside heredoc are expanded by default:

* `$name` will be substituted with user input.

### 🔒 Disable variable expansion using  **quotes** :

```bash
cat <<'EOF'
Hello, $USER
EOF
```

Output:

```
Hello, $USER
```

### 🔹 Why we usually use `<<EOF` in shell scripts (Here Documents), although any names can be usd:

The full form of **EOF** is  **End Of File** .

In Shell Scripting and Here Documents:

* When used as `<<EOF`, it **marks the beginning** of a  **here document** , where input is redirected until a matching ending word (often `EOF`) is found.
* `EOF` itself is **just a label** — it doesn't have to be the word `EOF`. You can name it anything, like:
  ```bash
  cat <<END
  This is a test
  END
  ```

##### In Programming (C/C++, Python, etc.):

* **EOF** is a special constant used to indicate that the **end of a file has been reached** when reading input.
* For example, in C:
  ```c
  while ((c = getchar()) != EOF) {
      // Process character
  }
  ```

The term **EOF (End Of File)** is named that way because of its **original purpose in programming and input processing** — to  **signal the end of input** .

In early programming (e.g., C, Unix utilities), when reading from a file or from input (like keyboard or terminal), the system needed a way to  **know when the input is finished** .

So they introduced a symbolic value: `EOF` (End Of File), which tells the program:

> ✅ “There is **no more data** to read. Stop here.”

It is **not a string** but rather a **signal or special marker** internally represented (e.g., `-1` in C).

###### In a shell  **here document** , you're simulating  **input being fed into a command** , and you need to mark the **end** of that input. So:

```bash
cat <<EOF
Hello
World
EOF
```

The shell sees:

* `<<EOF`: “Start taking all the next lines as input”
* `EOF`: “Stop here — this is the End Of File”

It’s just a  **convention** , and you can actually use any word instead of `EOF`:

```bash
cat <<ENDINPUT
This is input
To the cat command
ENDINPUT
```

But `EOF` stuck as the **most commonly used token** because it's short, meaningful, and historically consistent with programming terminology.

### 📘 **Why is it called a "here document"?**

The name **"here document"** comes from the idea that:

> 🔸 *The document (or input) is written **here** in the script, rather than being read from a separate file.*

It originates from **Unix shell scripting** and programming traditions where data was usually read from external files or passed in via the terminal.

So, instead of:

```bash
cat some_file.txt
```

You're doing:

```bash
cat <<EOF
This is the content
written right here
EOF
```

Hence the term:

> 🗒️ **“Here is the document”** → **Here Document**

### 🧠  **Conceptual Analogy** :

| Traditional way     | Here document                                     |
| ------------------- | ------------------------------------------------- |
| Reads from a file   | Provides input inline, right*here*in the script |
| `cat file.txt`    | `cat <<EOF`...`EOF`                           |
| Needs external data | Self-contained                                    |

##### 💡 Where did the term originate?

The phrase " **here document** " likely originated in the 1970s with early versions of the  **Unix shell** , particularly the  **Bourne shell (sh)** .

It's a  **shell syntax feature** , and the naming reflects Unix's preference for  **descriptive, plain-English, minimalistic terms** .

Similar Unix naming styles:

* `here doc` = document placed here
* `stdin` = standard input
* `grep` = globally search a regular expression and print
* `fork` = create a process by “forking” the current one

### 🧠 Summary:

| Term              | Meaning                | Origin                                                                        |
| ----------------- | ---------------------- | ----------------------------------------------------------------------------- |
| `EOF`           | End Of File            | From low-level programming and system input streams — indicates no more data |
| `<<EOF`in shell | Start of here document | `EOF`marks the end of inline input fed to a command                         |

Let me know if you want examples of how `EOF` behaves in C, Python, or other languages too.

### ✨ Use Cases

| Use Case                | Example Command      |
| ----------------------- | -------------------- |
| Writing multi-line text | `cat`,`tee`      |
| Sending commands to ssh | `ssh user@host`    |
| Interactive scripts     | `ftp`,`sqlplus`  |
| Creating config files   | `cat > config.txt` |

### 🚫 Common Errors

* **Mismatched delimiter** : Make sure `EOF` is **not indented** unless you use `<<-EOF`.
* **Unintended variable expansion** : Use `'EOF'` to prevent this.
* **Spacing matters** : The final `EOF` **must be at the start of the line** (no spaces before it).

### ✅ Bonus: Using `<<-EOF` to allow tab-indented ending delimiter

```bash
cat <<-EOF
	This is tab-indented content
EOF
```

Only **tabs** before the text and delimiter are stripped.  **Spaces are not** .

---
