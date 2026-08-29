#type/HowTo #topic/nb/installation #for/all 
# Installation 
## Dependencies
### Required
- [Bash](https://en.wikipedia.org/wiki/Bash_\(Unix_shell\))
    - `nb` works perfectly with Zsh, fish, and any other shell set as your primary login shell, the system just needs to have Bash available on it.
- [Git](https://git-scm.com/)
- A text editor with command line support, such as: 
    - [Vim](https://en.wikipedia.org/wiki/Vim_/\(text_editor/\)),
    - [Emacs](https://en.wikipedia.org/wiki/Emacs),
    - [Visual Studio Code](https://code.visualstudio.com/),
    - [Sublime Text](https://www.sublimetext.com/),
    - [Helix](https://helix-editor.com/),
    - [micro](https://github.com/zyedidia/micro),
    - [nano](https://en.wikipedia.org/wiki/GNU_nano),
    - [Atom](https://atom.io/),
    - [TextMate](https://macromates.com/),
    - [MacDown](https://macdown.uranusjr.com/),
    - [some of these](https://github.com/topics/text-editor),
    - [and many of these.](https://en.wikipedia.org/wiki/List_of_text_editors)
### Optional (but recommended)
`nb` leverages standard command line tools and works in standard Linux / Unix environments. `nb`also checks the environment for some additional optional tools and uses them to enhance the experience whenever they are available.

Recommended:
- [`bat`](https://github.com/sharkdp/bat)
- [`ncat`](https://nmap.org/ncat/) or [`socat`](https://www.kali.org/tools/socat/)
- [`pandoc`](https://pandoc.org/)
- [`rg`](https://github.com/BurntSushi/ripgrep)
- [`tig`](https://github.com/jonas/tig)
- [`w3m`](https://en.wikipedia.org/wiki/W3m)

Also supported for various enhancements:
[Ack](https://beyondgrep.com/), [`afplay`](https://ss64.com/osx/afplay.html), [`asciidoctor`](https://asciidoctor.org/), [The Silver Searcher (`ag`)](https://github.com/ggreer/the_silver_searcher), [`catimg`](https://github.com/posva/catimg), [Chafa](https://github.com/hpjansson/chafa), [Chromium](https://www.chromium.org/) / [Chrome](https://www.google.com/chrome/), [`eza`](https://github.com/eza-community/eza), [`ffplay`](https://ffmpeg.org/ffplay.html), [ImageMagick](https://imagemagick.org/), [`glow`](https://github.com/charmbracelet/glow), [GnuPG](https://en.wikipedia.org/wiki/GNU_Privacy_Guard), [`highlight`](http://www.andre-simon.de/doku/highlight/en/highlight.php), [`imgcat`](https://www.iterm2.com/documentation-images.html), [`joshuto`](https://github.com/kamiyaa/joshuto), [kitty’s `icat`kitten](https://sw.kovidgoyal.net/kitty/kittens/icat.html), [`lowdown`](https://kristaps.bsd.lv/lowdown), [`lsd`](https://github.com/lsd-rs/lsd), [Links](https://en.wikipedia.org/wiki/Links_\(web_browser\)), [Lynx](https://en.wikipedia.org/wiki/Lynx_\(web_browser\)), [`mdcat`](https://github.com/swsnr/mdcat), [`mdless`](https://github.com/ttscoff/mdless), [`mdv`](https://github.com/axiros/terminal_markdown_viewer), [Midnight Commander (`mc`)](https://en.wikipedia.org/wiki/Midnight_Commander), [`mpg123`](https://en.wikipedia.org/wiki/Mpg123), [MPlayer](https://en.wikipedia.org/wiki/MPlayer), [`ncat`](https://nmap.org/ncat/), [`netcat`](https://netcat.sourceforge.net/), [note-link-janitor](https://github.com/andymatuschak/note-link-janitor) (via [plugin](https://github.com/xwmx/nb/blob/master/plugins/backlink.nb-plugin)), [`pdftotext`](https://en.wikipedia.org/wiki/Pdftotext), [Pygments](https://pygments.org/), [Ranger](https://ranger.github.io/), [readability-cli](https://gitlab.com/gardenappl/readability-cli), [`rga` / ripgrep-all](https://github.com/phiresky/ripgrep-all), [`sc-im`](https://github.com/andmarti1424/sc-im), [`socat`](https://www.kali.org/tools/socat/), [`termvisage`](https://github.com/AnonymouX47/termvisage), [`termpdf.py`](https://github.com/dsanson/termpdf.py), [Tidy-Viewer (`tv`)](https://github.com/alexhallam/tv), [`timg`](https://github.com/hzeller/timg), [vifm](https://vifm.info/), [`viu`](https://github.com/atanunq/viu), [VisiData](https://www.visidata.org/)
## macOS / Homebrew

```
brew install xwmx/taps/nb
```

Installing nb with Homebrew also installs the recommended dependencies above and completion scripts for Bash, Zsh, and Fish.

Install the latest development version from the repository with:

```bash
brew install xwmx/taps/nb --head
```

nb is also available in homebrew-core. Installing it together with the bash formula is recommended:

```bash
brew install nb bash
```

Ubuntu, Windows, and others