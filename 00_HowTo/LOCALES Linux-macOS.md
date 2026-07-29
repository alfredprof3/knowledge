#type/linux-macos #topic/timezone-locale-variables #for/all 
> [!human] You
> If I want to set my region here in México but I want to set the language in English, could be that possible? Or do you recommend that I stay consistent?

> [!ai] Claude
> Locale environment variable
> ```bash
> # Primary user interface language (English)
> export LANG="en_US.UTF-8"
> 
> # Regional Formatting for Mexico
> export LC_NUMERIC="es_MX.UTF-8"
> export LC_TIME="es_MX.UTF-8"
> export LC_MONETARY="es_MX.UTF-8"
> export LC_PAPER="es_MX.UTF-8"
> export LC_NAME="es_MX.UTF-8"
> export LC_ADDRESS="es_MX.UTF-8"
> export LC_TELEPHONE="es_MX.UTF-8"
> export LC_MEASUREMENT="es_MX.UTF-8"
> export LC_IDENTIFICATION="es_MX.UTF-8"
> ```
