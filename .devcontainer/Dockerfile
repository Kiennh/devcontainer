# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.209.6/containers/java/.devcontainer/base.Dockerfile

# [Choice] Java version (use -bullseye variants on local arm64/Apple Silicon): 17, 17-bullseye, 17-buster
ARG VARIANT="17"
FROM mcr.microsoft.com/vscode/devcontainers/java:0-${VARIANT}
ARG GO_VERSION="1.20.4" # You can modify this version as needed

ENV GOPATH=/workspaces/go
ENV PATH=$GOPATH/bin:/usr/local/go/bin:$PATH

# [Option] Install Maven
ARG INSTALL_MAVEN="false"
ARG MAVEN_VERSION=""
# [Option] Install Gradle
ARG INSTALL_GRADLE="false"
ARG GRADLE_VERSION=""
RUN if [ "${INSTALL_MAVEN}" = "true" ]; then su vscode -c "umask 0002 && . /usr/local/sdkman/bin/sdkman-init.sh && sdk install maven \"${MAVEN_VERSION}\""; fi \
    && if [ "${INSTALL_GRADLE}" = "true" ]; then su vscode -c "umask 0002 && . /usr/local/sdkman/bin/sdkman-init.sh && sdk install gradle \"${GRADLE_VERSION}\""; fi

ENV SDKMAN_DIR="/usr/local/sdkman"
ENV PATH=${SDKMAN_DIR}/bin:${SDKMAN_DIR}/candidates/maven/current/bin:${PATH}
COPY library-scripts/maven-debian.sh /tmp/library-scripts/
RUN mkdir -p mkdir -p /usr/local/sdkman/candidates/java/ && ln -s /usr/lib/jvm/msopenjdk-current /usr/local/sdkman/candidates/java/current
ENV JAVA_HOME=/usr/lib/jvm/msopenjdk-current
# [Choice] Node.js version: none, lts/*, 16, 14, 12, 10
ARG NODE_VERSION="18"
RUN if [ "${NODE_VERSION}" != "none" ]; then su vscode -c "umask 0002 && . /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} 2>&1"; fi \
    && apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends vim ffmpeg \
    && apt-get install -y wget php telnet iputils-ping default-mysql-client \
        php7.4-fpm php7.4-mysql   php7.4-gd    php7.4-curl php7.4-gmp php7.4-json  \
        php7.4-mbstring php7.4-opcache php7.4-readline php7.4-xml php7.4-xmlrpc \
        php7.4-zip    nginx    php-igbinary php-redis php-mongodb php-soap \
    && wget https://golang.org/dl/go${GO_VERSION}.linux-arm64.tar.gz \
    && tar -C /usr/local -xzf go${GO_VERSION}.linux-arm64.tar.gz \
    && rm go${GO_VERSION}.linux-arm64.tar.gz \
    && ln -s /usr/local/go/bin/go /usr/local/bin/go \
    && ln -s /usr/local/go/bin/gofmt /usr/local/bin/gofmt \
    && apt-get clean \
    && mkdir -p /workspaces/go/src/github.com \
    && chown -R vscode.vscode /workspaces/go \
    && bash /tmp/library-scripts/maven-debian.sh "latest" "${SDKMAN_DIR}" \
    && npm install -g generator-jhipster

RUN sudo mkdir -p /var/run/php  &&  sudo chown -R vscode.vscode /var/log/nginx /run /var/run/php 

# [Optional] Uncomment this line to install global node packages.
# RUN su vscode -c "source /usr/local/share/nvm/nvm.sh && npm install -g <your-package-here>" 2>&1
