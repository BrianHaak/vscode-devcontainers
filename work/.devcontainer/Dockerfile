ARG VARIANT
FROM mcr.microsoft.com/vscode/devcontainers/typescript-node:${VARIANT}

# Install tools
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends unzip python3-pip

ARG EXTRA_NODE_VERSION
RUN su node -c "source /usr/local/share/nvm/nvm.sh && nvm install ${EXTRA_NODE_VERSION}"

# Install Docker CE CLI
RUN apt-get update && apt-get upgrade -y \
    && apt-get install -y apt-transport-https ca-certificates curl gnupg2 lsb-release \
    && curl -fsSL https://download.docker.com/linux/$(lsb_release -is | tr '[:upper:]' '[:lower:]')/gpg | apt-key add - 2>/dev/null \
    && echo "deb [arch=amd64] https://download.docker.com/linux/$(lsb_release -is | tr '[:upper:]' '[:lower:]') $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list \
    && apt-get update \
    && apt-get install -y docker-ce-cli

# Install Docker Compose
RUN export LATEST_COMPOSE_VERSION=$(curl -sSL "https://api.github.com/repos/docker/compose/releases/latest" | grep -o -P '(?<="tag_name": ").+(?=")') \
    && curl -sSL "https://github.com/docker/compose/releases/download/${LATEST_COMPOSE_VERSION}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose \
    && chmod +x /usr/local/bin/docker-compose

# Install GitHub cli
RUN export LATEST_GH_CLI_VERSION=$(curl -sSL "https://api.github.com/repos/cli/cli/releases/latest" | grep -o -P '(?<="tag_name": "v).+(?=")') \
    && curl -L https://github.com/cli/cli/releases/download/v${LATEST_GH_CLI_VERSION}/gh_${LATEST_GH_CLI_VERSION}_linux_amd64.deb > gh.deb \
    && dpkg -i gh.deb

# Install awscli V2
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" \
    && unzip awscliv2.zip && ./aws/install

# Install global pip and npm modules
RUN pip3 install -U pip setuptools wheel
RUN pip install localstack awscli-local
RUN su node -c "npm install npm@latest -g"
RUN su node -c "npm install -g aws-cdk aws-cdk-local lerna bili typescript ts-node eslint husky jest rollup"

# Install Conda
RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh
RUN bash ~/miniconda.sh -b -p $HOME/miniconda
# source ~/miniconda/bin/activate
# conda init zsh
# conda env create -f environment.yml

# Configure shell
ARG GIT_USER
ARG GIT_EMAIL
RUN git config --global user.email "$GIT_EMAIL" && git config --global user.name "$GIT_USER"
RUN chsh -s $(which zsh)
RUN git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k
RUN echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >> /root/.zshrc
RUN echo '[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh' >> /root/.zshrc

# Configure terminal aliases
RUN echo "alias git_graph='git log --oneline --abbrev-commit --all --graph'" >> /etc/zsh/zshrc
RUN echo "alias gh_pr='f() { cb=\$(git symbolic-ref --short HEAD) && git push && gh pr create -d -B \$1 -t \"\$1 <- \$cb\" -b \"\"; };f'" >> /etc/zsh/zshrc
RUN echo "alias b64enc='openssl base64 -e -A <<<'" >> /etc/zsh/zshrc
RUN echo "alias b64dec='openssl base64 -d -A <<<'" >> /etc/zsh/zshrc
