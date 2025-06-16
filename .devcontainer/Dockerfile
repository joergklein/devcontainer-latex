# TeX Live base image
FROM mcr.microsoft.com/devcontainers/base:latest

RUN apt-get update && apt-get install -y --no-install-recommends \
    inkscape \
    libnsl2 \
    && rm -rf /var/lib/apt/lists/*

groupadd docker
sudo usermod -aG docker $USER

LABEL maintainer="Jörg Klein <kwp.klein@gmail.com>"

# Install texlive
COPY texlive.profile .
RUN wget http://ctan.net/systems/texlive/tlnet/install-tl-unx.tar.gz
RUN tar xvzf install-tl-unx.tar.gz
RUN ./install-tl-*/install-tl -profile texlive.profile -repository http://ctan.net/systems/texlive/tlnet

# Add texlive to path
ENV PATH="/usr/local/texlive/2025/bin/x86_64-linux:$PATH"
ENV PATH="/usr/local/texlive/2025/bin/aarch64-linux:$PATH"
ENV PATH="/usr/local/texlive/2025/bin/armhf-linux:$PATH"

# Update tlmgr + tex-packages
RUN tlmgr update --self --all --reinstall-forcibly-removed
RUN tlmgr repository add https://mirror.ctan.org/systems/texlive/tlcontrib tlcontrib \
    && tlmgr pinning add tlcontrib "*" \
    && tlmgr install notocondensed \
    && curl -fsSL https://www.preining.info/rsa.asc | tlmgr key add -
RUN luaotfload-tool -v -vvv -u

ENV HOME /home/workspace
WORKDIR /workspace
VOLUME ["/workspace"]

CMD ["zch"]
