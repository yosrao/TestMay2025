ARG VARIANT=22-bookworm
FROM node:${VARIANT}

ARG USERNAME=node
ARG NPM_GLOBAL=/usr/local/share/npm-global

# Add NPM global to PATH.
ENV PATH=${NPM_GLOBAL}/bin:${PATH}

RUN \
    # Configure global npm install location, use group to adapt to UID/GID changes
    if ! cat /etc/group | grep -e "^npm:" > /dev/null 2>&1; then groupadd -r npm; fi \
    && usermod -a -G npm ${USERNAME} \
    && umask 0002 \
    && mkdir -p ${NPM_GLOBAL} \
    && touch /usr/local/etc/npmrc \
    && chown ${USERNAME}:npm ${NPM_GLOBAL} /usr/local/etc/npmrc \
    && chmod g+s ${NPM_GLOBAL} \
    && npm config -g set prefix ${NPM_GLOBAL} \
    && su ${USERNAME} -c "npm config -g set prefix ${NPM_GLOBAL}" \
    # Install eslint
    && su ${USERNAME} -c "umask 0002 && npm install -g eslint" \
    && npm cache clean --force > /dev/null 2>&1

# [Optional] Uncomment this section to install additional OS packages.
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>

# [Optional] Uncomment if you want to install an additional version of node using nvm
# ARG EXTRA_NODE_VERSION=10
# RUN su node -c "source /usr/local/share/nvm/nvm.sh && nvm install ${EXTRA_NODE_VERSION}"

# [Optional] Uncomment if you want to install more global node modules
# RUN su node -c "npm install -g <your-package-list-here>"


image: gitpod/workspace-nodejs:latest

tasks:
  - name: "Install PHP 8.4 and extensions"
    before: |
      # Install necessary packages for apt-get (if not already present)
      sudo apt-get update && sudo apt-get install ca-certificates apt-transport-https software-properties-common
    command: |
      # Add the Ondřej Surý PPA for PHP
      sudo add-apt-repository ppa:ondrej/php
      sudo apt-get update
      # Install PHP 8.4 and any needed extensions
      sudo apt-get install -y php8.4 php8.4-cli php8.4-mbstring php8.4-curl php8.4-json # Add more extensions as needed
      # Install Node.js dependencies (if needed)
      npm install
      echo "PHP 8.4 installed!"
ports:
  - port: 3000
    onOpen: open-preview