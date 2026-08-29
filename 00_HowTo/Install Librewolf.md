#type/HowTo #topic/Librewolf/Install-Uninstall #for/all 

Source: [Installation instructions for Debian based](https://www.librewolf.net/installation/debian/)

# Installation instructions for  ![Debian based Logo|20](https://librewolf.net/icons/debian.svg) Debian based

We have a repository for Debian-based distributions (Debian, Ubuntu, Mint, etc.), with which you can easily install and update LibreWolf. To add it to your system and install LibreWolf, run the following commands one by one:

```bash
sudo apt update && sudo apt install extrepo -y

sudo extrepo enable librewolf && sudo extrepo update librewolf

sudo apt update && sudo apt install librewolf -y
```

# Removing the repositories from your system

To remove the LibreWolf repository from your system, run:

```bash
sudo extrepo disable librewolf
```

If you added a LibreWolf repository without Extrepo, you can remove it with the following command:

```bash
sudo apt purge librewolf -y
sudo apt autoremove -y
sudo rm -f \
    /etc/apt/sources.list.d/extrepo_librewolf.sources \
    /var/lib/extrepo/keys/librewolf.asc \
    /var/lib/dpkg/info/librewolf.list \
    /var/lib/dpkg/info/librewolf.md5sums \
    /var/lib/apt/lists/repo.librewolf.net_dists_librewolf_InRelease \
    /var/lib/apt/lists/repo.librewolf.net_dists_librewolf_main_binary-amd64_Packages \
    /var/lib/apt/lists/repo.librewolf.net_dists_librewolf_main_binary-arm64_Packages \
    /usr/share/librewolf \ /usr/share/applications/librewolf.desktop
```

---

<iframe src="https://www.librewolf.net/installation/debian/" title="Installation instructions for Debian based" width="100%" height="800px" scrolling="no" frameborder="no" allow="fullscreen"></iframe>
