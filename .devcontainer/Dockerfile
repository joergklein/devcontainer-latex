# TeX Live Image
FROM mcr.microsoft.com/devcontainers/base:latest

RUN apt-get update && apt-get install -y --no-install-recommends \
    inkscape \
    libnsl2 \
    sudo \
    && rm -rf /var/lib/apt/lists/*

# ARG USER=coder
# RUN useradd --groups sudo --no-create-home --shell /bin/bash ${USER} \
#     && echo "${USER} ALL=(ALL) NOPASSWD:ALL" >/etc/sudoers.d/${USER} \
#     && chmod 0440 /etc/sudoers.d/${USER}

ARG USERNAME=latex
ARG USER_UID=1001
ARG USER_GID=1000

RUN if [ ${USERNAME} != "latex" ] && [ ${USERNAME} != "root" ]; \
    then useradd --uid $USER_UID --gid $USER_GID -m $USERNAME -s $(which zsh) \
    && echo $USERNAME ALL=(ALL) NOPASSWD:ALL >/etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/${USERNAME}
    else echo "No new user created"; fi

RUN if [ ${USERNAME} != "latex" ] && [ ${USERNAME} != "root" ]; \
    then mkdir -p /home/${USERNAME}/.vscode-server/extensions \
    && mkdir -p /root/.vscode-server/extensions \
    && chown -R ${USERNAME} /home/${USERNAME}/.vscode-server; fi

RUN if [ ${USERNAME} != "latex" ] && [ ${USERNAME} != "root" ]; \
    then SNIPPET="export PROMPT_COMMAND='history -a' && export HISTFILE=/commandhistory/.bash_history" \
    && mkdir -p /commandhistory \
    && touch /commandhistory/.bash_history \
    && chown -R $USERNAME /commandhistory \
    && echo $SNIPPET >> "/home/${USERNAME}/.bashrc" \
    && echo $SNIPPET >> "/root/.bashrc"; fi

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

USER latex
WORKDIR /home/coder/workspace
