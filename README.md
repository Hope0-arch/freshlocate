# freshlocate

**freshlocate** is a tiny Linux command I created using Bash scripting.

It started from a simple problem:

`locate` is extremely fast because it searches a prebuilt database of file paths, but that database can become outdated. Newly created files may therefore not appear immediately.

`freshlocate` solves the basic version of that problem by refreshing the database before running `locate`.

---

## What Does freshlocate Do?

Instead of manually doing:

```bash
sudo updatedb
locate <file_name>
```

you can run:

```bash
freshlocate <file_name>
```

The current version performs:

```text
freshlocate <file_name>
        ↓
check whether an argument was provided
        ↓
sudo updatedb
        ↓
if the update succeeds
        ↓
locate <file_name>
```

### Current Version

`v0.1` contains the basic logical structure of the command.

Future versions may improve things such as:

- avoiding unnecessary database refreshes
- better error handling
- optional manual refresh
- checking when the database was last updated
- additional command-line options

For now, the goal is to keep it small and understand every part of it.

---

# Installation

Clone or download the project and place the `freshlocate` script somewhere inside your `$PATH`.

A common location for personal commands is:

```bash
~/.local/bin
```

For example:

```bash
mkdir -p ~/.local/bin
cp freshlocate ~/.local/bin/freshlocate
chmod +x ~/.local/bin/freshlocate
```

Make sure `~/.local/bin` is included in your `$PATH`.

You can check with:

```bash
echo "$PATH"
```

Then verify that Bash can find the command:

```bash
type freshlocate
```

---

# Usage

```bash
freshlocate <file_name>
```

Example:

```bash
freshlocate hello.c
```

Running it without an argument prints:

```text
Usage: freshlocate <file_name>
```

---

# How the Bash Script Works

Before understanding `freshlocate`, it helps to understand what Bash actually does.

Bash is a **shell**.

A shell accepts commands such as:

```bash
ls
cd
mkdir
gcc
```

and arranges for them to be executed.

Bash also has its own scripting language, which lets us combine commands with things such as:

- variables
- conditions
- loops
- arguments
- functions
- exit codes

A Bash script is therefore a text file containing commands and Bash logic.

---

## 1. The Shebang

At the top of the script:

```bash
#!/usr/bin/env bash
```

This is called a **shebang**.

When the script is executed, the operating system sees this line and runs:

```text
/usr/bin/env
```

`env` then finds `bash` using `$PATH`, and Bash interprets the rest of the script.

Conceptually:

```text
execute freshlocate
        ↓
OS reads shebang
        ↓
/usr/bin/env
        ↓
find bash
        ↓
Bash reads the script
```

---

## 2. Comments

Comments begin with:

```bash
#
```

Example:

```bash
# Check whether the user supplied an argument
```

Comments are ignored when the script runs.

They exist to make the code easier for humans to understand.

---

## 3. Variables

Bash can store values in variables.

Example:

```bash
name="Hope0-arch"
```

To access the value stored inside the variable, use `$`:

```bash
echo "$name"
```

Output:

```text
Hope0-arch
```

So:

```text
name     → variable name
$name    → value stored inside name
```

Bash also provides many useful existing variables.

For example:

```bash
echo "$HOME"
```

might print:

```text
/home/username
```

while:

```bash
echo "$PATH"
```

might print:

```text
/home/username/.local/bin:/usr/local/bin:/usr/bin:/bin
```

`$PATH` contains directories Bash searches when you type a command without giving its complete path.

---

## 4. Script Arguments

When we run:

```bash
freshlocate hello.c
```

the script receives `hello.c` as an argument.

Bash provides special parameters for accessing arguments.

```text
$0  → script name
$1  → first argument
$2  → second argument
$#  → number of arguments
$@  → all arguments
```

For example:

```bash
if (( $# == 0 )); then
```

means:

> If the number of arguments is zero...

The script can then print its usage message and exit.

---

## 5. Exit Codes

Linux commands return an **exit status** when they finish.

Conventionally:

```text
0       → success
non-zero → some type of failure
```

The previous command's exit status can be accessed through:

```bash
$?
```

---

## 6. `&&`

The script uses:

```bash
sudo updatedb && locate -- "$@"
```

`&&` means:

> Run the command on the right only if the command on the left succeeds.

So:

```text
sudo updatedb
      ↓
successful?
   /       \
 yes        no
  ↓          ↓
locate      stop
```

---

## 7. Why `"$@"`?

`"$@"` represents all arguments supplied to the script while preserving them correctly as separate arguments.

Therefore:

```bash
freshlocate hello.c
```

eventually causes the script to run something equivalent to:

```bash
locate -- hello.c
```

The `--` tells `locate`:

> Stop interpreting following values as command-line options.

---

# Git Basics

Git is a **version-control system**.

GitHub is a website/service that can store a remote copy of a Git repository.

They are not the same thing.

```text
project files
     ↓
    Git
local version history
     ↓
   GitHub
remote copy
```

Git works even without internet access.

---

## Git Mental Model

The basic workflow is:

```text
WORKING DIRECTORY
files currently being edited
        ↓
      git add
        ↓
STAGING AREA
changes selected for next commit
        ↓
    git commit
        ↓
LOCAL GIT HISTORY
saved snapshots
        ↓
     git push
        ↓
GITHUB / REMOTE
```

---

# Starting a New Git Project

## 1. Create the Project

```bash
mkdir -p ~/projects/<project-name>
cd ~/projects/<project-name>
```

---

## 2. Initialize Git

The simplest command is:

```bash
git init
```

This creates a hidden:

```text
.git/
```

directory.

That directory stores Git's repository information and history.

If your Git version supports it, you can also initialize directly with a `main` branch:

```bash
git init -b main
```

If Git created a branch called `master`, it can later be renamed:

```bash
git branch -M main
```

---

## 3. Check the Repository

```bash
git status
```

Suppose you created:

```text
read.txt
```

Git may show:

```text
Untracked files:
    read.txt
```

**Untracked** means:

> Git sees the file, but it has not yet been included in version control.

---

## 4. Stage Changes

Use:

```bash
git add <file_name>
```

Example:

```bash
git add read.txt
```

Now the current version of `read.txt` is placed in the **staging area**.

The staging area contains the changes that will go into the next commit.

Check again:

```bash
git status
```

You may now see:

```text
Changes to be committed:
    new file: read.txt
```

---

## 5. Configure Your Git Identity

Git stores an author name and email inside commits.

Usually this only needs to be configured once for your Linux user:

```bash
git config --global user.name "Your Name"
git config --global user.email "Your Email"
```

This answers:

> Who created this commit?

It does **not** log you into GitHub.

GitHub authentication is separate.

---

## 6. Commit

Save the staged changes into your local Git history:

```bash
git commit -m "Describe your changes"
```

Example:

```bash
git commit -m "Add freshlocate v0.1"
```

A commit is a saved local snapshot.

It does **not** upload anything to GitHub.

---

# Connecting the Project to GitHub

Create an empty repository on GitHub.

Then connect the local repository to it:

```bash
git remote add origin <repository-url>
```

Example:

```bash
git remote add origin https://github.com/username/project.git
```

`origin` is simply the conventional nickname for the remote repository.

Check it with:

```bash
git remote -v
```

---

# Push to GitHub

For the first push:

```bash
git push -u origin main
```

Breakdown:

```text
git push
→ upload local commits

origin
→ remote repository

main
→ branch being uploaded

-u
→ remember the relationship between local main and origin/main
```

After that, future pushes normally only require:

```bash
git push
```

---

# GitHub Authentication

Git identity and GitHub authentication are two separate concepts.

```text
git config user.name / user.email
        ↓
identifies who wrote a commit

GitHub authentication
        ↓
proves you are allowed to push
```

For an HTTPS remote such as:

```text
https://github.com/username/project.git
```

GitHub may require a **Personal Access Token (PAT)** rather than your normal GitHub account password.

For an SSH remote such as:

```text
git@github.com:username/project.git
```

authentication normally uses an SSH key.

---

# Normal Git Workflow After Setup

Once the project and GitHub remote are already configured, the everyday workflow is much smaller:

```bash
git status
git diff

git add <file_name>
git commit -m "Describe what changed"
git push
```

Mental model:

```text
edit
 ↓
git add
 ↓
stage
 ↓
git commit
 ↓
save locally
 ↓
git push
 ↓
GitHub
```

---

# Useful Git Commands for Now

These are enough for basic solo projects:

```bash
git status
git diff
git add
git commit
git log --oneline
git remote -v
git push
```

There are many more Git commands, but there is no need to learn all of them before they become useful.

---

# Project Status

`freshlocate v0.1` is intentionally small.

The main purpose of this version was to:

- understand how `locate` works
- learn Bash scripting
- understand `$PATH`
- create a custom Linux command
- learn executable permissions
- understand script arguments
- learn exit statuses and `&&`
- use Git for version control
- publish the project on GitHub

Now we can start improving it.

**IN THE NEXT GIT COMMIT HEH 😎**
