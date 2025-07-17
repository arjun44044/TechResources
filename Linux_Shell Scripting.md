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

>
>
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
>

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
