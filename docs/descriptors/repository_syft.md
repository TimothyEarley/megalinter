---
title: syft configuration in MegaLinter
description: How to use syft (configure, ignore files, ignore errors, help & version documentations) to analyze REPOSITORY files
---
<!-- markdownlint-disable MD033 MD041 -->
<!-- @generated by .automation/build.py, please don't update manually -->

<div align="center">
  <a href="https://github.com/anchore/syft#readme" target="blank" title="Visit linter Web Site">
    <img src="https://user-images.githubusercontent.com/5199289/136844524-1527b09f-c5cb-4aa9-be54-5aa92a6086c1.png" alt="syft" height="150px" class="megalinter-banner">
  </a>
</div>

[![GitHub stars](https://img.shields.io/github/stars/anchore/syft?cacheSeconds=3600)](https://github.com/anchore/syft) ![sarif](https://shields.io/badge/-SARIF-orange) [![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/anchore/syft?sort=semver)](https://github.com/anchore/syft/releases) [![GitHub last commit](https://img.shields.io/github/last-commit/anchore/syft)](https://github.com/anchore/syft/commits) [![GitHub commit activity](https://img.shields.io/github/commit-activity/y/anchore/syft)](https://github.com/anchore/syft/graphs/commit-activity/) [![GitHub contributors](https://img.shields.io/github/contributors/anchore/syft)](https://github.com/anchore/syft/graphs/contributors/)

Builds a SBOM (Software Build Of Materials) from your repository

## syft documentation

- Version in MegaLinter: **0.101.1**
- Visit [Official Web Site](https://github.com/anchore/syft#readme){target=_blank}

[![syft - GitHub](https://gh-card.dev/repos/anchore/syft.svg?fullname=)](https://github.com/anchore/syft){target=_blank}

## Configuration in MegaLinter

- Enable syft by adding `REPOSITORY_SYFT` in [ENABLE_LINTERS variable](https://megalinter.io/beta/configuration/#activation-and-deactivation)
- Disable syft by adding `REPOSITORY_SYFT` in [DISABLE_LINTERS variable](https://megalinter.io/beta/configuration/#activation-and-deactivation)

| Variable                                    | Description                                                                                            | Default value                                   |
|---------------------------------------------|--------------------------------------------------------------------------------------------------------|-------------------------------------------------|
| REPOSITORY_SYFT_ARGUMENTS                   | User custom arguments to add in linter CLI call<br/>Ex: `-s --foo "bar"`                               |                                                 |
| REPOSITORY_SYFT_COMMAND_REMOVE_ARGUMENTS    | User custom arguments to remove from command line before calling the linter<br/>Ex: `-s --foo "bar"`   |                                                 |
| REPOSITORY_SYFT_PRE_COMMANDS                | List of bash commands to run before the linter                                                         | None                                            |
| REPOSITORY_SYFT_POST_COMMANDS               | List of bash commands to run after the linter                                                          | None                                            |
| REPOSITORY_SYFT_UNSECURED_ENV_VARIABLES     | List of env variables explicitly not filtered before calling REPOSITORY_SYFT and its pre/post commands | None                                            |
| REPOSITORY_SYFT_CONFIG_FILE                 | syft configuration file name</br>Use `LINTER_DEFAULT` to let the linter find it                        | `.syft.yaml`                                    |
| REPOSITORY_SYFT_RULES_PATH                  | Path where to find linter configuration file                                                           | Workspace folder, then MegaLinter default rules |
| REPOSITORY_SYFT_DISABLE_ERRORS              | Run linter but consider errors as warnings                                                             | `false`                                         |
| REPOSITORY_SYFT_DISABLE_ERRORS_IF_LESS_THAN | Maximum number of errors allowed                                                                       | `0`                                             |
| REPOSITORY_SYFT_CLI_EXECUTABLE              | Override CLI executable                                                                                | `['syft']`                                      |

## MegaLinter Flavours

This linter is available in the following flavours

|                                                                         <!-- -->                                                                         | Flavor                                                   | Description               | Embedded linters |                                                                                                                                                                                         Info |
|:--------------------------------------------------------------------------------------------------------------------------------------------------------:|:---------------------------------------------------------|:--------------------------|:----------------:|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| <img src="https://github.com/oxsecurity/megalinter/raw/main/docs/assets/images/mega-linter-square.png" alt="" height="32px" class="megalinter-icon"></a> | [all](https://megalinter.io/beta/supported-linters/)     | Default MegaLinter Flavor |       121        |                   ![Docker Image Size (tag)](https://img.shields.io/docker/image-size/oxsecurity/megalinter/beta) ![Docker Pulls](https://img.shields.io/docker/pulls/oxsecurity/megalinter) |
|      <img src="https://github.com/oxsecurity/megalinter/raw/main/docs/assets/icons/security.ico" alt="" height="32px" class="megalinter-icon"></a>       | [security](https://megalinter.io/beta/flavors/security/) | Optimized for security    |        24        | ![Docker Image Size (tag)](https://img.shields.io/docker/image-size/oxsecurity/megalinter-security/beta) ![Docker Pulls](https://img.shields.io/docker/pulls/oxsecurity/megalinter-security) |

## Behind the scenes

### How are identified applicable files

- If this linter is active, all files will always be linted

<!-- markdownlint-disable -->
<!-- /* cSpell:disable */ -->
### How the linting is performed

syft is called once on the whole project directory (`project` CLI lint mode)

- filtering can not be done using MegaLinter configuration variables,it must be done using syft configuration or ignore file (if existing)
- `VALIDATE_ALL_CODEBASE: false` doesn't make syft analyze only updated files

### Example calls

```shell
syft /tmp/lint
```


### Help content

```shell
Generate a packaged-based Software Bill Of Materials (SBOM) from container images and filesystems

Usage:
  syft [command]

Application Configuration:

  # the configuration file that was used to load application configuration (env: SYFT_CONFIG)
  config: ''

  # report output format (<format>=<file> to output to a file), formats=[cyclonedx-json cyclonedx-xml github-json spdx-json spdx-tag-value syft-json syft-table syft-text template] (env: SYFT_OUTPUT)
  output:
    - 'syft-table'

  # file to write the default report output to (default is STDOUT) (env: SYFT_LEGACYFILE)
  legacyFile: ''

  format:
    # (env: SYFT_FORMAT_PRETTY)
    pretty:

    template:
      # specify the path to a Go template file (env: SYFT_FORMAT_TEMPLATE_PATH)
      path: ''

    json:
      # (env: SYFT_FORMAT_JSON_LEGACY)
      legacy: false

      # (env: SYFT_FORMAT_JSON_PRETTY)
      pretty: false

    spdx-json:
      # (env: SYFT_FORMAT_SPDX_JSON_PRETTY)
      pretty: false

    cyclonedx-json:
      # (env: SYFT_FORMAT_CYCLONEDX_JSON_PRETTY)
      pretty: false

    cyclonedx-xml:
      # (env: SYFT_FORMAT_CYCLONEDX_XML_PRETTY)
      pretty: false

  # whether to check for an application update on start up or not (env: SYFT_CHECK_FOR_APP_UPDATE)
  check-for-app-update: true

  # enable one or more package catalogers (env: SYFT_CATALOGERS)
  catalogers: []

  # set the base set of catalogers to use (defaults to 'image' or 'directory' depending on the scan source) (env: SYFT_DEFAULT_CATALOGERS)
  default-catalogers: []

  # add, remove, and filter the catalogers to be used (env: SYFT_SELECT_CATALOGERS)
  select-catalogers: []

  package:
    # (env: SYFT_PACKAGE_SEARCH_UNINDEXED_ARCHIVES)
    search-unindexed-archives: false

    # (env: SYFT_PACKAGE_SEARCH_INDEXED_ARCHIVES)
    search-indexed-archives: true

    # (env: SYFT_PACKAGE_EXCLUDE_BINARY_OVERLAP_BY_OWNERSHIP)
    exclude-binary-overlap-by-ownership: true

  file:
    metadata:
      # (env: SYFT_FILE_METADATA_SELECTION)
      selection: owned-by-package

      # (env: SYFT_FILE_METADATA_DIGESTS)
      digests:
        - 'sha1'
        - 'sha256'

    content:
      # (env: SYFT_FILE_CONTENT_SKIP_FILES_ABOVE_SIZE)
      skip-files-above-size: 256000

      # (env: SYFT_FILE_CONTENT_GLOBS)
      globs: []

  # selection of layers to catalog, options=[squashed all-layers] (env: SYFT_SCOPE)
  scope: 'squashed'

  # number of cataloger workers to run in parallel (env: SYFT_PARALLELISM)
  parallelism: 1

  relationships:
    # include package-to-file relationships that indicate which files are owned by which packages. (env: SYFT_RELATIONSHIPS_PACKAGE_FILE_OWNERSHIP)
    package-file-ownership: true

    # include package-to-package relationships that indicate one package is owned by another due to files claimed to be owned by one package are also evidence of another package's existence. (env: SYFT_RELATIONSHIPS_PACKAGE_FILE_OWNERSHIP_OVERLAP)
    package-file-ownership-overlap: true

  golang:
    # (env: SYFT_GOLANG_SEARCH_LOCAL_MOD_CACHE_LICENSES)
    search-local-mod-cache-licenses: false

    # (env: SYFT_GOLANG_LOCAL_MOD_CACHE_DIR)
    local-mod-cache-dir: ''

    # (env: SYFT_GOLANG_SEARCH_REMOTE_LICENSES)
    search-remote-licenses: false

    # (env: SYFT_GOLANG_PROXY)
    proxy: ''

    # (env: SYFT_GOLANG_NO_PROXY)
    no-proxy: ''

  java:
    # (env: SYFT_JAVA_USE_NETWORK)
    use-network: false

    # (env: SYFT_JAVA_MAVEN_URL)
    maven-url: ''

    # (env: SYFT_JAVA_MAX_PARENT_RECURSIVE_DEPTH)
    max-parent-recursive-depth: 0

  javascript:
    # (env: SYFT_JAVASCRIPT_SEARCH_REMOTE_LICENSES)
    search-remote-licenses: false

    # (env: SYFT_JAVASCRIPT_NPM_BASE_URL)
    npm-base-url: ''

  linux-kernel:
    # (env: SYFT_LINUX_KERNEL_CATALOG_MODULES)
    catalog-modules: true

  python:
    # (env: SYFT_PYTHON_GUESS_UNPINNED_REQUIREMENTS)
    guess-unpinned-requirements: false

  registry:
    # (env: SYFT_REGISTRY_INSECURE_SKIP_TLS_VERIFY)
    insecure-skip-tls-verify: false

    # (env: SYFT_REGISTRY_INSECURE_USE_HTTP)
    insecure-use-http: false

    auth: []

    # (env: SYFT_REGISTRY_CA_CERT)
    ca-cert: ''

  # an optional platform specifier for container image sources (e.g. 'linux/arm64', 'linux/arm64/v8', 'arm64', 'linux') (env: SYFT_PLATFORM)
  platform: ''

  # (env: SYFT_NAME)
  name: ''

  source:
    # set the name of the target being analyzed (env: SYFT_SOURCE_NAME)
    name: ''

    # set the version of the target being analyzed (env: SYFT_SOURCE_VERSION)
    version: ''

    # base directory for scanning, no links will be followed above this directory, and all paths will be reported relative to this directory (env: SYFT_SOURCE_BASE_PATH)
    base-path: ''

    file:
      # (env: SYFT_SOURCE_FILE_DIGESTS)
      digests:
        - 'sha256'

    image:
      # (env: SYFT_SOURCE_IMAGE_DEFAULT_PULL_SOURCE)
      default-pull-source: ''

  # exclude paths from being scanned using a glob expression (env: SYFT_EXCLUDE)
  exclude: []

  log:
    # suppress all logging output (env: SYFT_LOG_QUIET)
    quiet: false

    # increase verbosity (-v = info, -vv = debug) (env: SYFT_LOG_VERBOSITY)
    verbosity: 0

    # explicitly set the logging level (available: [error warn info debug trace]) (env: SYFT_LOG_LEVEL)
    level: warn

    # file path to write logs to (env: SYFT_LOG_FILE)
    file: ''

  dev:
    # capture resource profiling data (available: [cpu, mem]) (env: SYFT_DEV_PROFILE)
    profile: none

  # show catalogers that have been de-selected (env: SYFT_SHOW_HIDDEN)
  show-hidden: false

  # (env: SYFT_KEY)
  key:

  # (env: SYFT_PASSWORD)
  password:

Config Search Locations:
  - .syft.yaml
  - .syft/config.yaml
  - /root/.syft.yaml
  - /root/.config/syft/config.yaml
  - /etc/xdg/syft/config.yaml

Available Commands:
  attest      Generate an SBOM as an attestation for the given [SOURCE] container image
  cataloger   Show available catalogers and configuration
  completion  Generate the autocompletion script for the specified shell
  convert     Convert between SBOM formats
  help        Help about any command
  login       Log in to a registry
  scan        Generate an SBOM
  version     show version information

Flags:
      --base-path string                          base directory for scanning, no links will be followed above this directory, and all paths will be reported relative to this directory
  -c, --config string                             syft configuration file
      --exclude stringArray                       exclude paths from being scanned using a glob expression
      --file string                               file to write the default report output to (default is STDOUT) (DEPRECATED: use: output)
  -h, --help                                      help for syft
      --name string                               set the name of the target being analyzed (DEPRECATED: use: source-name)
  -o, --output stringArray                        report output format (<format>=<file> to output to a file), formats=[cyclonedx-json cyclonedx-xml github-json spdx-json spdx-tag-value syft-json syft-table syft-text template] (default [syft-table])
      --override-default-catalogers stringArray   set the base set of catalogers to use (defaults to 'image' or 'directory' depending on the scan source)
      --platform string                           an optional platform specifier for container image sources (e.g. 'linux/arm64', 'linux/arm64/v8', 'arm64', 'linux')
  -q, --quiet                                     suppress all logging output
  -s, --scope string                              selection of layers to catalog, options=[squashed all-layers] (default "squashed")
      --select-catalogers stringArray             add, remove, and filter the catalogers to be used
      --source-name string                        set the name of the target being analyzed
      --source-version string                     set the version of the target being analyzed
  -t, --template string                           specify the path to a Go template file
  -v, --verbose count                             increase verbosity (-v = info, -vv = debug)
      --version                                   version for syft

Use "syft [command] --help" for more information about a command.
```

### Installation on mega-linter Docker image

- Dockerfile commands :
```dockerfile
RUN curl -sSfL https://raw.githubusercontent.com/anchore/syft/main/install.sh | sh -s -- -b /usr/local/bin
```

