# Session 1 — Linux, the Shell, and Filesystems

**Workshop:** Software Engineering / Networking / Cybersecurity
**Audience:** Undergraduates, mixed majors, no prior Linux assumed
**Environment:** Each student has a personal Ubuntu cloud VM with remote desktop. GUI apps (VSCode) available; terminal also available.

**How to use this document:** Scroll top-to-bottom as you teach. Each `---` is a natural pause / screen change. Bold labels signal what to do:

- **Talk:** narrative point — say this in your own words
- **Demo:** type this live on the projector
- **Exercise:** students do this on their VM (walk the room)
- **Watch out:** common pitfalls / things to head off

---

# Block 0 — Welcome

## What we're doing this session

- This session is a **foundation** for the whole workshop.
- Goal by the end: you can sit down at a Linux machine and *use it*. Open a terminal, find your way around, edit a file, run a program, read help.
- We're not aiming for expertise. We're aiming for fluency at the level of someone who can follow along in the rest of the workshop.
- Let's make this interactive.

## Logistics

- You each have your own Ubuntu VM with a remote desktop.
- You can use the GUI for some things; we will spend most of our time in the **terminal**.
- This is hands-on. When I demo something, you should follow along in your VM.
- Stop me with questions. If you get lost, raise your hand.

---

# What we'll cover today

The whole session at a glance — keep this in mind as we go.

| # | Block |
|---|---|
| 0 | Welcome |
| 1 | Orientation |
| 2 | First contact |
| 3 | Navigating the filesystem |
| 4 | Working with files and directories |
| 5 | Getting help |
| 6 | Editing files |
| 7 | Glue: redirection and pipes |
| 8 | Permissions and processes |
| 9 | Users, root, and sudo |
| 10 | Environment variables, PATH, LD_LIBRARY_PATH |
| 11 | Python virtual environments |
| 12 | Wrap up |

---

## Block 1 outline — Orientation

- What Linux actually is, and why every server we'll touch is Linux
- What a **shell** is and what a **terminal** is
- Why we use the terminal even though the GUI exists
- The three mental models we'll come back to all day

---

## Block 2 outline — First contact

- Open a terminal; read the prompt
- Your first commands: `whoami`, `hostname`, `date`, `echo`
- Tab completion — your new best friend
- Command history (↑ / ↓, Ctrl-R)

---

## Block 3 outline — Navigating the filesystem

- The filesystem as one big tree under `/`
- `pwd`, `ls` (and its flags), `cd`
- Absolute vs. relative paths; `.`, `..`, `~`
- `which`: commands are just files

---

## Block 4 outline — Working with files and directories

- `mkdir`, `touch` — making things
- `cat`, `less`, `head`, `tail` — reading
- `cp`, `mv` — copy, move, rename
- `rm` — the "no Trash, no undo" talk
- Hands-on file workout

---

## Block 5 outline — Getting help

- The mindset: look it up, don't memorize
- `--help` — usually all you need
- `man` — the full manual
- `apropos` — finding commands by keyword

---

## Block 6 outline — Editing files

- Why you need both a GUI and a terminal editor
- VSCode (`code <file>`)
- nano (Ctrl-O save, Ctrl-X exit)
- How to escape vim if you land in it accidentally

---

## Block 7 outline — Glue: redirection and pipes

- Each command as a "little factory"
- `>` and `>>` — sending output to a file
- `|` — piping output into another command
- `grep` and `wc` as glue partners

---

## Block 8 outline — Permissions and processes

- Reading `ls -l` and the `rwx` permission string
- `chmod +x` to make a script runnable
- `ps`, `top` — what's running
- `kill`, Ctrl-C — stopping things

---

## Block 9 outline — Users, root, and sudo

- Linux is multi-user; everything belongs to someone
- Regular users vs. **root** (UID 0)
- Why you can't install packages as a regular user
- `sudo` — what it is, how it behaves, what it strips

---

## Block 10 outline — Environment variables, PATH, LD_LIBRARY_PATH

- What environment variables are
- `PATH` — how the shell finds commands (and why venvs work)
- A small `PATH` foot-gun to avoid
- `LD_LIBRARY_PATH` — shared libraries, with a security angle

---

## Block 11 outline — Python virtual environments

- Why venvs exist (and why Ubuntu makes them practically mandatory)
- One-time install: `python3-venv` and `python3-pip`
- Create, activate, install, deactivate
- The workflow you'll use forever

---

## Block 12 outline — Wrap up

- What you can now do
- What's coming next in the workshop
- The cheat sheet
- Q&A

---

# Block 1 — Orientation

## What is Linux?

**Talk:**

- Linux is the **operating system** that runs most of the servers, cloud VMs, supercomputers, and embedded devices on the planet.
- Android is Linux underneath. Most websites you visit run on Linux. Your VM is Linux.
- For our workshop: every server you'll touch later is a Linux machine. Learning Linux now buys you everything else.

---

## Where Linux comes from

![Unix family tree showing Linux, BSD, macOS/Darwin, Solaris, AIX, HP-UX and others](images/unix-history.png)

**Talk:**

- Linux is part of the **Unix family** — born in 1991, designed to behave like the Unix systems used in research and industry since the 1970s.
- Same family: **BSD**, **macOS** (its core, Darwin, is BSD-derived), Solaris, AIX, HP-UX, Android.
- Notice what's **not** on this chart: Windows, DOS, and OS/2. They come from a completely separate lineage (CP/M → MS-DOS → Windows). That's why the command line, file paths, and many conventions look so different on those systems.
- *Image: "Unix history (simplified)" — Wikimedia Commons, CC BY-SA 3.0 / GFDL.*

---

## Many distributions, one Linux

![Timeline of GNU/Linux distributions from 1991 to present, branching from Debian, Red Hat, Slackware, and other early ancestors](images/linux-distro-timeline.png)

**Talk:**

- "Linux" technically just refers to the **kernel** — the core of the OS.
- A **distribution** ("distro") bundles the Linux kernel with a package manager, default tools, and an installer to make a full usable system.
- Hundreds of distros exist. Most descend from a small number of ancestors: **Debian** (→ Ubuntu, Mint), **Red Hat** (→ Fedora, CentOS, Rocky), **Slackware**, **Arch**.
- **Your VM runs Ubuntu**, which is in the Debian family. Everything you learn today works on basically every Linux distro.
- *Image: "2023 Linux Distributions Timeline" — GioComitini via GNUclad, building on Lundqvist & Rodic's work. Wikimedia Commons, CC BY-SA 4.0.*

---

## What is a shell?

**Talk:**

- A **shell** is a program that reads what you type, runs it, and shows you the result.
- The shell we're using is called **bash**. There are others (zsh, fish) — same idea, slight differences.
- A **terminal** is the window the shell runs in. People say "terminal" and "shell" loosely; they're not the same thing, but you'll be fine treating them as the same for now.

---

## Why are we using the terminal at all? We have a GUI.

**Talk:**

- The GUI is a thin layer over the same files and programs. Everything you can do in the GUI, you can do in the terminal — and a lot more.
- Most servers don't *have* a GUI. When you SSH into a cloud machine, all you get is a shell.
- The terminal is how professionals actually drive these systems. It's faster, scriptable, and reproducible.
- This is the single biggest skill jump in this workshop. After today, you can follow along everywhere else.

---

## Three ideas to hold in your head all day

1. **The filesystem is a tree.** Everything lives in one big tree starting at `/`.
2. **The shell is a conversation.** You say a command, it responds. One turn at a time.
3. **Every command is: `verb [flags] [nouns]`.** A verb (the command), some flags (how), some nouns (what to act on).

Example: `ls -l /home`
- verb: `ls` (list)
- flag: `-l` (long format)
- noun: `/home` (the thing to list)

We'll come back to this pattern over and over.

---

# Block 2 — First contact

## Open a terminal

**Demo:**

- On your VM desktop, open the **Terminal** application.
- Or: right-click the desktop → "Open Terminal".

**Exercise:** Open a terminal on your VM. Make it bigger so you can read it.

---

## What you're looking at — the prompt

You'll see something like:

```
exouser@vm-42:~$
```

Breaking that down:

- `exouser` — your **username**
- `vm-42` — the **hostname** (the name of this machine)
- `~` — your **current directory** (`~` is shorthand for "your home directory")
- `$` — the prompt symbol. The shell is ready for a command.

**Talk:** The prompt is telling you *who* you are, *where* you are, and that it's listening. After every command, you come back to a prompt.

---

## Your first commands

**Demo + Exercise:** Type each of these and press Enter.

```bash
whoami
```

Prints your username. (Stupid? Useful when you SSH into a server and forget who you logged in as.)

```bash
hostname
```

The name of this machine.

```bash
date
```

The current date and time.

```bash
echo "hello world"
```

`echo` prints whatever you give it. Boring on its own. We'll see why it matters later.

---

## Tab completion — your new best friend

**Talk:**

- The shell will finish words for you. Type a few letters, press **Tab**.
- If it's unique, it completes. If not, press Tab twice to see options.
- **Use this constantly.** It prevents typos and makes you faster.

**Demo:** Type `who` then press Tab. Type `da` then press Tab Tab.

**Watch out:** Tab completion is case-sensitive. `Desktop` and `desktop` are different.

---

## Command history

**Talk:** The shell remembers what you typed. You don't have to retype.

- **Up arrow / Down arrow** — walk through previous commands.
- **Ctrl-R** — search your history. Start typing a few letters of an old command.
- `history` — print everything you've typed in this session.

**Exercise:** Press Up a few times. Find the `whoami` you ran a minute ago and re-run it.

---

# Block 3 — Navigating the filesystem

## The filesystem is a tree

**Talk + draw on the board if you have one:**

```
/                  ← the "root" — everything lives under this
├── home
│   └── exouser    ← your home directory (this is what ~ means)
├── etc            ← system configuration files
├── tmp            ← scratch space, wiped on reboot
├── usr            ← installed programs
├── var            ← logs and variable data
└── bin            ← essential commands
```

- One tree. One root. No drive letters like `C:\`.
- Every file and every directory has a **path** that describes where it sits in the tree.

---

## Where am I right now?

**Demo:**

```bash
pwd
```

`pwd` = **print working directory**. Tells you exactly where you are in the tree.

You should see something like `/home/exouser`.

**Talk:** You are *always* somewhere in the filesystem. Commands you run usually act on your current location unless you tell them otherwise.

---

## What's here?

**Demo:**

```bash
ls
```

`ls` = **list**. Shows the files and directories in your current location.

```bash
ls -l
```

The `-l` flag gives a **long** listing — sizes, dates, permissions.

```bash
ls -a
```

`-a` shows **all** files, including hidden ones (filenames starting with `.`).

```bash
ls -la
```

Combine flags. Long *and* all.

**Talk:** This is the verb/flags/nouns pattern. `ls` is the verb. `-l` and `-a` are flags. We didn't give a noun — so it uses the default, "here."

---

## Moving around: `cd`

**Demo:**

```bash
cd /
ls
pwd
```

You moved to the root of the filesystem. `ls` shows the top-level directories.

```bash
cd /etc
ls
```

You moved into `/etc`. Lots of config files here.

```bash
cd ~
pwd
```

Back home. `~` is shorthand for *your* home directory.

```bash
cd
```

`cd` with no arguments — also goes home. Handy when you're lost.

---

## Absolute vs relative paths

**Talk:**

- **Absolute path** starts with `/`. It's the full address from the root. Always works no matter where you are.
  - Example: `/home/exouser/Documents`
- **Relative path** does not start with `/`. It's *relative to where you currently are*.
  - Example: if you're in `/home/exouser`, then `Documents` and `./Documents` mean `/home/exouser/Documents`.

Two special names:

- `.` — "here" (the current directory)
- `..` — "one level up" (the parent directory)

---

## Try moving around with both kinds of paths

**Exercise:**

```bash
cd ~
mkdir practice
cd practice
pwd
cd ..
pwd
cd practice
cd ../..
pwd
```

Talk through what happened at each step. Did each `pwd` match what you expected?

---

## Where do programs live?

**Demo:**

```bash
which ls
which python3
which bash
```

`which` tells you where a command's program file actually lives on disk. Useful when you wonder "where is this thing coming from?"

**Talk:** Commands are just files. `ls` is a real file at `/usr/bin/ls`. When you type `ls`, the shell looks it up and runs it.

---

# Block 4 — Working with files and directories

## Making things

**Demo:**

```bash
cd ~
mkdir workshop
cd workshop
mkdir notes scripts data
ls
```

`mkdir` = **make directory**. You can make several at once.

```bash
touch hello.txt
touch notes/day1.md
ls
ls notes
```

`touch` creates an empty file (or updates its timestamp if it already exists).

---

## Reading file contents

**Demo:** Let's look at a real file on the system.

```bash
cat /etc/os-release
```

`cat` = **concatenate** (historical name). In practice: "dump this file to the screen."

```bash
less /etc/services
```

`less` is a **pager**. It shows the file one screen at a time.

- **Space** — next page
- **b** — previous page
- **/word** — search for "word"
- **q** — quit

**Watch out:** `less` looks like it took over your terminal. It did. **Press `q` to get out.**

```bash
head /etc/services
tail /etc/services
head -n 3 /etc/services
```

`head` shows the first 10 lines. `tail` shows the last 10. `-n 3` changes the count.

---

## Copy, move, rename

**Demo:**

```bash
cd ~/workshop
cp hello.txt hello-copy.txt
ls
```

`cp source destination` = **copy**.

```bash
mv hello-copy.txt notes/
ls
ls notes
```

`mv` = **move**. You moved the file into the `notes` directory.

```bash
mv notes/hello-copy.txt notes/greeting.txt
ls notes
```

`mv` is also how you **rename**. There's no separate rename command.

---

## Deleting — read this before you type

**Talk, slowly:**

- `rm` deletes files. **Permanently.** There is no Trash. There is no undo.
- `rm -r` deletes a directory and everything in it. Recursively.
- `rm -rf` deletes recursively and forcefully (no prompts).
- The most infamous typo in computing: `rm -rf /`. It tries to delete the entire system.
- A close second: `rm -rf ~` — wipes your home directory.
- **Always look at what you typed before pressing Enter.** Especially with `rm`.

**Demo (carefully):**

```bash
cd ~/workshop
rm hello.txt
ls
```

Gone.

```bash
mkdir junk
touch junk/a junk/b junk/c
ls junk
rm -r junk
ls
```

Gone with everything inside.

**Watch out:** If you ever feel uncertain, run `ls` first to see what `rm` *would* affect. Or use `rm -i`, which asks before each delete.

---

## Wildcards: matching many files at once

**Talk:**

- The shell knows two **wildcards** for matching filenames:
  - `*` — matches **any sequence** of characters (including nothing)
  - `?` — matches **exactly one** character
- The shell **expands** these *before* the command sees them. The command just gets a list of matching filenames.

**Demo:**

```bash
cd ~/workshop
touch a.txt b.txt c.log readme.md
ls *.txt        # a.txt  b.txt
ls *.log        # c.log
ls ?.txt        # a.txt  b.txt   (one character + .txt)
ls *            # everything
echo *.txt      # see what the shell expands * into
```

**Talk:**

- Works with **most commands** that take filenames: `ls`, `cp`, `mv`, `rm`, `cat`, `grep`, etc.
- Example: `rm *.tmp` deletes every `.tmp` file in this directory. Powerful — and pairs directly with the "look before you press Enter" rule from a minute ago.
- If nothing matches the pattern, you'll usually get an error or the literal `*` passed through. **Run `ls <pattern>` first** if you're unsure what your wildcard will hit.

---

## Hands-on: a small file workout

**Exercise:** Without looking at notes, on your VM:

1. Go to your home directory.
2. Make a directory called `lab`.
3. Inside `lab`, make three directories: `morning`, `afternoon`, `evening`.
4. In `morning`, create a file called `breakfast.txt`.
5. Copy `breakfast.txt` into `afternoon`, renaming it to `lunch.txt`.
6. Move `lunch.txt` from `afternoon` into `evening`, renaming it to `dinner.txt`.
7. List the contents of `lab` recursively: `ls -R lab`.
8. Delete the whole `lab` directory.

Raise your hand when you've finished, or if you're stuck. Take your time.

---

# Block 5 — Getting help

## You will not remember all of this. Nobody does.

**Talk:**

- Working in Linux is not about memorizing commands. It's about knowing **how to look things up fast.**
- Four tools, in order of usefulness for beginners:

---

## `--help` — the quickest answer

**Demo:**

```bash
ls --help
mkdir --help
```

Most commands accept `--help` and print a short usage summary. Often this is all you need.

---

## `man` — the full manual

**Demo:**

```bash
man ls
```

`man` opens the full manual page. It uses the same pager as `less`, so:

- **Space** — next page
- **/word** — search
- **q** — quit

**Watch out:** `man` pages are dense and written for people who already mostly understand the command. Don't be discouraged — start with `--help`, and reach for `man` when you need the full detail.

---

## `apropos` — "what command does X?"

**Demo:**

```bash
apropos "list directory"
apropos compress
```

Searches the man page descriptions for keywords. Useful when you don't know the command's name.

---

## Pattern check

**Talk:** Look at this command:

```bash
ls -la /etc
```

- Verb: ____
- Flags: ____
- Nouns: ____

(Wait for the room to call out: `ls`, `-l` and `-a`, `/etc`.)

Every command you'll meet today follows this shape.

---

# Block 6 — Editing files

## Two ways to edit, and why you need both

**Talk:**

- You have a GUI editor on your VM (**VSCode**). It's lovely. Use it when you can.
- You also need to know **one terminal editor**. When you SSH into a server later in this workshop, there is no VSCode. There is only a terminal.
- We'll use **nano** because it's the friendliest. Vim and Emacs exist; we'll skip them. (If you've heard the "how do I exit Vim?" jokes — that's why we're using nano.)

---

## First: edit with VSCode (GUI)

**Demo:**

```bash
cd ~/workshop
code notes/day1.md
```

`code` opens VSCode. (If `code` isn't available, use the VSCode icon and File → Open.)

Type some text:

```
# Day 1 notes
- I opened a terminal.
- I am editing a file.
```

Save (Ctrl-S). Close the file.

Back in the terminal:

```bash
cat notes/day1.md
```

You should see what you typed. **Same file. Two surfaces.**

---

## Now: edit the same file with nano (terminal)

**Demo:**

```bash
nano notes/day1.md
```

You're now inside nano. Things to notice:

- Your text is in the middle.
- The bottom shows shortcuts. `^` means **Ctrl**. So `^X` = **Ctrl-X**.

Add a line:

```
- I edited this in nano too.
```

To save and exit:

1. **Ctrl-O** — write Out (save). It asks for a filename — press Enter to accept.
2. **Ctrl-X** — eXit.

Back in the shell:

```bash
cat notes/day1.md
```

Both edits are there.

---

## Exercise: edit a file two ways

**Exercise:**

1. In `~/workshop`, create a file `aboutme.txt` (use `touch` or just open it in nano).
2. In **VSCode**, add three lines: your name, your major, one thing you're hoping to learn.
3. In **nano**, add a fourth line: your favorite food.
4. `cat aboutme.txt` to confirm all four lines are there.

---

## Let's write a real shell script

**Talk:**

- A **shell script** is just a text file with shell commands in it.
- The `.sh` extension is a convention — Linux doesn't *require* it, but humans use it to know what the file is.

**Demo:**

```bash
cd ~/workshop
nano hello.sh
```

Inside nano, type these two lines:

```bash
#!/bin/bash
echo "Hello, World!"
```

Save (Ctrl-O, Enter) and exit (Ctrl-X), then confirm:

```bash
cat hello.sh
```

---

## What's `#!/bin/bash`? — the shebang

**Talk:**

- That first line is called the **shebang** (pronounced "shuh-bang" or "hash-bang").
- The `#!` must be the **very first two characters** of line 1. No spaces. No blank line above it.
- It tells the kernel: "when someone runs this file as a program, hand it to `/bin/bash` to interpret."
- Without a shebang, the system has to guess which interpreter to use — and may pick the wrong one or refuse to run the script.
- Other shebangs you'll see in the wild:
  - `#!/bin/sh` — minimal POSIX shell
  - `#!/usr/bin/env python3` — Python script (the `env` trick finds python3 wherever it lives in PATH)
  - `#!/usr/bin/perl` — Perl script

**Watch out:**

- `#!` is **not** a comment, even though `#` is the bash comment character. The kernel reads the first two bytes of the file specially.

---

## Make it executable, then run it

**Demo:**

```bash
$ ./hello.sh
bash: ./hello.sh: Permission denied
```

The file exists, but it isn't marked as executable yet. Check:

```bash
$ ls -l hello.sh
-rw-r--r-- 1 exouser exouser 32 ... hello.sh
```

No `x` in the permission string. Add it:

```bash
$ chmod +x hello.sh
$ ls -l hello.sh
-rwxr-xr-x 1 exouser exouser 32 ... hello.sh
```

Now run it:

```bash
$ ./hello.sh
Hello, World!
```

**Talk:**

- `chmod +x` adds the **execute** bit. (Block 8 will go deeper into what every position in that string means.)
- Why `./hello.sh` and not just `hello.sh`? The current directory `.` is **not** in your `PATH`, for security reasons. `./` says "the file right here." (We'll cover `PATH` in Block 10.)

---

## Exercise: your own script

**Exercise:**

1. In `~/workshop`, use `nano` to create `greet.sh` containing:
   ```bash
   #!/bin/bash
   echo "Hello from $USER on $(hostname) — today is $(date)"
   ```
2. Try to run it: `./greet.sh`. It should fail with "Permission denied."
3. Make it executable: `chmod +x greet.sh`.
4. Run it again. You should see your username, hostname, and the current date.
5. (Bonus) Remove the shebang line and re-run. What changes?

---

## Quick mention: `vim`

**Talk:**

- You will see `vim` mentioned everywhere. It's powerful and beloved by some, infuriating to most beginners.
- If you accidentally end up in vim, press **Esc**, then type **`:q!`** and press Enter to escape without saving.
- That's all you need to know about vim today.

---

# Block 7 — Glue: redirection and pipes

## Why this block matters

**Talk:**

- So far, each command has been standalone.
- The real power of the shell is **combining** commands — sending the output of one into another.
- Two ideas: **redirection** (output to a file) and **pipes** (output to another command).
- Once you get these, you can solve problems by **composing** small tools.

---

## Redirection: send output to a file

**Demo:**

```bash
echo "first line" > log.txt
cat log.txt
```

`>` means "send the output of the command into this file." If the file exists, it's **overwritten**.

```bash
echo "second line" >> log.txt
echo "third line" >> log.txt
cat log.txt
```

`>>` means "**append** to the file" instead of overwriting.

**Watch out:** `>` silently overwrites. If you meant `>>` and typed `>`, your file is replaced. Look before you press Enter.

---

## Pipes: send output to another command

**Demo:**

```bash
ls /etc
```

Lots of output, scrolls off the top.

```bash
ls /etc | less
```

The `|` (pipe) takes the output of `ls /etc` and feeds it to `less`. Now you can scroll.

```bash
ls /etc | wc -l
```

`wc -l` counts lines. So this answers "how many things are in `/etc`?"

---

## A real example: searching

**Demo:**

```bash
grep root /etc/passwd
```

`grep` searches for a pattern in text. This finds the line(s) about the `root` user.

Now combine:

```bash
cat /etc/passwd | grep root
cat /etc/passwd | wc -l
```

How many users are defined on this system?

```bash
ls /usr/bin | grep python
```

Find every program in `/usr/bin` whose name contains "python."

---

## The mental model

**Talk:**

- Think of each command as a **little factory** with an input and an output.
- `|` is a **conveyor belt** between factories.
- `>` and `>>` are **drains** into a file.
- You can chain as many as you like. This is how short shell commands solve big problems.

---

## Exercise: compose a one-liner

**Exercise:** On your VM:

1. Count how many lines are in `/etc/services`.
   - Hint: `wc -l /etc/services`
2. Find lines in `/etc/services` that mention `http`.
   - Hint: `grep http /etc/services`
3. Count how many lines in `/etc/services` mention `http`.
   - Hint: combine grep and wc with a pipe.
4. Save the lines that mention `http` into a file called `http-lines.txt` in your home directory.
   - Hint: combine grep and `>`.

---

# Block 8 — Permissions and processes

## Why this block is short

**Talk:**

- We're going to **introduce vocabulary**, not master it. The cybersecurity session will go deeper.
- The goal: you can read `ls -l` and roughly know what's going on, and you know how to see what's running.

---

## Reading `ls -l`

**Demo:**

```bash
ls -l /etc/hostname
```

You'll see something like:

```
-rw-r--r-- 1 root root 8 May 10 09:00 /etc/hostname
```

Breaking that down:

| `-rw-r--r--` | `1` | `root` | `root` | `8` | `May 10 09:00` | `/etc/hostname` |
|---|---|---|---|---|---|---|
| permissions | links | owner | group | size (bytes) | modified | name |

---

## Decoding the permission string

`-rw-r--r--` is ten characters. Read in groups:

```
-     rw-     r--     r--
type  owner   group   others
```

- First character: `-` for a regular file, `d` for a directory.
- Next three: what the **owner** can do.
- Next three: what the **group** can do.
- Last three: what **everyone else** can do.

Each group is three slots: **r**ead, **w**rite, e**x**ecute.

**Talk:** So `-rw-r--r--` means: regular file, owner can read and write, everyone else can only read.

---

## Trying it

**Demo:**

```bash
cd ~/workshop
touch script.sh
ls -l script.sh
```

You'll see `-rw-r--r--`. The file is not executable.

```bash
chmod +x script.sh
ls -l script.sh
```

Now it's `-rwxr-xr-x`. We added the **executable** bit. This is how you make a script runnable.

**Talk:** We'll come back to `chmod` and permissions in the security session. Today, just know: that string on the left of `ls -l` describes who can do what.

---

## What's running? — processes

**Talk:** A **process** is a running program. Every command you run starts one. Every browser tab, every background service — all processes.

**Demo:**

```bash
ps
```

Shows processes in your current terminal. Not very many.

```bash
ps aux | head -n 20
```

Shows *all* processes on the system. Long output — we pipe through `head` to see the first 20.

Each row has a **PID** (process ID), an owner, CPU/memory usage, and the command.

---

## Watching live: `top`

**Demo:**

```bash
top
```

A live, updating view of processes — sorted by CPU usage. Like the Activity Monitor / Task Manager.

- **q** — quit `top`

If installed:

```bash
htop
```

A nicer version with colors. `q` to quit.

---

## Stopping a process

**Talk:**

- If a process is misbehaving, you can stop it by its **PID**.
- `kill 1234` — politely ask process 1234 to stop.
- `kill -9 1234` — force it to stop. Use sparingly.
- In your terminal: **Ctrl-C** stops the currently running command. This is one of the most useful keys to know.

**Demo:**

```bash
sleep 60
```

The shell hangs (sleeping for 60 seconds). Press **Ctrl-C**. You're back.

---

# Block 9 — Users, root, and sudo

## Linux is a multi-user system

**Talk:**

- Every Linux machine has the concept of **users**. Each user has a username, a numeric ID, and a home directory.
- You saw this already: `whoami` printed your username, and `ls -l` showed the owner of each file.
- Files and processes always belong to *some* user. Permissions decide who else can touch them.

---

## Two kinds of users

**Talk:**

- **Regular users** (like you) — can read, write, and run their own files plus anything permissions let them touch. Cannot change the system itself.
- **The `root` user** — the administrator. Unique. Numeric ID 0. Bypasses almost all permission checks.
  - Can read any file.
  - Can modify any file.
  - Can kill any process.
  - Can install or remove software for the whole machine.
  - Can also wreck the system in one command.

**Talk:** There is exactly **one** root account per Linux machine. That's a feature, not a limitation.

---

## Why the split exists

**Talk:**

- Most of the time you should **not** be able to break the system, even by accident.
- Separating "the user doing their work" from "the user with system-wide power" means a typo, a buggy program, or a piece of malware running as you can damage *your* stuff but not the OS.
- Same logic protects other users on shared systems — your mistakes don't affect them.
- This is one of the reasons Linux servers stay up for years.

---

## Why you can't install packages as a normal user

**Talk:**

- Installing a package puts files into directories like `/usr/bin/`, `/usr/lib/`, `/etc/`.
- Those directories are owned by `root`. Look:

**Demo:**

```bash
ls -ld /usr/bin /usr/lib /etc
```

You'll see `root root` as the owner/group. Permissions don't let regular users write there.

**Demo:** Watch a normal install attempt fail:

```bash
apt install cowsay
```

You'll get a permission error (or be told to use sudo). The package manager doesn't have permission to write into `/usr/bin/`, because **you** don't.

**Talk:** This isn't `apt` being unfriendly — it's the filesystem doing its job. To install system-wide, you need to act as root.

---

## What `sudo` is

**Talk:**

- `sudo` = "**s**ubstitute **u**ser, **do**" — run a single command as another user, defaulting to root.
- You authenticate with **your own password**, not root's password. (You usually don't even know root's password, and you don't need to.)
- Whether you're *allowed* to use sudo is configured by a sysadmin in `/etc/sudoers`.
- On your workshop VM, your account has been given sudo privileges. On a random server you log into, you may or may not.

---

## How sudo behaves

**Demo:**

```bash
whoami
sudo whoami
```

The first prints your username. The second prints `root` — because `whoami` ran as root.

**Talk:**

- Notice your shell is still **you** — only the one command was elevated.
- The first time you sudo in a terminal it asks for your password. **Nothing shows up as you type** — that's intentional, not broken.
- Sudo caches your authentication for a short time (typically ~15 minutes), so a follow-up `sudo` won't ask again.

**Demo:** Now the install works:

```bash
sudo apt install cowsay
```

`apt` is running as root, so it can write to `/usr/bin/`.

---

## Getting a root shell: `sudo -i`

**Talk:**

- For a *series* of admin commands, you can drop into an interactive root shell:

**Demo:**

```bash
sudo -i
whoami
pwd
exit
```

- Your prompt typically changes — often `$` becomes `#` to remind you you're root.
- `exit` returns you to your normal shell.
- Use this sparingly. The longer you stay root, the more damage a typo can do.

(There's also `su` for switching to other users — we don't need it today.)

---

## Watch out

- **Don't habitually `sudo` everything.** Run as yourself; sudo only when you actually need to. This is what keeps you safe from typos and bad scripts.
- `sudo rm -rf /` really does try to wipe the system. The most famous disaster command in computing. **Look before you press Enter when sudo is on the line.**
- For security, `sudo` strips most environment variables — including `LD_LIBRARY_PATH` and `PATH` is reset to a safe default. This will matter in the next block.
- `sudo !!` re-runs your last command with sudo — useful when you forgot it the first time.

---

## Try it

**Exercise:**

1. Run `whoami`. Note your username.
2. Run `sudo whoami`. Note the output.
3. Run `id` and `sudo id`. Compare. The numeric `uid=` and `gid=` change.
4. Try `apt update` (no sudo) — fails.
5. Run `sudo apt update` — works.
6. (Optional) `sudo -i`, then `whoami`, then `exit`.

---

# Block 10 — Environment variables, PATH, and LD_LIBRARY_PATH

## What are environment variables?

**Talk:**

- Your shell carries a bag of named settings called **environment variables**.
- They're how programs discover things about *their* environment: who you are, where your home is, what language you prefer, where to look for things.
- Every program you launch inherits a copy of this bag.

**Demo:**

```bash
env | head -n 20
```

A long list of `NAME=value` pairs. Read one by name with `echo` and a `$`:

```bash
echo $HOME
echo $USER
echo $SHELL
```

**Talk:** The `$` says "give me the *value* of the variable, not the literal word." `$HOME` becomes `/home/exouser`; the bare word `HOME` is just four letters.

---

## PATH — how the shell finds commands

**Talk:**

- When you type `ls`, how does the shell know that `ls` is a program in `/usr/bin`?
- It looks in the directories listed in the `PATH` variable, **in order**, and runs the first match.

**Demo:**

```bash
echo $PATH
```

You'll see something like:

```
/home/exouser/.local/bin:/usr/local/bin:/usr/bin:/bin:/usr/games
```

- Colon-separated list of directories.
- The shell tries each one left to right. **Earlier wins.**
- `which` shows you which match it found:

```bash
which ls
which python3
```

---

## Why PATH matters in this workshop

**Talk:**

- This is exactly **why activating a venv works**. The `activate` script prepends the venv's `bin/` to your `PATH`. So when you type `python`, the venv's Python wins because it's earlier in the list.
- If you write a script of your own and put it in some random directory, the shell won't find it by name. You have to call it by its full path (`./myscript` or `/home/exouser/scripts/myscript`).
- Adding a directory to `PATH` is how you make your own programs "globally" callable.

**Demo:**

```bash
export PATH=$HOME/scripts:$PATH
echo $PATH
```

That says: "Set `PATH` to my new directory, followed by everything it was before." The change lasts only for **this** shell session.

---

## A small PATH foot-gun

**Watch out:**

- If you typo `PATH` — for example, `export PATH=$HOME/scripts` (forgetting `:$PATH` at the end) — you can suddenly lose access to `ls`, `cat`, and almost everything else. The programs still exist on disk; the shell just can't find them anymore.
- Fix: open a new terminal (the bad change doesn't persist), or restore manually with `export PATH=/usr/bin:/bin:$PATH`.

---

## LD_LIBRARY_PATH — how programs find shared libraries

**Talk:**

- Most programs don't carry all their code with them. They depend on **shared libraries** — files ending in `.so` — that get loaded into the program when it runs.
- Linux has standard places it looks for these (`/usr/lib`, `/lib`, and others configured system-wide).
- `LD_LIBRARY_PATH` is the variable that lets you add **extra** places to look.

**Demo:** See what libraries a program needs:

```bash
ldd $(which ls)
```

You'll see lines like `libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6`. Each one is a shared library `ls` will load at run time.

---

## When you'll touch LD_LIBRARY_PATH

**Talk:**

- You'll set it when you run a program whose libraries live in a non-standard place — typically custom-built software, scientific tools, or proprietary software that ships bundled libraries:

```bash
LD_LIBRARY_PATH=/opt/mytool/lib ./mytool
```

That sets the variable **only for this one command**. The variable goes back to whatever it was afterwards.

- For the rest of this workshop: when something fails with an error like `error while loading shared libraries: libfoo.so.3: cannot open shared object file`, `LD_LIBRARY_PATH` is what you reach for.

---

## A security flavor for later

**Talk:**

- Because `LD_LIBRARY_PATH` (and a sibling, `LD_PRELOAD`) change which code gets loaded into a program, they're a classic vector for both legitimate library overrides **and** library-injection attacks.
- For your safety, Linux **strips** `LD_LIBRARY_PATH` when you run a program with `sudo` or any setuid binary. That's intentional. The cybersecurity session will revisit why.

**Watch out:**

- Don't set `LD_LIBRARY_PATH` globally (e.g., in `.bashrc`) unless you really need to — it affects every program you run and can quietly break things. Set it inline, just for the command that needs it.

---

## Making changes stick: `.bashrc`

**Talk:**

- `export` in your terminal is **temporary** — it dies when the terminal closes.
- To make a setting permanent, add the line to `~/.bashrc`. That file runs every time you open a new bash shell.

**Demo:**

```bash
nano ~/.bashrc
```

Scroll to the bottom and add:

```bash
export PATH=$HOME/scripts:$PATH
```

Save (Ctrl-O), exit (Ctrl-X). To apply it to the **current** terminal without reopening:

```bash
source ~/.bashrc
```

(`source` here is the same `source` you use to activate a venv — "run this script in my current shell so the changes affect me.")

---

## Exercise

**Exercise:** On your VM:

1. Print your current `PATH`: `echo $PATH`.
2. Find where `python3` lives: `which python3`.
3. Look at the shared libraries `python3` depends on: `ldd $(which python3) | head`.
4. Make a tiny script:
   ```bash
   mkdir -p ~/scripts
   echo 'echo "hello from my script"' > ~/scripts/greet
   chmod +x ~/scripts/greet
   ```
5. Try running it by name: `greet`. It fails — not in `PATH`.
6. Run it by full path: `~/scripts/greet`. Works.
7. Add `~/scripts` to your `PATH`: `export PATH=$HOME/scripts:$PATH`.
8. Now `greet` works by name.

---

# Block 11 — Python virtual environments

## Why this block matters

**Talk:**

- Most of the Python you'll write in this workshop — and most Python you'll write afterwards — needs **libraries** (other people's code) installed with `pip`.
- If you install libraries system-wide, two projects with different version needs collide.
- Worse: on Ubuntu, the system Python is used by the OS itself. Installing random things into it can break system tools.
- A **virtual environment** ("venv") is an isolated Python setup that lives in a folder, per project. Install whatever you want inside it; nothing else on the system is affected.
- This is the standard way Python is used on Linux. You'll use venvs constantly in later sessions.

---

## One-time install: get the venv module

Ubuntu ships Python but not the `venv` module by default. Install it once per VM:

**Demo:**

```bash
sudo apt update
sudo apt install python3-venv python3-pip
```

**Talk:**

- `sudo` = "do this as the administrator." Needed for installing system packages.
- You'll be prompted for your password. **Nothing shows up as you type** — that's normal, not a bug. Just type it and press Enter.
- `apt` is Ubuntu's package manager. `apt update` refreshes the list of available packages; `apt install` installs them.

---

## Create a venv

**Demo:**

```bash
cd ~/workshop
python3 -m venv myenv
ls myenv
```

**Talk:**

- `python3 -m venv myenv` says: "run the Python `venv` tool, and make a venv in a new directory called `myenv`."
- Look inside — it's a normal directory holding an isolated Python plus a place for installed libraries.
- The venv lives in this folder. To delete the venv, delete the folder. No system cleanup needed.

---

## Activate the venv

**Demo:**

```bash
source myenv/bin/activate
```

Look at your prompt — it now starts with `(myenv)`. That's the venv telling you it's active.

**Talk:**

- `source` runs the activation script in your current shell. It changes your `PATH` so that `python` and `pip` point inside `myenv` instead of at system Python.
- Anything you install from now on goes into `myenv`, not into the system.

---

## Install a package

**Demo:**

```bash
pip install requests
```

`requests` is a popular library for making HTTP requests. Verify it landed:

```bash
python3 -c "import requests; print(requests.__version__)"
```

You should see a version number. The library is installed — but only inside this venv.

---

## See what's in the venv

**Demo:**

```bash
pip list
```

You'll see `requests` and a few of its dependencies.

---

## Leaving the venv

**Demo:**

```bash
deactivate
```

The `(myenv)` prefix disappears. You're back to your normal shell.

```bash
python3 -c "import requests"
```

If `requests` wasn't installed system-wide (it shouldn't be), this will fail. That's the whole point — the venv kept it contained.

---

## The workflow you'll use forever

**Talk:** For every Python project you start on Linux:

1. Make a folder for the project.
2. Make a venv inside (`python3 -m venv env`).
3. Activate it (`source env/bin/activate`).
4. `pip install` whatever libraries the project needs.
5. Work on the project.
6. `deactivate` when you're done.

When you come back to the project later, you just re-activate — you don't need to recreate the venv.

---

## Watch out

- `pip install` **outside** a venv on Ubuntu often fails now with a message about `externally-managed-environment`. That's Ubuntu protecting you from breaking the system. The fix is always: make and activate a venv first.
- If you clone someone else's Python project, they'll usually have a file called `requirements.txt`. After creating and activating your own venv, run `pip install -r requirements.txt` to install everything they listed.
- You don't put the venv folder in git. It's a local thing per machine.

---

## Exercise

**Exercise:** On your VM:

1. Make a new folder: `mkdir ~/python-lab && cd ~/python-lab`.
2. Create a venv called `env`: `python3 -m venv env`.
3. Activate it. Confirm your prompt shows `(env)`.
4. Install a fun package: `pip install cowsay`.
5. Run it:
   ```bash
   cowsay -t "hello from my venv"
   ```
   You should see an ASCII cow saying your message.
6. `deactivate`.
7. Try `cowsay -t hi` again. It should say "command not found" — because `cowsay` only lived in the venv.

---

# Block 12 — Wrap up

## What you can now do

- Open a terminal and read the prompt.
- Navigate the filesystem with `pwd`, `ls`, `cd`, and paths.
- Create, read, copy, move, and delete files and directories.
- Edit files in both VSCode and nano.
- Look up help with `--help` and `man`.
- Send output to files (`>`, `>>`) and chain commands with pipes (`|`).
- Read `ls -l` and roughly understand permissions.
- See what's running and stop it.
- Tell the difference between a regular user and root, and use `sudo` to run individual commands as root.
- Understand `PATH`, `LD_LIBRARY_PATH`, and how environment variables shape what your shell finds.
- Create and use Python virtual environments.

That's the entire foundation for every later session. Well done.

---

## What's next in the workshop

- Software engineering sessions will assume you can navigate a terminal and edit files.
- Networking sessions will get you SSHing between machines (you already know how to use a remote shell — it's the same shell, just on another machine).
- Cybersecurity sessions will revisit permissions, processes, and users in real depth.

---

## Cheat sheet (one page)

### Navigation
| Command | What it does |
|---|---|
| `pwd` | Print working directory |
| `ls` / `ls -l` / `ls -la` | List files (long, all) |
| `cd <path>` | Change directory |
| `cd ~` / `cd` | Go home |
| `cd ..` | Up one level |
| `cd -` | Go to previous directory |

### Files & directories
| Command | What it does |
|---|---|
| `mkdir <name>` | Make a directory |
| `touch <name>` | Make an empty file |
| `cat <file>` | Dump a file to screen |
| `less <file>` | Page through a file (`q` to quit) |
| `head` / `tail <file>` | First / last 10 lines |
| `cp <src> <dst>` | Copy |
| `mv <src> <dst>` | Move / rename |
| `rm <file>` | Delete (no undo!) |
| `rm -r <dir>` | Delete a directory and contents |

### Help
| Command | What it does |
|---|---|
| `<cmd> --help` | Quick usage summary |
| `man <cmd>` | Full manual (`q` to quit) |
| `which <cmd>` | Where the program lives |
| `apropos <word>` | Find commands by keyword |

### Editing
| Command | What it does |
|---|---|
| `code <file>` | Open in VSCode |
| `nano <file>` | Open in nano (`Ctrl-O` save, `Ctrl-X` exit) |

### Glue
| Symbol | What it does |
|---|---|
| `>` | Send output to file (overwrite) |
| `>>` | Append output to file |
| `\|` | Pipe output to next command |
| `grep <pat> <file>` | Search for a pattern |
| `wc -l` | Count lines |

### Permissions & processes
| Command | What it does |
|---|---|
| `ls -l` | See permissions, owner, size |
| `chmod +x <file>` | Make a file executable |
| `ps aux` | All processes |
| `top` / `htop` | Live process view (`q` to quit) |
| `kill <PID>` | Stop a process |
| `Ctrl-C` | Stop the current command |

### Survival
- `Tab` — auto-complete
- `Up arrow` — previous command
- `Ctrl-R` — search history
- `Ctrl-C` — stop the current command
- **Look before you press Enter, especially with `rm`.**

### Users & root
| Command | What it does |
|---|---|
| `whoami` | Your username |
| `id` | Your numeric IDs and group memberships |
| `sudo <cmd>` | Run one command as root (authenticate with your own password) |
| `sudo -i` | Open an interactive root shell (`exit` to leave) |
| `sudo !!` | Re-run your previous command with sudo |
| `sudo apt update` / `sudo apt install <pkg>` | Refresh / install packages system-wide |

### Environment & paths
| Command | What it does |
|---|---|
| `env` | List all environment variables |
| `echo $VAR` | Print the value of one variable |
| `echo $PATH` | Show command search path |
| `which <cmd>` | Show which PATH entry the shell picks for a command |
| `export VAR=value` | Set a variable for this shell (and its children) |
| `export PATH=$HOME/scripts:$PATH` | Prepend a directory to PATH |
| `ldd <program>` | List shared libraries a program needs |
| `LD_LIBRARY_PATH=/path ./prog` | Add a non-standard library directory, just for this run |
| `source ~/.bashrc` | Re-read your shell config in the current terminal |

### Python virtual environments
| Command | What it does |
|---|---|
| `sudo apt install python3-venv python3-pip` | One-time install (per VM) |
| `python3 -m venv <name>` | Create a venv in directory `<name>` |
| `source <name>/bin/activate` | Activate it (prompt shows `(<name>)`) |
| `pip install <pkg>` | Install a package into the active venv |
| `pip list` | Show what's installed in the venv |
| `pip install -r requirements.txt` | Install from a project's requirements file |
| `deactivate` | Leave the venv |

---

## Resources to keep going

- **Software Carpentry — The Unix Shell:** https://swcarpentry.github.io/shell-novice/
- **MIT — The Missing Semester of Your CS Education:** https://missing.csail.mit.edu/
- **Ubuntu command-line tutorial:** https://ubuntu.com/tutorials/command-line-for-beginners
- **explainshell.com** — paste any command, see it annotated

---

## Questions?

(Open the floor. If quiet: "What's one thing that confused you? Even if you think you got it — what was the muddy part?")
