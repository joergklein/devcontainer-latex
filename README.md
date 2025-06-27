## Latex development container for VS-Code Desktop/Code-Server in Coder

Coder is an open-source platform for creating and managing cloud development environments on your infrastructure. Further information can be found here: https://coder.com.

## Help needed

- Please take a look at the Dockerfile and docker-compose.yaml. Are they correct?
- In the devcontainer.yaml I want to install the extensions and settings per default. Unfortunately, the extensions and settings are not installed in the Code-Server. What is wrong?

## Quickstart

Please read first my [Coder installation guide](install.md) in this repository.

1. Visit https://github.com/joergklein/devcontainer-latex
1. Login in coder
1. Use the template `Docker (Devcontainer)`
1. Use this repo https://github.com/joergklein/devcontainer-latex.git

## TeX Live

[TeX Live](https://www.tug.org/texlive) is the most comprehensive TeX distribution available. The distribution based on the [TeX Directory Structure](https://tug.org/tds/tds.html) is compiled by the [TeX Users Group](https://de.wikipedia.org/wiki/TeX_Users_Group) in cooperation with several local TeX user groups, such as the [Deutschsprachige Anwendervereinigung TeX (DANTE)](https://www.dante.de).

### Select a scheme

The recommendation is: **install scheme-full**

Change the first line in the `texlive.profile` file, in which the `scheme-full` option is set by default.

```sh
selected_scheme scheme-full
```

Further information can be found here:

```sh
tlmgr info schemes
```
```text
i scheme-basic: basic scheme (plain and latex)
i scheme-bookpub: book publishing scheme (core LaTeX and add-ons)
i scheme-context: ConTeXt scheme
i scheme-full: full scheme (everything)
i scheme-gust: GUST TeX Live scheme
i scheme-infraonly: infrastructure-only scheme (no TeX at all)
i scheme-medium: medium scheme (small + more packages and languages)
i scheme-minimal: minimal scheme (plain only)
i scheme-small: small scheme (basic + xetex, metapost, a few languages)
i scheme-tetex: teTeX scheme (more than medium, but nowhere near full)
```

For more information for example for the scheme-full type:

```sh
tlmgr info --list scheme-full
````

Output:

```sh
package:     scheme-full
category:    Scheme
shortdesc:   full scheme (everything)
longdesc:    This is the full TeX Live scheme: it installs everything available.
installed:   Yes
revision:    54074
sizes:       8858545k
relocatable: Yes
depends:
        collection-basic
        collection-bibtexextra
        collection-binextra
        collection-context
        collection-fontsextra
        collection-fontsrecommended
        collection-fontutils
        collection-formatsextra
        collection-games
        collection-humanities
        collection-langarabic
        collection-langchinese
        collection-langcjk
        collection-langcyrillic
        collection-langczechslovak
        collection-langenglish
        collection-langeuropean
        collection-langfrench
        collection-langgerman
        collection-langgreek
        collection-langitalian
        collection-langjapanese
        collection-langkorean
        collection-langother
        collection-langpolish
        collection-langportuguese
        collection-langspanish
        collection-latex
        collection-latexextra
        collection-latexrecommended
        collection-luatex
        collection-mathscience
        collection-metapost
        collection-music
        collection-pictures
        collection-plaingeneric
        collection-pstricks
        collection-publishers
        collection-texworks
        collection-xetex
Included files, by type:
```
