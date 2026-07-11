# Your Shell & Dotfiles Setup Guide
### macOS (zsh) → Debian Linux — from zero to a real developer environment

---

## How to use this guide

Work through it in order the first time. Each section builds on the last. Once your Mac is set up, skip straight to **Part 5** when you're ready to do the same thing on Debian — everything before that is background you'll already have.

Don't try to absorb every detail in one sitting. Get Part 1–3 working, actually use your terminal for a few days, then come back for the rest.

---

## Part 1 — The mental model (read this first)

Coming from Photoshop/Illustrator/Figma, here's the closest analogy: those apps are all **GUI tools** — you click buttons and drag panels, and the app translates that into changes to a file. The command line is the same idea, minus the buttons. You type an instruction, press Enter, and a program carries it out directly. Nothing is hidden behind a menu — which is exactly why it feels intimidating at first, and exactly why it becomes powerful once it clicks.

Two words get mixed up constantly, so let's separate them:

| Term | What it actually is |
|---|---|
| **Terminal** | The *window/app* you type into. It just displays text — it doesn't understand your commands. Examples: Terminal.app, iTerm2, GNOME Terminal, Konsole. |
| **Shell** | The *program running inside the terminal* that reads what you type and does something with it. Examples: `bash`, `zsh`, `fish`. |

So "configuring your shell" means configuring **zsh** or **bash** — not the terminal app itself. You could switch from Terminal.app to iTerm2 tomorrow and your shell config wouldn't care; it's a different layer.

**bash vs zsh, briefly:** they're siblings — both descended from the original Unix shell, both use nearly identical scripting syntax. zsh added quality-of-life features (better autocomplete, better history, plugin support) which is why it became the default shell on macOS in 2019 and why most modern terminal setups — including the ones you found — are built on it. Debian still defaults to `bash`, which is why Part 5 exists.

---

## Part 2 — What dotfiles actually are

Nothing mystical here: on macOS and Linux, **any file or folder whose name starts with a dot is hidden from normal directory listings by convention.** That's it — it's not a special file type, it's just a "don't clutter my view" flag that Finder, Nautilus, and `ls` all respect. Config files adopted this convention decades ago so your home folder wouldn't be full of clutter, and the name "dotfiles" just stuck as a catch-all term for **your personal config files**, most of which live directly in your home folder (`~`).

Run this to actually see yours right now:

```bash
ls -la ~
```

You'll see things like `.zshrc`, `.gitconfig`, `.ssh/` — all normal files, just hidden by default.

### The dotfiles that control your shell, and when each one loads

This is the part people usually never get a straight answer on, so here it is:

| File (zsh) | Bash equivalent | Loads when | Put here |
|---|---|---|---|
| `.zshenv` | *(no direct equivalent)* | **Every single time**, no matter how the shell starts — new tab, script, subprocess, everything | Things that must always be true: `PATH` exports, environment variables |
| `.zprofile` | `.bash_profile` | Once, only for a **login shell** | Login-time setup (rare to need this for basics) |
| `.zshrc` | `.bashrc` | Every **interactive shell** (basically: every new terminal tab/window) | Aliases, prompt, plugins, functions — 95% of your customization goes here |
| `.zlogin` | — | After `.zshrc`, login shells only | Rarely used |

**A "login shell" vs an "interactive shell"** is the one genuinely confusing distinction, and it's also exactly where macOS and Linux behave differently — which matters a lot for you specifically, since you're setting this up on both:

- **macOS Terminal.app and iTerm2** launch every new window/tab as a **login shell**, a quirk inherited from old Unix terminal behavior. That means on your Mac, both `.zprofile` *and* `.zshrc` run every time you open a tab.
- **Most Linux terminal emulators** (the one inside your Debian install included) launch new windows as **interactive, non-login** shells. Only `.zshrc` runs — `.zprofile` gets skipped entirely.

This is exactly why the config you found puts the one thing that matters everywhere (`ZDOTDIR`, which we'll get to) inside `.zshenv` instead of `.zprofile` — `.zshenv` is the only file guaranteed to load on both operating systems, in every scenario. Keep this rule of thumb and you'll never be confused by "why did this work on my Mac but not on Linux": **if it must always be true, `.zshenv`; if it's a shell convenience, `.zshrc`.**

---

## Part 3 — Setting up zsh on your MacBook, step by step

### Step 1: Confirm you're already on zsh

macOS has defaulted to zsh since Catalina (2019), so you likely don't need to change anything:

```bash
echo $SHELL
```

If that prints `/bin/zsh`, you're set. If it prints `/bin/bash`, run `chsh -s /bin/zsh` and restart your terminal.

### Step 2: Pick your terminal app

Terminal.app (built-in) works fine for everything in this guide. **iTerm2** is the popular upgrade — split panes, better color support, more configurability — and it's a safe, free install if you want it: [iterm2.com](https://iterm2.com). Nothing below depends on which one you pick.

### Step 3: Install Homebrew

Homebrew is the package manager you'll use to install almost everything else on macOS. Check if you already have it first:

```bash
brew --version
```

If that fails, install it:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Follow the on-screen instructions at the end — it'll tell you to add a couple of lines to your shell config to put Homebrew on your `PATH`. Do that now:

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

(This goes in `.zprofile`, not `.zshrc` — it's exactly the "must be true at login" case from Part 2.)

### Step 4: Create a real `.zshrc`

Open it in any text editor — `nano ~/.zshrc` works fine if you don't have a preference yet — and start with this. Every line is commented so you know what it does and can delete anything you don't want:

```bash
# ---- History ----
HISTSIZE=10000
SAVEHIST=10000
setopt SHARE_HISTORY        # share history across all open terminal tabs
setopt HIST_IGNORE_DUPS     # don't save duplicate consecutive commands

# ---- Quality-of-life aliases ----
alias ll='ls -lah'
alias ..='cd ..'
alias ...='cd ../..'
alias gs='git status'
alias ga='git add'
alias gc='git commit'
alias gp='git push'

# A shortcut to make a folder and cd into it in one go
mkcd() { mkdir -p "$1" && cd "$1"; }
```

Save, then reload it without closing your tab:

```bash
source ~/.zshrc
```

Try `ll` — if you see a nicely formatted file listing, it worked.

### Step 5: A prompt that actually tells you something

The default zsh prompt just shows your username and folder. **Starship** is a fast, cross-shell prompt that shows your current directory, git branch, git status, and language versions (Python, Node, etc. — genuinely useful once you're coding) automatically:

```bash
brew install starship
```

Add one line to the very bottom of `.zshrc`:

```bash
eval "$(starship init zsh)"
```

Reload (`source ~/.zshrc`) and your prompt will change immediately.

### Step 6: Two plugins worth having from day one

These aren't essential, but they remove a lot of friction while you're learning:

- **zsh-autosuggestions** — as you type, it shows a faint gray suggestion based on your history. Press `→` to accept it.
- **zsh-syntax-highlighting** — commands turn green if valid, red if not, *before* you press Enter.

Install by cloning them into a plugins folder and sourcing them — no framework required:

```bash
mkdir -p ~/.config/zsh/plugins
git clone https://github.com/zsh-users/zsh-autosuggestions ~/.config/zsh/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting ~/.config/zsh/plugins/zsh-syntax-highlighting
```

Add to `.zshrc` (syntax-highlighting must be sourced **last**, after everything else):

```bash
source ~/.config/zsh/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh
source ~/.config/zsh/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

Reload once more. Type a command and watch it happen.

> **Aside on the video/repos you found:** what you were looking at is a "framework-free" style of zsh setup — no oh-my-zsh, just plugins cloned by hand and a `ZDOTDIR` pointing at a tidy config folder instead of a cluttered home directory. It's a genuinely good, minimal approach, and Steps 4–6 above are the same philosophy, just spelled out slower. If you'd rather have an all-in-one framework that manages plugins and themes for you automatically, **oh-my-zsh** is the other extremely common path — worth knowing it exists, but I'd get comfortable with the manual version first since it teaches you what's actually happening.

### Step 7: Level up — organize like the repo you found (optional)

Once Steps 1–6 feel boring rather than confusing, this is the natural next move: instead of one `.zshrc` with everything crammed in, redirect zsh to a whole *folder* of config files. Add this single line to `.zshenv` (remember — the file that always loads, on any OS):

```bash
export ZDOTDIR="$HOME/.config/zsh"
```

Then move your `.zshrc` into `~/.config/zsh/.zshrc`, and optionally split it into pieces you `source` from there — `aliases.zsh`, `prompt.zsh`, `plugins.zsh`, etc. This is exactly the structure in the `radleylewis/zsh` repo you referenced — now that you understand *why* each file exists, that repo should read as far less mysterious if you want to study it or borrow from it.

---

## Part 4 — Keep one config on two machines: dotfiles + git

Since you're setting this up on a Mac now and Debian later, you'll want the same `.zshrc` in both places without copy-pasting by hand. The standard trick (also used in the `radleylewis/dotfiles` repo you found) is a **bare git repository** stored outside your home folder, that tracks files *inside* your home folder:

```bash
git init --bare $HOME/.dotfiles
alias dotfiles='git --git-dir=$HOME/.dotfiles --work-tree=$HOME'
dotfiles config --local status.showUntrackedFiles no
```

Add that `alias dotfiles=...` line permanently to your `.zshrc` too. Now `dotfiles` behaves like `git`, but only sees files you explicitly add:

```bash
dotfiles add ~/.zshrc ~/.config/zsh
dotfiles commit -m "Add zsh config"
dotfiles remote add origin git@github.com:yourusername/dotfiles.git
dotfiles push -u origin main
```

On any new machine (your future native Debian install), you'd clone it back with:

```bash
git clone --bare git@github.com:yourusername/dotfiles.git $HOME/.dotfiles
dotfiles config --local status.showUntrackedFiles no
dotfiles checkout
```

This is a nice-to-have, not a blocker — feel free to skip it until Part 5 is done and come back.

---

## Part 5 — Adapting everything for Debian

Good news: **Parts 2, 4, and the contents of your `.zshrc` transfer over unchanged.** Dotfiles are just text — the shell reads them the same way regardless of OS. What's different is *how you install things*, because Debian uses `apt` instead of Homebrew.

This applies identically whether you're doing it in the VirtualBox VM right now or the native ThinkPad install later.

### Step 1: Install zsh and make it your default

```bash
sudo apt update
sudo apt install zsh git curl -y
chsh -s $(which zsh)
```

Log out and back in (or just close and reopen the terminal) for the shell change to take effect.

### Step 2: Reuse your `.zshrc`

If you set up the git bare-repo trick from Part 4:

```bash
git clone --bare git@github.com:yourusername/dotfiles.git $HOME/.dotfiles
alias dotfiles='git --git-dir=$HOME/.dotfiles --work-tree=$HOME'
dotfiles config --local status.showUntrackedFiles no
dotfiles checkout
```

Otherwise, just copy the file over manually (USB drive, `scp`, whatever's easiest) and drop it at `~/.zshrc`.

### Step 3: Same plugins, same starship — different install commands

```bash
# zsh-autosuggestions / zsh-syntax-highlighting: identical git clone commands as macOS

# Starship (no Homebrew needed):
curl -sS https://starship.rs/install.sh | sh
```

### Step 4: One Debian/Ubuntu quirk worth knowing about

A couple of the modern CLI tools you'll see recommended alongside zsh setups (`bat`, `fd`) get installed under **different binary names** on Debian/Ubuntu because those short names were already taken by unrelated packages in the official repos:

```bash
sudo apt install bat fd-find -y
mkdir -p ~/.local/bin
ln -s $(which batcat) ~/.local/bin/bat
ln -s $(which fdfind) ~/.local/bin/fd
```

That's not a mistake on your part if you hit it — it's a known Debian/Ubuntu packaging quirk, and now you know the fix.

---

## Quick-reference cheat sheet

| I want to... | Do this |
|---|---|
| Edit my shell config | `nano ~/.zshrc` (or your editor of choice) |
| Apply changes without restarting | `source ~/.zshrc` |
| See what's actually hidden in my home folder | `ls -la ~` |
| Check my current shell | `echo $SHELL` |
| See my PATH | `echo $PATH` |
| Add a folder to PATH | `export PATH="$HOME/some/folder:$PATH"` (in `.zprofile`/`.zshenv`) |
| Make a shortcut for a command | `alias name='full command'` (in `.zshrc`) |

---

## What's next

This covers the environment itself. When you're ready, I'm happy to put together a separate learning path for actually getting into Python, C, C#, JavaScript, and eventually SQL/databases — ideally sequenced so each one reinforces the last rather than context-switching constantly. Just say the word.
