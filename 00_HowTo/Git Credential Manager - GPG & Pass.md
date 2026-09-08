#type/HowTo/Configure #topic/Git/Git-Credential-Manager #for/Debian 

To configure **Git Credential Manager (GCM)** to use **GPG/pass** as its credential store along with `gpg-agent`, you must ==explicitly route GCM's backend storage to GPG and establish a proper terminal pathway for the GPG agent==.

Follow these sequential steps to set up and configure your environment:

Initialize your GPG Key and Pass Store

If you haven't already, you need a GPG key pair and a password store initialized via `pass`. 

**Generate a GPG key** (if you don't have one):
    
```bash
gpg --full-generate-key
```
    
- **Find your Key ID**:
    
    ```
    gpg --list-secret-keys --keyid-format LONG
    ```
    
    _(Look for the line starting with `sec`. The string after the `/` is your Key ID, e.g., `3AA5C34371567BD2`)_.
- **Initialize `pass`** with your Key ID:
    
    ```
    pass init <YOUR_KEY_ID>
    ```

2. Configure Git Credential Manager

Tell GCM to route all token and credential management through your `pass` database instead of the system's default GUI keychain.

Execute the following commands in your terminal:

```
# Set Git Credential Manager as the default helper
git config --global credential.helper manager

# Set the credential store backend to GPG (pass)
git config --global credential.credentialStore gpg
```

_Alternatively, you can achieve this by adding `export GCM_CREDENTIAL_STORE=gpg` to your shell profile._ 

3. Configure gpg-agent and Pinentry

Because GCM often triggers authentication from headless or non-GUI terminal sessions, `gpg-agent` needs to know how to ask you for your GPG passphrase inside the terminal. 

- **Install a TTY pinentry program** (e.g., on Debian/Ubuntu):
    
    ```
    sudo apt install pinentry-tty
    ```
    
- **Direct gpg-agent to use it** by editing your `~/.gnupg/gpg-agent.conf` file:
    
    ```
    pinentry-program /usr/bin/pinentry-tty
    ```
    
- **Reload the agent** to apply the configuration:
    
    ```
    gpg-connect-agent reloadagent /bye
    ```

4. Update Your Shell Profile Environment

GPG requires explicit mapping to the active terminal window to securely render the prompt. Add the following lines to your shell profile (`~/.bashrc`, `~/.zshrc`, or `~/.profile`):

```
# Export the current TTY session to GPG
export GPG_TTY=$(tty)
```

Apply the changes to your active terminal window by running `source ~/.bashrc` (or your respective profile).

How it looks in practice

The next time you perform a `git push` or `git fetch` requiring HTTP authentication, GCM will spin up your browser or device authentication flow. Once authenticated, GCM will invoke `gpg` behind the scenes to encrypt your OAuth token and save it seamlessly inside your `~/.password-store/` hierarchy.

Are you setting this up on a **headless server/SSH session**, or a **local Linux desktop**? If you run into issues prompting for the password, letting me know your **Linux distribution** can help me pinpoint the exact package names or paths.