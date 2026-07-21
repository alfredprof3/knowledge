#type/HowTo #topic/clean-installation #for/all 
# Sources list
## Option 3
[Source](https://gist.github.com/sephyran/4a2a9049758fed57c6d311fe620cf976)
```bash
deb http://deb.debian.org/debian/ trixie main non-free-firmware contrib non-free

deb-src http://deb.debian.org/debian/ trixie main non-free-firmware contrib non-free

deb http://security.debian.org/debian-security trixie-security main non-free-firmware contrib non-free

deb-src http://security.debian.org/debian-security trixie-security main non-free-firmware contrib non-free
```
## Option 4
[Source](https://gist.github.com/shiwildy/47c5085ff04bc2cee01d28f4151e96a3)
```bash
deb https://deb.debian.org/debian/ trixie contrib main non-free non-free-firmware
# deb-src https://deb.debian.org/debian/ trixie contrib main non-free non-free-firmware

deb https://deb.debian.org/debian/ trixie-updates contrib main non-free non-free-firmware
# deb-src https://deb.debian.org/debian/ trixie-updates contrib main non-free non-free-firmware

deb https://deb.debian.org/debian/ trixie-proposed-updates contrib main non-free non-free-firmware
# deb-src https://deb.debian.org/debian/ trixie-proposed-updates contrib main non-free non-free-firmware

deb https://deb.debian.org/debian/ trixie-backports contrib main non-free non-free-firmware
# deb-src https://deb.debian.org/debian/ trixie-backports contrib main non-free non-free-firmware

deb https://security.debian.org/debian-security/ trixie-security contrib main non-free non-free-firmware
# deb-src https://security.debian.org/debian-security/ trixie-security contrib main non-free non-free-firmware
```
## Option 5
[Source](https://linuxconfig.org/configuring-apt-sources-list-a-quick-reference-guide-for-debian-systems)

```bash
deb http://deb.debian.org/debian/ trixie main non-free contrib non-free-firmware

deb http://security.debian.org/debian-security trixie-security main non-free contrib non-free-firmware

deb http://deb.debian.org/debian/ trixie-updates main non-free contrib non-free-firmware
```
