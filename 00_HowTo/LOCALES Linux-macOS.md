#type/linux-macos #topic/timezone-locale-variables #for/all 
> [!human] You
> If I want to set my region here in México but I want to set the language in English, could be that possible? Or do you recommend that I stay consistent?

> [!ai] Claude
> Locale environment variable
> ```bash
> export LANG="en_US.UTF-8"
> export LC_ALL="en_US.UTF-8"
> export LC_TIME="es_MX.UTF-8"
> export LC_MONETARY="es_MX.UTF-8"
> export LC_NUMERIC="es_MX.UTF-8"
> ```
