# syntax=docker/dockerfile:1
# MEGALINTER FLAVOR [formatters]: Contains only formatters
###########################################
###########################################
## Dockerfile to run MegaLinter ##
###########################################
###########################################

# @not-generated

#############################################################################################
## @generated by .automation/build.py using descriptor files, please do not update manually ##
#############################################################################################
#FROM__START
FROM mvdan/shfmt:latest-alpine as shfmt
FROM alpine/terragrunt:latest as terragrunt
#FROM__END

##################
# Get base image #
##################
FROM python:3.12.1-alpine3.19
ARG GITHUB_TOKEN

#############################################################################################
## @generated by .automation/build.py using descriptor files, please do not update manually ##
#############################################################################################
#ARG__START
ARG PSSA_VERSION='latest'
#ARG__END

####################
# Run APK installs #
####################

WORKDIR /

#############################################################################################
## @generated by .automation/build.py using descriptor files, please do not update manually ##
#############################################################################################
#APK__START
RUN apk add --no-cache \
                bash \
                ca-certificates \
                curl \
                gcc \
                git \
                git-lfs \
                libffi-dev \
                make \
                musl-dev \
                openssh \
                dotnet7-sdk \
                icu-libs \
                npm \
                nodejs-current \
                yarn \
    && git config --global core.autocrlf true
#APK__END

# PATH for golang & python
ENV GOROOT=/usr/lib/go \
    GOPATH=/go
    # PYTHONPYCACHEPREFIX="$HOME/.cache/cpython/" NV: not working for all packages :/
# hadolint ignore=DL3044
ENV PATH="$PATH":"$GOROOT"/bin:"$GOPATH"/bin
RUN mkdir -p ${GOPATH}/src ${GOPATH}/bin || true && \
    # Ignore npm package issues
    yarn config set ignore-engines true || true

##############################
# Installs rust dependencies #
#############################################################################################
## @generated by .automation/build.py using descriptor files, please do not update manually ##
#############################################################################################

#CARGO__START
RUN curl https://sh.rustup.rs -sSf | sh -s -- -y --profile minimal --default-toolchain stable \
    && export PATH="/root/.cargo/bin:${PATH}" \
    && cargo install --force --locked sarif-fmt \
    && rm -rf /root/.cargo/registry /root/.cargo/git /root/.cache/sccache /root/.rustup
ENV PATH="/root/.cargo/bin:${PATH}"
#CARGO__END

################################
# Installs python dependencies #
#############################################################################################
## @generated by .automation/build.py using descriptor files, please do not update manually ##
#############################################################################################

#PIPVENV__START
RUN PYTHONDONTWRITEBYTECODE=1 pip3 install --no-cache-dir --upgrade pip virtualenv \
    && mkdir -p "/venvs/black" && cd "/venvs/black" && virtualenv . && source bin/activate && PYTHONDONTWRITEBYTECODE=1 pip3 install --no-cache-dir black && deactivate && cd ./../.. \
    && mkdir -p "/venvs/isort" && cd "/venvs/isort" && virtualenv . && source bin/activate && PYTHONDONTWRITEBYTECODE=1 pip3 install --no-cache-dir isort black && deactivate && cd ./../.. \
    && mkdir -p "/venvs/rstfmt" && cd "/venvs/rstfmt" && virtualenv . && source bin/activate && PYTHONDONTWRITEBYTECODE=1 pip3 install --no-cache-dir rstfmt && deactivate && cd ./../.. \
    && mkdir -p "/venvs/snakefmt" && cd "/venvs/snakefmt" && virtualenv . && source bin/activate && PYTHONDONTWRITEBYTECODE=1 pip3 install --no-cache-dir snakefmt && deactivate && cd ./../..  \
    && find . | grep -E "(/__pycache__$|\.pyc$|\.pyo$)" | xargs rm -rf && rm -rf /root/.cache
ENV PATH="${PATH}":/venvs/black/bin:/venvs/isort/bin:/venvs/rstfmt/bin:/venvs/snakefmt/bin
#PIPVENV__END

############################
# Install NPM dependencies #
#############################################################################################
## @generated by .automation/build.py using descriptor files, please do not update manually ##
#############################################################################################

ENV NODE_OPTIONS="--max-old-space-size=8192" \
    NODE_ENV=production
#NPM__START
WORKDIR /node-deps
RUN npm --no-cache install --ignore-scripts --omit=dev \
                typescript \
                prettier \
                markdownlint-cli \
                markdown-table-formatter  && \
    echo "Cleaning npm cache…" \
    && npm cache clean --force || true \
    && echo "Changing owner of node_modules files…" \
    && chown -R "$(id -u)":"$(id -g)" node_modules # fix for https://github.com/npm/cli/issues/5900 \
    && echo "Removing extra node_module files…" \
    && rm -rf /root/.npm/_cacache \
    && find . -name "*.d.ts" -delete \
    && find . -name "*.map" -delete \
    && find . -name "*.npmignore" -delete \
    && find . -name "*.travis.yml" -delete \
    && find . -name "CHANGELOG.md" -delete \
    && find . -name "README.md" -delete \
    && find . -name ".package-lock.json" -delete \
    && find . -name "package-lock.json" -delete \
    && find . -name "README.md" -delete
WORKDIR /

#NPM__END

# Add node packages to path #
ENV PATH="/node-deps/node_modules/.bin:${PATH}" \
    NODE_PATH="/node-deps/node_modules"

##############################
# Installs ruby dependencies #
#############################################################################################
## @generated by .automation/build.py using descriptor files, please do not update manually ##
#############################################################################################

#GEM__START

#GEM__END

##############################
# COPY instructions #
#############################################################################################
## @generated by .automation/build.py using descriptor files, please do not update manually ##
#############################################################################################

#COPY__START
COPY --link --from=shfmt /bin/shfmt /usr/bin/
COPY --link --from=terragrunt /bin/terraform /usr/bin/
#COPY__END

#############################################################################################
## @generated by .automation/build.py using descriptor files, please do not update manually ##
#############################################################################################
#OTHER__START
# CSHARP installation
ENV PATH="${PATH}:/root/.dotnet/tools"

# POWERSHELL installation
RUN curl -L https://github.com/PowerShell/PowerShell/releases/download/v7.4.0/powershell-7.4.0-linux-musl-x64.tar.gz -o /tmp/powershell.tar.gz \
    && mkdir -p /opt/microsoft/powershell/7 \
    && tar zxf /tmp/powershell.tar.gz -C /opt/microsoft/powershell/7 \
    && chmod +x /opt/microsoft/powershell/7/pwsh \
    && ln -s /opt/microsoft/powershell/7/pwsh /usr/bin/pwsh \

# VBDOTNET installation
# Next line commented because already managed by another linter
# ENV PATH="${PATH}:/root/.dotnet/tools"

# shfmt installation
# Managed with COPY --link --from=shfmt /bin/shfmt /usr/bin/

# csharpier installation
    && dotnet tool install --global csharpier \

# roslynator installation
    && dotnet tool install -g roslynator.dotnet.cli \

# powershell_formatter installation
    && pwsh -c 'Install-Module -Name PSScriptAnalyzer -RequiredVersion ${PSSA_VERSION} -Scope AllUsers -Force'

# terraform-fmt installation
# Managed with COPY --link --from=terragrunt /bin/terraform /usr/bin/

#OTHER__END

################################
# Installs python dependencies #
################################
COPY megalinter /megalinter
RUN PYTHONDONTWRITEBYTECODE=1 python /megalinter/setup.py install \
    && PYTHONDONTWRITEBYTECODE=1 python /megalinter/setup.py clean --all \
    && rm -rf /var/cache/apk/* \
    && find . | grep -E "(/__pycache__$|\.pyc$|\.pyo$)" | xargs rm -rf

#######################################
# Copy scripts and rules to container #
#######################################
COPY megalinter/descriptors /megalinter-descriptors
COPY TEMPLATES /action/lib/.automation

# Copy server scripts
COPY server /server

###########################
# Get the build arguments #
###########################
ARG BUILD_DATE
ARG BUILD_REVISION
ARG BUILD_VERSION

#################################################
# Set ENV values used for debugging the version #
#################################################
ENV BUILD_DATE=$BUILD_DATE \
    BUILD_REVISION=$BUILD_REVISION \
    BUILD_VERSION=$BUILD_VERSION

#FLAVOR__START
ENV MEGALINTER_FLAVOR=formatters
#FLAVOR__END

#########################################
# Label the instance and set maintainer #
#########################################
LABEL com.github.actions.name="MegaLinter" \
      com.github.actions.description="The ultimate linters aggregator to make sure your projects are clean" \
      com.github.actions.icon="code" \
      com.github.actions.color="red" \
      maintainer="Nicolas Vuillamy <nicolas.vuillamy@gmail.com>" \
      org.opencontainers.image.created=$BUILD_DATE \
      org.opencontainers.image.revision=$BUILD_REVISION \
      org.opencontainers.image.version=$BUILD_VERSION \
      org.opencontainers.image.authors="Nicolas Vuillamy <nicolas.vuillamy@gmail.com>" \
      org.opencontainers.image.url="https://megalinter.io" \
      org.opencontainers.image.source="https://github.com/oxsecurity/megalinter" \
      org.opencontainers.image.documentation="https://megalinter.io" \
      org.opencontainers.image.vendor="Nicolas Vuillamy" \
      org.opencontainers.image.description="Lint your code base with GitHub Actions"

#EXTRA_DOCKERFILE_LINES__START
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x entrypoint.sh
ENTRYPOINT ["/bin/bash", "/entrypoint.sh"]
#EXTRA_DOCKERFILE_LINES__END
