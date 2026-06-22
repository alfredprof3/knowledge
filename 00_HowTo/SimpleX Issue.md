#type/HowTo #topic/simplex-issue #for/personal 

> [!author] Prompt
> I installed `simplex-chat` command line tool to send messages via terminal but I'm having an issue. The message prompt says
> 
> ```
> dyld[5296]: Library not loaded: /usr/local/opt/openssl@3.0/lib/libcrypto.3.dylib
>   Referenced from: <69F426E2-9529-3E8B-ACC3-2541B7996FD8> /Users/alfredxuser/.local/bin/simplex-chat
>   Reason: tried: '/usr/local/opt/openssl@3.0/lib/libcrypto.3.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/usr/local/opt/openssl@3.0/lib/libcrypto.3.dylib' (no such file), '/usr/local/opt/openssl@3.0/lib/libcrypto.3.dylib' (no such file)
> [1]    5296 abort      simplex-chat
> ```
> 
> How can I fix it? I give you the repository where I installed the tool https://github.com/simplex-chat/simplex-chat/blob/stable/docs/CLI.md#usage

> [!author] Claude
> Let me fetch the simplex-chat CLI docs to give you accurate guidance.Important note first: this error is happening on **macOS** (not iSH on iPad), since the path shows `/Users/alfredxuser/` — a Mac home directory. The binary was compiled expecting OpenSSL 3.0 at `/usr/local/opt/openssl@3.0/` but that path doesn't exist on your system. Here's how to fix it:
> 
> ---
> 
> ## Fixing the `libcrypto.3.dylib` Error on macOS
> 
> ### Step 1 — Check if Homebrew is installed
> 
> ```sh
> brew --version
> ```
> 
> If not installed, get it first:
> 
> ```sh
> /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
> ```
> 
> ---
> 
> ### Step 2 — Install OpenSSL 3 via Homebrew
> 
> ```sh
> brew install openssl@3
> ```
> 
> ---
> 
> ### Step 3 — Find where Homebrew installed it
> 
> This depends on your Mac chip:
> 
> ```sh
> # Check which one exists on your machine:
> ls /usr/local/opt/openssl@3/lib/      # Intel Mac
> ls /opt/homebrew/opt/openssl@3/lib/   # Apple Silicon (M1/M2/M3)
> ```
> 
> ---
> 
> ### Step 4 — Create the symlink the binary expects
> 
> The binary is hardcoded to look at `/usr/local/opt/openssl@3.0/` (note the `.0`). Create a symlink pointing there:
> 
> **Intel Mac:**
> 
> ```sh
> ln -sf /usr/local/opt/openssl@3 /usr/local/opt/openssl@3.0
> ```
> 
> **Apple Silicon (M1/M2/M3):**
> 
> ```sh
> # First create the directory structure it expects
> sudo mkdir -p /usr/local/opt/openssl@3.0/lib
> 
> # Then symlink the specific library files
> ln -sf /opt/homebrew/opt/openssl@3/lib/libcrypto.3.dylib \
>     /usr/local/opt/openssl@3.0/lib/libcrypto.3.dylib
> 
> ln -sf /opt/homebrew/opt/openssl@3/lib/libssl.3.dylib \
>     /usr/local/opt/openssl@3.0/lib/libssl.3.dylib
> ```
> 
> ---
> 
> ### Step 5 — Test it
> 
> ```sh
> simplex-chat --version
> ```
> 
> ---
> 
> ### Step 6 — If it still fails, use `install_name_tool`
> 
> This directly patches the binary to point to the correct library path:
> 
> ```sh
> # Find the real path of libcrypto
> OPENSSL_LIB=$(brew --prefix openssl@3)/lib/libcrypto.3.dylib
> 
> # Patch the binary
> install_name_tool -change \
>     /usr/local/opt/openssl@3.0/lib/libcrypto.3.dylib \
>     $OPENSSL_LIB \
>     ~/.local/bin/simplex-chat
> ```
> 
> Then run it again:
> 
> ```sh
> simplex-chat --version
> ```
> 
> ---
> 
> ### Step 7 — Allow Gatekeeper (macOS security)
> 
> Since `simplex-chat` is a downloaded binary, macOS may block it. Run:
> 
> ```sh
> xattr -d com.apple.quarantine ~/.local/bin/simplex-chat
> ```
> 
> ---
> 
> ### Quick Summary
> 
> |Root Cause|Fix|
> |---|---|
> |OpenSSL 3.0 not installed|`brew install openssl@3`|
> |Binary expects wrong path|Create symlink or use `install_name_tool`|
> |macOS Gatekeeper blocking binary|`xattr -d com.apple.quarantine`|
> 
> The symlink approach in **Step 4** is usually all you need. Let me know which Mac chip you have (Intel or Apple Silicon) if you run into trouble!

---

> [!author] Prompt
> I'm using a macbook air M1 2020 with a zsh shell. I do it the previous steps but a second message shows up
> 
> ```
> dyld[6438]: Library not loaded: /usr/local/opt/openssl@3.0/lib/libcrypto.3.dylib
>   Referenced from: <69F426E2-9529-3E8B-ACC3-2541B7996FD8> /Users/alfredxuser/.local/bin/simplex-chat
>   Reason: tried: '/usr/local/opt/openssl@3.0/lib/libcrypto.3.dylib' (mach-o file, but is an incompatible architecture (have 'arm64', need 'x86_64')), '/System/Volumes/Preboot/Cryptexes/OS/usr/local/opt/openssl@3.0/lib/libcrypto.3.dylib' (no such file), '/usr/local/opt/openssl@3.0/lib/libcrypto.3.dylib' (mach-o file, but is an incompatible architecture (have 'arm64', need 'x86_64')), '/opt/homebrew/Cellar/openssl@3/3.6.2/lib/libcrypto.3.dylib' (mach-o file, but is an incompatible architecture (have 'arm64', need 'x86_64')), '/System/Volumes/Preboot/Cryptexes/OS/opt/homebrew/Cellar/openssl@3/3.6.2/lib/libcrypto.3.dylib' (no such file), '/opt/homebrew/Cellar/openssl@3/3.6.2/lib/libcrypto.3.dylib' (mach-o file, but is an incompatible architecture (have 'arm64', need 'x86_64'))
> [1]    6438 abort      simplex-chat --version
> ```
> 
> How do I fix it?

> [!author] Claude
> The new error tells us exactly what's wrong — look at this key part:
> 
> > `have 'arm64', need 'x86_64'`
> 
> You downloaded the **Intel (x86_64)** binary, but your M1 Mac needs the **aarch64 (Apple Silicon)** binary. No amount of OpenSSL symlinking will fix an architecture mismatch — you just need the right binary. Here's how to fix it:
> 
> ---
> 
> ## Fix: Download the Correct arm64 Binary
> 
> ### Option 1 — Use the official install script (easiest)
> 
> The install script auto-detects your architecture and downloads the right binary:
> 
> ```sh
> # Remove the wrong binary first
> rm ~/.local/bin/simplex-chat
> 
> # Run the official install script
> curl -o- https://raw.githubusercontent.com/simplex-chat/simplex-chat/stable/install.sh | bash
> ```
> 
> This should automatically grab `simplex-chat-macos-aarch64` for your M1.
> 
> ---
> 
> ### Option 2 — Manually download the aarch64 binary
> 
> ```sh
> # Remove the wrong binary
> rm ~/.local/bin/simplex-chat
> 
> # Download the correct arm64 binary
> curl -L "https://github.com/simplex-chat/simplex-chat/releases/latest/download/simplex-chat-macos-aarch64" \
>   -o ~/.local/bin/simplex-chat
> 
> # Make it executable
> chmod +x ~/.local/bin/simplex-chat
> ```
> 
> ---
> 
> ### Step 2 — Remove the Gatekeeper quarantine flag
> 
> macOS will block any downloaded binary. Run this after installing:
> 
> ```sh
> xattr -d com.apple.quarantine ~/.local/bin/simplex-chat
> ```
> 
> ---
> 
> ### Step 3 — Verify it works
> 
> ```sh
> simplex-chat --version
> ```
> 
> ---
> 
> ### Step 4 — Confirm the architecture is now correct
> 
> ```sh
> file ~/.local/bin/simplex-chat
> ```
> 
> You should see `arm64` in the output, like:
> 
> ```
> simplex-chat: Mach-O 64-bit executable arm64
> ```
> 
> ---
> 
> ### Why did this happen?
> 
> The simplex-chat releases page offers separate binaries for Mac: `x86_64` for Intel Macs and `aarch64` for Apple Silicon. The install script you originally ran likely grabbed the Intel one by default, or the binary was copied from somewhere that only had the x86 version. The aarch64 binary has its own OpenSSL bundled correctly for arm64, so no symlink workarounds are needed at all. Let me know if it works!

---