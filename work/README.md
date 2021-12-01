This repo contains the .devcontainer definition for a vscode container with my preferred env for work.

Based off: https://github.com/microsoft/vscode-dev-containers/tree/main/containers/typescript-node

Highlights:
- Debian
- Node / Typescript / nvm
- Docker CE CLI and Docker Compose (connects to dockerd on host)
- GitHub CLI
- AWS CLI v2, AWS CDK
- Python 3, pip
- zsh, Oh My Zsh!, powerlevel10k (can add your own .p10k.zsh config file, or use mine)
- localstack


# To use:

1. Copy the .devcontainer folder into the root of the directory you want to use in the dev container

2. Inside devcontainer.json, set the variables
- GITHUB_TOKEN
- GIT_USER
- GIT_EMAIL

3. Open vscode and have the dev container extension installed

4. Build container (Ctrl + Shift + p  -> "Rebuild and Reopen in Container")
