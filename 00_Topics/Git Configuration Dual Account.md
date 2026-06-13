#type/topic #topic/git-configuration #for/all 

> [!author] Prompt
> how to configure git only for one user in macos

> [!author] Gemini
> To configure Git for a single user on macOS so that every repository you work on uses the same identity, you should ==set your configuration at the **global** level==. This stores your details in a `.gitconfig` file located in your home directory (`~/.gitconfig`), which Git applies to all your projects.
> # Step-by-Step Configuration
> 1. **Open Terminal**  
>     You can find Terminal in your **Applications > Utilities** folder or by searching with Spotlight (Cmd + Space).
> 2. **Set Your Username**  
>     Run the following command, replacing `"Your Name"` with your actual name:  
>     `git config --global user.name "Your Name"`
> 3. **Set Your Email**  
>     Run the following command, replacing `"your_email@example.com"` with the email you use for services like GitHub or GitLab:  
>     `git config --global user.email "your_email@example.com"`
> 4. **Verify the Settings**  
>     To confirm your information was saved correctly, use:  
>     `git config --global --list`
> # Managing Multiple Users (Advanced)
> If you need to use different identities for specific projects (e.g., a personal email for one folder and a work email for another), you have two main options:
> 
> - **Repository-Specific (Local):** Navigate into a specific project folder and run the commands _without_ the `--global` flag. This overrides your global settings only for that one repository.
>     - `git config user.name "Specific Name"`
>     - `git config user.email "specific_email@example.com"`
> - **Conditional Includes:** You can automate this by editing your global `~/.gitconfig` to include separate config files based on the directory path

---

> [!author] Prompt
> Thanks, helpful information. I'm a noob user, learning Git and Github stuff. I saw a couple of tutorial in YouTube and they say to configure Git via the command you told me. But reading further, I realize that not only exists the `--global` flag. I want to know, what is the difference between global, local and system flags? Explain me, and give me examples to allow me to understand where and how to use this options for my workflow.

> [!author] Gemini
> Git saves your settings in three different levels of configuration files. The difference between **local**, **global**, and **system** comes down to **scope**: which repositories and which users on your Mac are affected by the changes.
> 
> Here is the breakdown of how they work, ranked from the most specific to the most general.
> 
> ---
> 
> 1. Local (`--local`)
> 
> This flag applies settings **only to the specific repository** you are currently inside.
> 
> - **Where it is saved:** Inside the hidden `.git/config` file of that specific project folder.
> - **When to use it:** When you need a one-off exception for a single project (like using a work email for a client project, while keeping your personal email for everything else).
> 
> **Example Scenario:**  
> You have a specific freelance project where you must use your client-provided email address.
> 
> bash
> 
> ```
> cd ~/Projects/client-project
> git config --local user.email "your.name@clientcompany.com"
> ```
> 
> Use code with caution.
> 
> _Result: Only commits made inside `client-project` will use this email._
> 
> ---
> 
> 2. Global (`--global`)
> 
> This flag applies settings to **all repositories owned by your Mac user account**.
> 
> - **Where it is saved:** In a hidden file in your user home directory (`~/.gitconfig`).
> - **When to use it:** This is your primary "fallback" or default layer. Use it for settings you want active across 95% of your projects, like your main name, favorite text editor, or color themes.
> 
> **Example Scenario:**  
> Setting your default Git username and your preferred text editor (like VS Code) for all your personal coding.
> 
> bash
> 
> ```
> git config --global user.name "John Doe"
> git config --global core.editor "code --wait"
> ```
> 
> Use code with caution.
> 
> _Result: Every new or existing repository you open will use these settings by default._
> 
> ---
> 
> 3. System (`--system`)
> 
> This flag applies settings to **every single user and every repository on the entire Mac**.
> 
> - **Where it is saved:** In a system-wide file (usually `/etc/gitconfig`). Because it affects the whole computer, you usually need admin privileges (`sudo`) to change it.
> - **When to use it:** As a beginner, **you will almost never use this**. System administrators use it to enforce corporate security policies or default paths for all employees sharing a machine.
> 
> **Example Scenario:**  
> A school IT administrator wants to make sure Git automatically ignores annoying macOS system files (like `.DS_Store`) for every student logging into a lab iMac.
> 
> bash
> 
> ```
> sudo git config --system core.excludesfile /etc/gitignore_global
> ```
> 
> Use code with caution.
> 
> ---
> 
> Summary Table for Quick Reference
> 
> |Flag|Scope|Location on Mac|Priority|
> |---|---|---|---|
> |**`--local`**|Current folder/project only|`[project]/.git/config`|🥇 **Highest** (Overrides all)|
> |**`--global`**|Your Mac user account|`~/.gitconfig`|🥈 **Medium** (Overrides system)|
> |**`--system`**|Entire computer (all users)|`/etc/gitconfig`|🥉 **Lowest** (Fallback)|
> 
> How Git Resolves Conflicts (The Priority Rule)
> 
> Git always checks settings from **most specific to least specific** (Local ➡️ Global ➡️ System).
> 
> If you set your email to `personal@me.com` **globally**, but set it to `work@company.com` **locally** inside a specific folder, Git will use the **local** email when you are in that folder.
> 
> ---
> 
> I can help you build your custom Git workflow. If you want, tell me:
> 
> - Do you plan to use **separate GitHub accounts** (like one for school/work and one for personal projects)?
> - What **code editor** (VS Code, Cursor, Windsurf, etc.) do you plan to use?
> 
> I can give you the exact commands to link them together seamlessly.

---

> [!author] Prompt
> Oh thank you very much; the information is much clearer and easier to understand now. Yes, I want to plan to use separate Github accounts for my work and personal projects. I'm currently planning to use Neovim for my code editor, do you think it's a good idea to use that code editor?

> [!author] Gemini
> **Yes, Neovim is an excellent code editor**, but it comes with a steep learning curve because it is entirely keyboard-driven and terminal-based. Since you are just starting with Git and GitHub, using Neovim will require you to learn text editing shortcuts at the same time you are learning coding and version control.
> 
> Many beginners love it because it is incredibly fast, lightweight, and teaches you to navigate code without a mouse. If you enjoy customizing your tools, it is a great choice; if you want something that "just works" out of the box, a graphical editor like Visual Studio Code might feel less overwhelming at first.
> 
> If you choose to move forward with Neovim, here is exactly how to set it as your default Git editor and safely manage your dual GitHub accounts on your Mac.
> 
> ---
> 
> Step 1: Set Neovim as your Default Git Editor
> 
> By default, Git often uses basic `vim` or `nano` for writing commit messages. To force Git to use Neovim globally, run this command in your Terminal:
> 
> bash
> 
> ```
> git config --global core.editor "nvim"
> ```
> 
> Use code with caution.
> 
> _Now, whenever you run `git commit` without a message, Git will automatically open Neovim for you to type your note._
> 
> ---
> 
> Step 2: Manage Dual GitHub Accounts (Personal vs. Work)
> 
> The easiest, most foolproof way for a beginner to handle two GitHub accounts on one Mac is to use **Conditional Includes** [1]. This method automatically swaps your Git identity based on the folder you are working in, so you never accidentally commit to a work project using your personal email.
> 
> 1. Organize your folders
> 
> Separate your projects into two distinct directories in your Mac's home folder:
> 
> - `~/Projects/personal/` (All your personal repositories)
> - `~/Projects/work/` (All your professional or school repositories)
> 
> 2. Create specific sub-config files
> 
> You will create two tiny configuration files—one for each identity. Open your terminal and run these commands to create them using Neovim:
> 
> For your **personal** account:
> 
> bash
> 
> ```
> nvim ~/.gitconfig-personal
> ```
> 
> Use code with caution.
> 
> Type `i` to enter Insert Mode, paste the following text (replace with your personal details), press `Esc`, and type `:wq` then `Enter` to save and exit:
> 
> ini
> 
> ```
> [user]
>     name = Your Personal Name
>     email = personal_email@example.com
> ```
> 
> Use code with caution.
> 
> For your **work** account:
> 
> bash
> 
> ```
> nvim ~/.gitconfig-work
> ```
> 
> Use code with caution.
> 
> Press `i`, paste the following, press `Esc`, and type `:wq` then `Enter`:
> 
> ini
> 
> ```
> [user]
>     name = Your Work Name
>     email = work_email@company.com
> ```
> 
> Use code with caution.
> 
> 3. Update your main Global Config
> 
> Now, link them together in your main global configuration file so Git knows when to switch them:
> 
> bash
> 
> ```
> nvim ~/.gitconfig
> ```
> 
> Use code with caution.
> 
> Clear out any existing `[user]` block and add these conditional rules to the bottom of the file:
> 
> ini
> 
> ```
> # Default fallback (optional)
> [user]
>     name = Your Personal Name
>     email = personal_email@example.com
> 
> # If the project is inside the personal folder, use personal config
> [includeIf "gitdir:~/Projects/personal/"]
>     path = ~/.gitconfig-personal
> 
> # If the project is inside the work folder, use work config
> [includeIf "gitdir:~/Projects/work/"]
>     path = ~/.gitconfig-work
> ```
> 
> Use code with caution.
> 
> Save and exit (`Esc`, `:wq`, `Enter`).
> 
> ---
> 
> How to test your new setup
> 
> To make sure it works, create a test folder inside your work directory and check which email Git uses:
> 
> bash
> 
> ```
> mkdir -p ~/Projects/work/test-repo
> cd ~/Projects/work/test-repo
> git init
> git config user.email
> ```
> 
> Use code with caution.
> 
> _The terminal should instantly print out your `work_email@company.com`!_
> 
> ---
> 
> To make sure your GitHub accounts connect securely without fighting each other, we will need to handle authentication next. Would you like to set up **SSH Keys** for both accounts, or would you prefer using the **GitHub CLI (gh)** tool to log in?

---

> [!author] Prompt
> Ok helpful information, I'm going to test it and I would let you name if it works, but for now I have a doubt. If I using and switching from Mac to Linux (Debian) How the workflow and configuration needs to be? Is the same step or they involve different process? Please, guide me through this extra workflow. In my daily I work on both, Linux and macOS. I want to set up these things to avoid struggling and get a messy with my personal and work account.Once we've sorted all this out, we'll get back to the part where I need to set up my Github accounts using Github Access Tokens. Or let me know what would you recommend me to use to secure my credentials, just for your know, I have been using Github's PAT configuration with Pass Standar Unix manager and Git Credential Manager.

> [!author] Gemini
> 🐧 Cross-Platform Compatibility: macOS vs. Linux (Debian)
> 
> Good news: **The workflow and configuration steps are exactly identical.**
> 
> Because Git operates the same way across all Unix-like operating systems, you can use the exact same directory structures, conditional includes, and `.gitconfig` files on both macOS and Debian Linux.
> 
> Minor Adjustments to Watch Out For:
> 
> 1. **The Home Directory Symbol (`~`):** On macOS, your home directory expands to `/Users/username/`. On Debian, it expands to `/home/username/`. Git natively understands the `~` shortcut, so as long as you use `~/Projects/...` in your configuration files, the paths will translate automatically across both systems.
> 2. **Neovim Installation:** Neovim is built into macOS via Homebrew (`brew install neovim`), but on Debian, you will want to install it using the advanced packaging tool:
>     
>     bash
>     
>     ```
>     sudo apt update
>     sudo apt install neovim
>     ```
>     
>     Use code with caution.
>     
> 
> ---
> 
> 🔑 Credential Security Recommendation
> 
> Since you already use **Pass (the standard Unix password manager)** and **Git Credential Manager (GCM)**, you are actually using the highest industry-standard security available!
> 
> However, because you are managing **two separate GitHub accounts on two different operating systems**, Personal Access Tokens (PATs) combined with standard HTTPS URLs can become incredibly messy. GCM will constantly try to guess which token belongs to which folder, often resulting in authentication errors.
> 
> The Pro Recommendation: Switch to SSH Keys
> 
> For dual-account, cross-platform workflows using terminal tools like Neovim, **SSH Keys** are significantly easier to maintain than PATs.
> 
> - **Why it's better:** You don't need a credential manager at all. You map your Personal SSH key to your personal GitHub account, and your Work SSH key to your work GitHub account.
> - **How it works seamlessly with your folders:** You configure a tiny SSH `config` file on your Mac and Debian machine. When you clone a repo in your work folder, it automatically signs you in with your work identity.
> 
> ---
> 
> Step-by-Step: Setting up Dual SSH Keys for macOS and Debian
> 
> Follow these steps on **both** your Mac and your Debian machine to lock down your multi-account security.
> 
> 1. Generate Two Separate SSH Keys
> 
> Open your terminal and generate a key for each account. (Press `Enter` to accept the default file paths when prompted, but **rename the files** as shown below so they don't overwrite each other).
> 
> **Personal Key:**
> 
> bash
> 
> ```
> ssh-keygen -t ed25519 -C "personal_email@example.com" -f ~/.ssh/id_ed25519_personal
> ```
> 
> Use code with caution.
> 
> **Work Key:**
> 
> bash
> 
> ```
> ssh-keygen -t ed25519 -C "work_email@company.com" -f ~/.ssh/id_ed25519_work
> ```
> 
> Use code with caution.
> 
> 2. Configure the SSH Config File
> 
> Create or open your SSH configuration file using Neovim:
> 
> bash
> 
> ```
> nvim ~/.ssh/config
> ```
> 
> Use code with caution.
> 
> Paste the following configurations. This creates two distinct "nicknames" (`github.com-personal` and `github.com-work`) so your computer knows exactly which security key to pass to GitHub.
> 
> text
> 
> ```
> # Personal GitHub Account
> Host github.com-personal
>     HostName github.com
>     User git
>     IdentityFile ~/.ssh/id_ed25519_personal
>     IdentitiesOnly yes
> 
> # Work GitHub Account
> Host github.com-work
>     HostName github.com
>     User git
>     IdentityFile ~/.ssh/id_ed25519_work
>     IdentitiesOnly yes
> ```
> 
> Use code with caution.
> 
> Save and exit (`Esc`, `:wq`, `Enter`).
> 
> 3. Add the Keys to Your GitHub Accounts
> 
> You need to share the public half of your keys with GitHub.
> 
> 4. Copy your personal public key to your clipboard:
>     - **macOS:** `pbcopy < ~/.ssh/id_ed25519_personal.pub`
>     - **Debian:** `cat ~/.ssh/id_ed25519_personal.pub` (then manually highlight and copy)
> 5. Go to GitHub (Logged into your **Personal** account) -> **Settings -> SSH and GPG keys -> New SSH Key** and paste it.
> 6. Log out of GitHub, log into your **Work** account, and repeat the process using your work key (`id_ed25519_work.pub`).
> 
> ---
> 
> 🚀 The Final Step: How to Clone Repositories
> 
> Because you are using aliases in your SSH config file, you must slightly alter the URL when you clone a repository for the first time.
> 
> Instead of copying the normal SSH link from GitHub (`git@github.com:username/repo.git`), you will change `github.com`to match the specific alias you created.
> 
> - **For a Personal Repo (inside `~/Projects/personal/`):**
>     
>     bash
>     
>     ```
>     git clone git@github.com-personal:your-personal-username/your-repo.git
>     ```
>     
>     Use code with caution.
>     
> - **For a Work Repo (inside `~/Projects/work/`):**
>     
>     bash
>     
>     ```
>     git clone git@github.com-work:your-work-organization/work-repo.git
>     ```
>     
>     Use code with caution.
>     
> 
> Once cloned, your folder layout will automatically control your Git name/email, and your SSH alias will automatically control your login security—leaving you with zero credential conflicts on both macOS and Linux!
> 
> ---
> 
> If you prefer to stick strictly with your current setup, let me know:
> 
> - Do you want to try setting up **SSH** first, or do you want to proceed with configuring **Git Credential Manager and Pass** to handle the PATs across both OS environments?

---

> [!author] Prompt
> Ok thank you, I take notes and I will implement this steps. Since you recommend me using SSH keys to handle the authentication I want to know and learn also how to set up using Github CLI (gh) tool to log in and as well how to set up using Git Credential Manager and Pass to handle the PATs across both OS environments. Here is my goal, learn with:1. SSH Keys (you already explain to me)2. Github CLI3. Github Access Token (PAT)

> [!author] Gemini
> It is incredibly smart to learn all three methods. Understanding SSH, the GitHub CLI, and Git Credential Manager (GCM) gives you total flexibility, no matter what environment you find yourself working in.
> 
> Here is how to set up **GitHub CLI** and **PATs with GCM + Pass** for a dual-account, cross-platform workflow on macOS and Debian.
> 
> ---
> 
> Method 2: Authenticatin via GitHub CLI (`gh`)
> 
> The GitHub CLI (`gh`) is a powerful tool that automates authentication. Instead of dealing with passwords or keys manually, you just log in through your browser.
> 
> To manage two accounts, you will use **environment variables** to tell the CLI where to save each account's credentials.
> 
> 1. Install GitHub CLI
> 
> - **macOS:** `brew install gh`
> - **Debian:** `sudo apt install gh`
> 
> 2. Log into your Personal Account
> 
> Open your terminal and run:
> 
> bash
> 
> ```
> gh auth login
> ```
> 
> Use code with caution.
> 
> - Select **GitHub.com**.
> - Select **HTTPS** as your preferred protocol.
> - When asked to authenticate, choose **Log in with a web browser**, copy the one-time code, and complete the login in your browser.
> 
> 3. Log into your Work Account
> 
> By default, logging into a second account will overwrite the first one. To prevent this, you must temporarily change the configuration directory using an environment variable before logging in:
> 
> bash
> 
> ```
> # Create a separate config directory for work
> mkdir -p ~/.config/gh-work
> 
> # Tell gh to use this new directory temporarily
> export GH_CONFIG_DIR=~/.config/gh-work
> 
> # Log into your work account
> gh auth login
> ```
> 
> Use code with caution.
> 
> _Follow the same browser login prompts for your work account._
> 
> 4. Automate the Switcher
> 
> To make this seamless, add aliases to your shell configuration file (`~/.zshrc` on Mac, or `~/.bashrc` on Debian) using Neovim:
> 
> bash
> 
> ```
> nvim ~/.zshrc  # (Use ~/.bashrc if on Debian)
> ```
> 
> Use code with caution.
> 
> Add these lines to the bottom:
> 
> bash
> 
> ```
> # GitHub CLI Account Switchers
> alias gh-personal="unset GH_CONFIG_DIR"
> alias gh-work="export GH_CONFIG_DIR=~/.config/gh-work"
> ```
> 
> Use code with caution.
> 
> Save and exit (`:wq`). Run `source ~/.zshrc` (or `source ~/.bashrc`) to apply.
> 
> - Now, typing `gh-work` switches your CLI to your work account, and `gh-personal` switches it back.
> 
> ---
> 
> Method 3: GitHub Access Token (PAT) with GCM and Pass
> 
> This is your existing preference. Git Credential Manager (GCM) securely stores your PATs. On macOS, it defaults to the Apple Keychain. On Debian, it natively integrates with **Pass**.
> 
> 1. Generate Your PATs on GitHub
> 
> For **both** accounts, go to GitHub -> **Settings -> Developer Settings -> Personal Access Tokens (Tokens classic)**. Generate a token with `repo` scopes and copy it securely.
> 
> 2. Install Git Credential Manager (GCM)
> 
> - **macOS:** `brew install --cask git-credential-manager`
> - **Debian:** Download the latest `.deb` package from the GCM GitHub Releases page and install it via `sudo apt install ./gcm-linux_amd64.deb`.
> 
> 3. Configure GCM to use `pass` on Debian
> 
> On your Debian machine, tell GCM to use `pass` as its secure store:
> 
> bash
> 
> ```
> git config --global credential.credentialStore pass
> ```
> 
> Use code with caution.
> 
> _(Make sure your pass store is initialized via `pass init <your-gpg-id>` before doing this)._
> 
> 4. The Trick for Dual Accounts: Explicit Usernames
> 
> Normally, GCM caches one token per host (`github.com`). If you have two accounts, it gets confused. To fix this, you must force Git to include your GitHub username in the URL so GCM can tell the accounts apart.
> 
> Run this command **globally** on both Mac and Debian:
> 
> bash
> 
> ```
> git config --global credential.useHttpPath true
> ```
> 
> Use code with caution.
> 
> 5. Cloning and Logging In
> 
> When you clone a repository via HTTPS, you **must manually insert your specific GitHub username** into the URL.
> 
> - **For Personal (Your personal username is `dev-personal`):**
>     
>     bash
>     
>     ```
>     git clone https://dev-personal@://github.com
>     ```
>     
>     Use code with caution.
>     
>     _GCM will pop up and ask for your password. Paste your **Personal PAT** here._
> - **For Work (Your work username is `dev-work`):**
>     
>     bash
>     
>     ```
>     git clone https://dev-work@://github.com
>     ```
>     
>     Use code with caution.
>     
>     _GCM will pop up again, recognizing this as a completely different user. Paste your **Work PAT** here._
> 
> Because of `useHttpPath`, GCM will store `dev-personal` and `dev-work` as separate entries inside your Mac Keychain and your Debian `pass` manager without overwriting each other.
> 
> ---
> 
> Now that you have all three blueprints, you can test them out to see which fits your terminal workflow best.
> 
> Would you like help setting up a **custom Neovim shortcut** to easily view your current Git configuration layer while you work, or should we look into setting up a global `.gitignore` file next?

---

> [!author] Prompt
> I was thinking you could start by showing me how to set it up through Github CLI (gh) including a step-by-step to avoid mistakes and errors, if so I would let you know in which step I struggle, please start with Github CLI with every single detail you think I should know. Remember that the set up needs to be functional using macOS and Linux Debian operation systems.

> [!author] Gemini
> Master Guide: Dual-Account GitHub CLI (`gh`) Setup for macOS & Debian
> 
> This setup isolates your configuration files into separate directories. It allows you to switch your terminal's context instantly, preventing authentication errors across both operating systems.
> 
> ---
> 
> Step 1: Install the GitHub CLI
> 
> Open your terminal and install the CLI based on the OS you are currently using.
> 
> - **On macOS:**
>     
>     bash
>     
>     ```
>     brew install gh
>     ```
>     
>     Use code with caution.
>     
> - **On Linux (Debian):**
>     
>     bash
>     
>     ```
>     sudo apt update && sudo apt install gh -y
>     ```
>     
>     Use code with caution.
>     
> 
> ---
> 
> Step 2: Prepare the Work Directories
> 
> By default, the CLI saves everything to `~/.config/gh`. To keep your work account separate, you need a dedicated directory for it.
> 
> Run this command on your terminal (works on both Mac and Debian):
> 
> bash
> 
> ```
> mkdir -p ~/.config/gh-work
> ```
> 
> Use code with caution.
> 
> ---
> 
> Step 3: Authenticate the Personal Account (Default)
> 
> First, log into your personal GitHub account. Because we are not changing any environment variables yet, this will save directly to your default `~/.config/gh` directory.
> 
> 1. Run the login command:
>     
>     bash
>     
>     ```
>     gh auth login
>     ```
>     
>     Use code with caution.
>     
> 2. **Interactive Prompts (Select exactly these options):**
>     - _What account do you want to log into?_ → **GitHub.com**
>     - _What is your preferred protocol for Git operations on this host?_ → **HTTPS**
>     - _Authenticate Git with your GitHub credentials?_ → **Yes**
>     - _How would you like to authenticate GitHub CLI?_ → **Login with a web browser**
> 3. **The Browser Step:**
>     - The terminal will give you an 8-character **one-time code** (e.g., `XXXX-XXXX`).
>     - Press **Enter** to automatically open your default browser.
>     - Make sure you are logged into your **Personal** GitHub account in that browser.
>     - Paste the code and click **Authorize github**.
> 
> ---
> 
> Step 4: Authenticate the Work Account
> 
> To log into your work account without wiping out your personal login, you must explicitly force the CLI to point to your new `gh-work` folder.
> 
> 4. **Redirect the CLI configuration path:**
>     
>     bash
>     
>     ```
>     export GH_CONFIG_DIR=~/.config/gh-work
>     ```
>     
>     Use code with caution.
>     
>     _(Note: This flag only stays active in your current terminal window. If you close the window, it resets)._
> 5. Run the login command again:
>     
>     bash
>     
>     ```
>     gh auth login
>     ```
>     
>     Use code with caution.
>     
> 6. **Interactive Prompts (Select exactly these options):**
>     - _What account do you want to log into?_ → **GitHub.com**
>     - _What is your preferred protocol for Git operations on this host?_ → **HTTPS**
>     - _Authenticate Git with your GitHub credentials?_ → **Yes**
>     - _How would you like to authenticate GitHub CLI?_ → **Login with a web browser**
> 7. **The Browser Step:**
>     - **Crucial:** Before entering the new code, go to github.com in your browser, **log out of your personal account**, and **log into your work account**.
>     - Copy the new 8-character code from your terminal.
>     - Paste it into the browser and click **Authorize github**.
> 
> ---
> 
> Step 5: Automate the Switcher (Cross-Platform Shell Setup)
> 
> To avoid typing the long `export` commands manually every time you change projects, you will create automated terminal shortcuts (aliases).
> 
> 8. Open your Shell Profile in Neovim
> 
> - **On macOS** (Default shell is Zsh):
>     
>     bash
>     
>     ```
>     nvim ~/.zshrc
>     ```
>     
>     Use code with caution.
>     
> - **On Debian** (Default shell is usually Bash):
>     
>     bash
>     
>     ```
>     nvim ~/.bashrc
>     ```
>     
>     Use code with caution.
>     
> 
> 2. Add the Code
> 
> Press `i` to enter Insert Mode in Neovim, scroll to the very bottom of the file, and paste these exact lines:
> 
> bash
> 
> ```
> # GitHub CLI Account Shortcuts
> alias gh-personal="unset GH_CONFIG_DIR; echo '🔄 Switched to Personal GitHub'"
> alias gh-work="export GH_CONFIG_DIR=~/.config/gh-work; echo '🔄 Switched to Work GitHub'"
> ```
> 
> Use code with caution.
> 
> 3. Save and Exit
> 
> Press `Esc`, type `:wq`, and press `Enter`.
> 
> 4. Reload Your Terminal
> 
> To make the shortcuts work immediately without restarting your computer, run:
> 
> - **macOS:** `source ~/.zshrc`
> - **Debian:** `source ~/.bashrc`
> 
> ---
> 
> Step 6: Verify and Test the Setup
> 
> Open a fresh terminal window and test your switcher to ensure no errors occur.
> 
> 1. Check your default identity (it should be personal):
>     
>     bash
>     
>     ```
>     gh auth status
>     ```
>     
>     Use code with caution.
>     
> 2. Switch to your work account:
>     
>     bash
>     
>     ```
>     gh-work
>     ```
>     
>     Use code with caution.
>     
> 3. Check the identity again (it should now display your work username):
>     
>     bash
>     
>     ```
>     gh auth status
>     ```
>     
>     Use code with caution.
>     
> 4. Switch back to your personal account:
>     
>     bash
>     
>     ```
>     gh-personal
>     ```
>     
>     Use code with caution.
>     
> 
> ---
> 
> How to use this workflow daily
> 
> When cloning repositories using the GitHub CLI, you no longer need to worry about manually altering URLs. The CLI handles everything natively based on your active account flag.
> 
> - **To work on a Personal repo:**
>     
>     bash
>     
>     ```
>     cd ~/Projects/personal/
>     gh-personal
>     gh repo clone username/repo-name
>     ```
>     
>     Use code with caution.
>     
> - **To work on a Work repo:**
>     
>     bash
>     
>     ```
>     cd ~/Projects/work/
>     gh-work
>     gh repo clone organization/repo-name
>     ```
>     
>     Use code with caution.
>     
> 
> Please attempt these configuration steps on either your Mac or Debian machine. Let me know **which step number you are currently on**, or if you hit any unexpected terminal outputs along the way!

---
