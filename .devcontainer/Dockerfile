# TeX Live Image
# syntax=docker/dockerfile:1
FROM mcr.microsoft.com/devcontainers/base:latest

RUN apt-get update && apt-get install -y --no-install-recommends \
    apt-utils \
    inkscape \
    libnsl2 \
    libyaml-tiny-perl

RUN apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

# Install texlive
COPY texlive.profile .
RUN wget http://ctan.net/systems/texlive/tlnet/install-tl-unx.tar.gz
RUN tar xvzf install-tl-unx.tar.gz
RUN ./install-tl-*/install-tl -profile texlive.profile -repository http://ctan.net/systems/texlive/tlnet

# Add texlive to path
ENV PATH="/usr/local/texlive/2025/bin/x86_64-linux:$PATH"
ENV MANPATH="/usr/local/texlive/2025/texmf-dist/doc/man:$MANPATH"
ENV INFOPATH="/usr/local/texlive/2025/texmf-dist/doc/info:$INFOPATH"

# Update tlmgr
RUN tlmgr update --self --all

# Add additional packages from the contrib repository
RUN tlmgr repository add https://mirror.ctan.org/systems/texlive/tlcontrib tlcontrib \
    && tlmgr pinning add tlcontrib "*" \
    && tlmgr install notocondensed \
    && curl -fsSL https://www.preining.info/rsa.asc | tlmgr key add -

RUN luaotfload-tool -v -vvv -u

RUN useradd -ms /bin/bash coder
USER coder
WORKDIR /home/coder

CMD ["bash"]
