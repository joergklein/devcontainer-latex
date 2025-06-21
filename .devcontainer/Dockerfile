# TeX Live Image
FROM mcr.microsoft.com/devcontainers/base:latest

RUN apt-get update && apt-get install -y --no-install-recommends \
    inkscape \
    libnsl2 \
    sudo \
    && rm -rf /var/lib/apt/lists/*

# Install cpanm & missing latexindent dependencies
# RUN sudo curl -L http://cpanmin.us | perl - --self-upgrade \
#     && cpanm Log::Dispatch::File YAML::Tiny File::HomeDir

ARG USER=coder
RUN useradd --groups sudo --no-create-home --shell /bin/bash ${USER} \
    && echo "${USER} ALL=(ALL) NOPASSWD:ALL" >/etc/sudoers.d/${USER} \
    && chmod 0440 /etc/sudoers.d/${USER}

# Install texlive
COPY texlive.profile .
RUN wget http://ctan.net/systems/texlive/tlnet/install-tl-unx.tar.gz
RUN tar xvzf install-tl-unx.tar.gz
RUN ./install-tl-*/install-tl -profile texlive.profile -repository http://ctan.net/systems/texlive/tlnet

# Add texlive to path
ENV PATH="/usr/local/texlive/2025/bin/x86_64-linux:$PATH"
ENV MANPATH="/usr/local/texlive/2025/texmf-dist/doc/man:$MANPATH"
ENV INFOPATH="/usr/local/texlive/2025/texmf-dist/doc/info:$INFOPATH"

# # Update tlmgr
RUN sudo tlmgr update --self --all

# Add additional packages from the contrib repository
RUN tlmgr repository add https://mirror.ctan.org/systems/texlive/tlcontrib tlcontrib \
    && tlmgr pinning add tlcontrib "*" \
    && tlmgr install notocondensed \
    && sudo curl -fsSL https://www.preining.info/rsa.asc | tlmgr key add -

RUN luaotfload-tool -v -vvv -u

# USER ${USER}
# WORKDIR /home/${USER}

USER coder
WORKDIR /home/coder/workspace

CMD ["bash"]
