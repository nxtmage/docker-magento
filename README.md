    <h1 align="center">nxtmage/docker-magento</h1>

    <div align="center">
      <p>Docker Configuration for Magento 2 and Mage-OS</p>
      <img src="https://img.shields.io/badge/magento-2.X-brightgreen.svg?logo=magento&longCache=true" alt="Supported Magento Versions" />
      <a href="https://hub.docker.com/r/markoshust/magento-php/" target="_blank"><img src="https://img.shields.io/docker/pulls/markoshust/magento-php.
  svg?label=php%20docker%20pulls" alt="Docker Hub Pulls - PHP" /></a>
      <a href="https://hub.docker.com/r/markoshust/magento-nginx/" target="_blank"><img src="https://img.shields.io/docker/pulls/markoshust/magento-nginx.
  svg?label=nginx%20docker%20pulls" alt="Docker Hub Pulls - Nginx" /></a>
      <a href="https://github.com/nxtmage/docker-magento/graphs/commit-activity" target="_blank"><img src="https://img.shields.io/badge/maintained%3F-yes-
  brightgreen.svg" alt="Maintained - Yes" /></a>
      <img src="https://img.shields.io/badge/apple%20silicon%20support-yes-brightgreen" alt="Apple Silicon Support" />
      <a href="https://opensource.org/licenses/MIT" target="_blank"><img src="https://img.shields.io/badge/license-MIT-blue.svg" /></a>
    </div>

    ## Table of Contents

    - [Docker Hub](#docker-hub)
    - [Usage](#usage)
    - [Prerequisites](#prerequisites)
    - [Setup](#setup)
      - [Automated Setup (New Project)](#automated-setup-new-project)
      - [Manual Setup](#manual-setup)
      - [Elasticsearch vs OpenSearch](#elasticsearch-vs-opensearch)
    - [WSL2 / Windows Setup & Fix for Slow Start / Container Failures](#wsl2--windows-setup--fix-for-slow-start--container-failures)
    - [Updates](#updates)
      - [Persisting local compose changes across updates](#persisting-local-compose-changes-across-updates)
      - [Auto-detected service image versions](#auto-detected-service-image-versions)
    - [Custom CLI Commands](#custom-cli-commands)
    - [Configuration & Guides](#configuration--guides)
      - [Accessing the Magento Backend](#accessing-the-magento-backend)
      - [Caching](#caching)
      - [Database](#database)
      - [Composer Authentication](#composer-authentication)
      - [Email / Mailcatcher](#email--mailcatcher)
      - [Redis](#redis)
      - [PhpMyAdmin](#phpmyadmin)
      - [Xdebug & IDEs](#xdebug--ides)
      - [SSH](#ssh)
      - [Linux](#linux)
      - [Multi-storefront / Multi-domain Setup](#multi-storefront--multi-domain-setup)
      - [Blackfire.io](#blackfireio)
      - [Cloudflare Tunnel](#cloudflare-tunnel)
      - [Grunt + LiveReload](#grunt--livereload-for-frontend-development)
      - [PHP-SPX](#php-spx)
    - [Troubleshooting](#troubleshooting)
    - [Credits & License](#credits--license)

    ---

    ## Docker Hub

    View Dockerfiles for the latest tags:

    - [markoshust/magento-nginx (Docker Hub)](https://hub.docker.com/r/markoshust/magento-nginx/)
      - [`1.18`, `1.18-8`](images/nginx/1.18)
      - [`1.22`, `1.22-0`](images/nginx/1.22)
      - [`1.24`, `1.24-1`](images/nginx/1.24)
      - [`1.26`, `1.26-0`](images/nginx/1.26)
      - [`1.28`, `1.28-0`](images/nginx/1.28)
    - [markoshust/magento-php (Docker Hub)](https://hub.docker.com/r/markoshust/magento-php/)
      - [`8.1-fpm`, `8.1-fpm-9`](images/php/8.1) · [`8.1-fpm-xdebug`, `8.1-fpm-xdebug-9`](images/php/8.1)
      - [`8.2-fpm`, `8.2-fpm-9`](images/php/8.2) · [`8.2-fpm-xdebug`, `8.2-fpm-xdebug-9`](images/php/8.2)
      - [`8.3-fpm`, `8.3-fpm-7`](images/php/8.3) · [`8.3-fpm-xdebug`, `8.3-fpm-xdebug-7`](images/php/8.3)
      - [`8.4-fpm`, `8.4-fpm-2`](images/php/8.4) · [`8.4-fpm-xdebug`, `8.4-fpm-xdebug-2`](images/php/8.4)
      - [`8.5-fpm`, `8.5-fpm-0`](images/php/8.5) · [`8.5-fpm-xdebug`, `8.5-fpm-xdebug-0`](images/php/8.5)
    - [markoshust/magento-opensearch (Docker Hub)](https://hub.docker.com/r/markoshust/magento-opensearch/)
      - [`1.2`, `1.2-0`](images/opensearch/1.2)
      - [`2.5`, `2.5-1`](images/opensearch/2.5)
      - [`2.12`, `2.12-0`](images/opensearch/2.12)
      - [`3`, `3-0`](images/opensearch/3)
    - [markoshust/magento-elasticsearch (Docker Hub)](https://hub.docker.com/r/markoshust/magento-elasticsearch/)
      - [`7.16`, `7.16-0`](images/elasticsearch/7.16)
      - [`7.17`, `7.17-1`](images/elasticsearch/7.17)
      - [`8.4`, `8.4-0`](images/elasticsearch/8.4)
      - [`8.5`, `8.5-0`](images/elasticsearch/8.5)
      - [`8.7`, `8.7-0`](images/elasticsearch/8.7)
      - [`8.11`, `8.11-0`](images/elasticsearch/8.11)
      - [`8.13`, `8.13-0`](images/elasticsearch/8.13)
    - [markoshust/magento-rabbitmq (Docker Hub)](https://hub.docker.com/r/markoshust/magento-rabbitmq/)
      - [`3.9`, `3.9-0`](images/rabbitmq/3.9)
      - [`3.11`, `3.11-1`](images/rabbitmq/3.11)
      - [`3.12`, `3.12-0`](images/rabbitmq/3.12)
      - [`3.13`, `3.13-0`](images/rabbitmq/3.13)
      - [`4.1`, `4.1-0`](images/rabbitmq/4.1)
      - [`4.2`, `4.2-0`](images/rabbitmq/4.2)
    - [markoshust/ssh (Docker Hub)](https://hub.docker.com/r/markoshust/magento-ssh/)
      - [`latest`](images/ssh)

    ---

    ## Usage

    This configuration is intended to be used as a Docker-based development environment for Magento 2 and Mage-OS.

    Folders:
    - `images`: Docker images for nginx and php
    - `compose`: Sample setups with Docker Compose

    ---

    ## Prerequisites

    - Docker Desktop or OrbStack running with at least **6GB–8GB of RAM** allocated, a multi-core CPU, and SSD storage.
    - Tested on macOS and Linux. Windows is supported via **WSL 2** (see the [WSL2 section](#wsl2--windows-setup--fix-for-slow-start--container-failures) below
  for required fixes).

    ---

    ## Setup

    ### Automated Setup (New Project)

    ```bash
    # Create your project directory then navigate into it:
    mkdir -p ~/Sites/magento
    cd $_

    # Run the automated installer:
    curl -s https://raw.githubusercontent.com/nxtmage/docker-magento/master/lib/onelinesetup | bash -s -- magento.test mageos 3.0.0

  • magento.test defines the hostname.
  • mageos is the edition (mageos, community, or enterprise).
  • 3.0.0 specifies the version (e.g. community 2.4.9).
  • Since an entry is written to /etc/hosts for DNS resolution, you may be prompted for your system password.

  Access your site at https://magento.test.

  #### Install sample data and development modules

    bin/init
  ──────
  ### Manual Setup

  #### New Projects

    # Create your project directory then navigate into it:
    mkdir -p ~/Sites/magento
    cd $_

    # Download the Docker Compose template:
    curl -s https://raw.githubusercontent.com/nxtmage/docker-magento/master/lib/template | bash

    # Download Magento or Mage-OS:
    bin/download mageos 3.0.0
    # Or for Adobe Commerce / Open Source:
    # bin/download community 2.4.9

    # Run the setup installer:
    bin/setup magento.test

    # Initialize development environment with sample data and dev modules:
    bin/init

    open https://magento.test

  #### Existing Projects

    # Create your project directory:
    mkdir -p ~/Sites/magento
    cd $_

    # Download the Docker Compose template:
    curl -s https://raw.githubusercontent.com/nxtmage/docker-magento/master/lib/template | bash

    # Backup existing database:
    bin/mysqldump > ~/Sites/existing/magento.sql

    # Copy existing source code into the src directory:
    cp -R ~/Sites/existing src
    # or: git clone git@github.com:myrepo.git src

    # Start containers and copy files:
    bin/start --no-dev
    bin/copytocontainer --all

    # Populate vendor dependencies:
    bin/composer install

    # Import database:
    bin/mysql < ../existing/magento.sql

    # Import environment configuration:
    bin/magento app:config:import

    # Setup domain and restart:
    bin/setup-domain yoursite.test
    bin/restart

    open https://yoursite.test
  ──────
  ### Elasticsearch vs OpenSearch

  OpenSearch is the default search engine. To use Elasticsearch instead:

  1. In both compose.yaml https://github.com/nxtmage/docker-magento/blob/master/compose/compose.yaml and compose.healthcheck.yaml https://github.
  com/nxtmage/docker-magento/blob/master/compose/compose.healthcheck.yaml, comment out the opensearch container and uncomment the elasticsearch container.
  2. In bin/setup-install, update the search engine options from:
    --opensearch-host="$OPENSEARCH_HOST" \
    --opensearch-port="$OPENSEARCH_PORT" \
  to:
    --elasticsearch-host="$ES_HOST" \
    --elasticsearch-port="$ES_PORT" \

  ──────
  ## WSL2 / Windows Setup & Fix for Slow Start / Container Failures

  Running Docker under Windows Subsystem for Linux (WSL2) can cause slow container starts, healthcheck timeouts (e.g. OpenSearch or MariaDB failing), or random
  container crashes if not configured properly. Apply the following fixes:

  ### 1. Store Files Inside the Linux Filesystem (Critical)

  Never place project folders in the Windows filesystem (such as /mnt/c/Users/...). Crossing the 9P filesystem bridge causes severe I/O degradation and causes
  containers to fail healthchecks during startup.

  • Fix: Keep your project strictly inside the WSL Linux user home directory (e.g., ~/Sites/magento or /home/<username>/projects/magento).
  • Use the VS Code WSL Extension or PhpStorm via WSL to open the project.

  ### 2. Configure WSL2 Resource Allocation (.wslconfig)

  By default, WSL2 can consume all host memory or starve the containers of enough RAM during heavy container initialization, resulting in containers exiting or
  being OOM-killed.

  • Open or create C:\Users\<YourUsername>\.wslconfig in Windows:
    [wsl2]
    memory=8GB           # Minimum 6GB; 8GB+ recommended for Magento/OpenSearch
    processors=4         # Allocate at least 4 CPU cores
    swap=4GB             # Provide adequate swap to prevent abrupt OOM kills
    localhostForwarding=true

  • Apply changes in PowerShell (Admin):
    wsl --shutdown


  ### 3. Extend Compose & Daemon Startup Timeouts

  During cold starts on WSL, container healthchecks (OpenSearch, MariaDB, RabbitMQ) can take longer to turn green than Docker Compose defaults allow.

  • Add these environment variables to your WSL ~/.bashrc or ~/.zshrc:
    export COMPOSE_HTTP_TIMEOUT=300
    export DOCKER_CLIENT_TIMEOUT=300

  • If a service repeatedly fails healthcheck on startup, override the start_period by creating compose.override.yaml:
    services:
      opensearch:
        healthcheck:
          start_period: 120s
      db:
        healthcheck:
          start_period: 60s


  ### 4. Prevent CRLF Line Ending Breakage

  If scripts in bin/ are saved with Windows CRLF endings, bash in the containers will throw \r: command not found or fail silently on start.

  • In WSL, set Git to preserve Unix line endings:
    git config --global core.autocrlf false
    git config --global core.eol lf

  • If files were already downloaded with CRLF endings:
    find ./bin -type f -exec sed -i 's/\r$//' {} +


  ### 5. Exclude WSL / Docker from Windows Defender

  Real-time scanning of the WSL2 virtual disk (ext4.vhdx) significantly degrades I/O performance during container startup.

  • Open Windows Security > Virus & threat protection settings > Exclusions.
  • Add a folder exclusion for:
  %LOCALAPPDATA%\Docker\wsl
  and your WSL network path:
  \\wsl$\<DistroName>\home\<username>\Sites

  ### 6. WSL Firewall Rule for Xdebug

  Run this command once in Windows PowerShell (Admin) to ensure incoming Xdebug packets from WSL to host are not blocked:

    New-NetFirewallRule -DisplayName "WSL-Xdebug" -Direction Inbound -InterfaceAlias "vEthernet (WSL)" -Action Allow
  ──────
  ## Updates

  To update your project to the latest version of docker-magento, run:

    bin/update

  Review modified files, then restart containers with bin/restart.

  ### Persisting local compose changes across updates

  Files shipped by the template (compose.yaml, compose.dev.yaml, compose.healthcheck.yaml) are overwritten by bin/update. To persist local adjustments, create a
  compose.override.yaml in your project root:

    # compose.override.yaml
    services:
      db:
        ports:
          - "3307:3306"

  bin/docker-compose automatically appends compose.override.yaml last in the -f chain so its values take precedence.

  ### Auto-detected service image versions

  When you install Magento or Mage-OS via bin/download (or onelinesetup), bin/detect-versions runs and pins compatible images (PHP, nginx, OpenSearch, database,
  RabbitMQ, cache) in a generated compose.versions.yaml.

  Load order (later files win):

  1. compose.yaml
  2. compose.healthcheck.yaml
  3. compose.dev.yaml (unless --no-dev)
  4. compose.versions.yaml (auto-generated, gitignored)
  5. compose.override.yaml (manual overrides)

  To regenerate pins after an in-place upgrade:

    bin/detect-versions
  ──────
  ## Custom CLI Commands

   Command                                          | Description                                             | Example
  --------------------------------------------------|---------------------------------------------------------|--------------------------------------------------
   bin/analyse                                      | Run PHPStan static analysis                             | bin/analyse app/code
   bin/bash                                         | Open bash shell in phpfpm container                     | bin/bash
   bin/blackfire                                    | Manage Blackfire profiler (enable, disable, status)     | bin/blackfire enable
   bin/cache-clean                                  | Manage cache-clean file watcher                         | bin/cache-clean config full_page
   bin/check-dependencies                           | Check recommended dependencies for installed version    | bin/check-dependencies
   bin/detect-versions                              | Generate compose.versions.yaml based on Magento version | bin/detect-versions
   bin/cli                                          | Execute command in container                            | bin/cli ls -la
   bin/clinotty                                     | Execute command in container without TTY                | bin/clinotty chmod u+x bin/magento
   bin/cliq                                         | Execute command quietly (redirects output to /dev/null) | bin/cliq bin/magento cache:flush
   bin/composer                                     | Run Composer inside container                           | bin/composer install
   bin/configure-linux                              | Add Docker IP to /etc/hosts and setup Xdebug port       | bin/configure-linux
   bin/copyfromcontainer                            | Copy file/folder from container to host                 | bin/copyfromcontainer vendor
   bin/copytocontainer                              | Copy file/folder from host to container                 | bin/copytocontainer --all
   bin/create-user                                  | Create admin user or customer account                   | bin/create-user
   bin/cron                                         | Control cron service (start, stop)                      | bin/cron start
   bin/debug-cli                                    | Run CLI command with Xdebug enabled                     | bin/debug-cli bin/magento indexer:reindex
   bin/deploy                                       | Run static deploy pipeline                              | bin/deploy en_US
   bin/dev-test-run                                 | Run PHPUnit tests                                       | bin/dev-test-run unit
   bin/dev-urn-catalog-generate                     | Generate URNs for IDE schema mapping                    | bin/dev-urn-catalog-generate
   bin/devconsole                                   | Open n98-magerun2 interactive console                   | bin/devconsole
   bin/docker-compose                               | Wrapper for Docker Compose V1/V2 with config loading    | bin/docker-compose ps
   bin/docker-stats                                 | Show real-time CPU and memory usage of containers       | bin/docker-stats
   bin/download                                     | Download Magento/Mage-OS release packages               | bin/download mageos 3.0.0
   bin/ece-patches                                  | Run Cloud Patches CLI                                   | bin/ece-patches apply
   bin/fixowns                                      | Fix filesystem user ownership in container              | bin/fixowns
   bin/fixperms                                     | Fix filesystem file/directory permissions               | bin/fixperms
   bin/grunt                                        | Run Grunt tasks                                         | bin/grunt exec
   bin/init                                         | Initialize sample data and dev modules                  | bin/init
   bin/install-php-extensions                       | Install additional PHP extensions                       | bin/install-php-extensions sourceguardian
   bin/log                                          | Tail Magento log files                                  | bin/log system.log
   bin/magento                                      | Run Magento CLI                                         | bin/magento cache:flush
   bin/magento-version                              | Output installed Magento version                        | bin/magento-version
   bin/mysql                                        | Access MySQL / MariaDB CLI                              | bin/mysql
   bin/mysqldump                                    | Export MySQL / MariaDB database dump                    | bin/mysqldump > dump.sql
   bin/n98-magerun2                                 | Run n98-magerun2 commands                               | bin/n98-magerun2 dev:console
   bin/node                                         | Execute Node.js binary                                  | bin/node --version
   bin/npm                                          | Execute npm binary                                      | bin/npm install
   bin/phpcbf                                       | Run PHP_CodeSniffer autofixer                           | bin/phpcbf app/code/MyVendor
   bin/phpcs                                        | Run PHP_CodeSniffer validation                          | bin/phpcs app/code/MyVendor
   bin/redis                                        | Run Redis CLI commands                                  | bin/redis redis-cli monitor
   bin/restart                                      | Restart all project containers                          | bin/restart
   bin/root                                         | Run command as root in container                        | bin/root apt-get update
   bin/setup                                        | Run full Magento install procedure                      | bin/setup magento.test
   bin/setup-composer-auth                          | Set up Composer auth.json                               | bin/setup-composer-auth
   bin/setup-domain                                 | Configure domain hostname & base URL                    | bin/setup-domain magento.test
   bin/setup-ssl                                    | Generate self-signed SSL certificates                   | bin/setup-ssl magento.test
   bin/start                                        | Start all project containers                            | bin/start
   bin/status                                       | Check container status                                  | bin/status
   bin/stop                                         | Stop all project containers                             | bin/stop
   bin/remove                                       | Stop and remove project containers                      | bin/remove
   bin/removeall                                    | Wipe all containers, networks, and persistent volumes   | bin/removeall
  ──────
  ## Configuration & Guides

  ### Accessing the Magento Backend

  1. Open https://magento.test/admin/.
  2. Default credentials:
      • Username: john.smith
      • Password: password123
  3. If 2FA is active, retrieve the email verification code via Mailcatcher at http://magento.test:1080 (sent to john.smith@gmail.com).

  ### Caching

  Caches automatically refresh on code changes using cache-clean https://github.com/mage2tv/magento-cache-clean. To disable this watcher, comment out the watcher
  line at the end of bin/start.

  ### Database

  The default database is MariaDB (11.4). The internal database hostname within Docker is db.

  • Connect to MySQL CLI: bin/mysql
  • Import a database: bin/mysql < magento.sql
  • Export a database: bin/mysqldump > magento.sql

  ### Composer Authentication

  1. Copy src/auth.json.sample to src/auth.json.
  2. Enter your Magento Marketplace public/private keys.
  3. Sync into the container: bin/copytocontainer auth.json.

  ### Email / Mailcatcher

  View emails sent by Magento locally at http://magento.test:1080.

  • SMTP host: mailcatcher
  • SMTP port: 1025

  ### Redis

  Redis is the default cache and session storage engine. To reconfigure manually on existing instances:

    bin/magento setup:config:set --cache-backend=redis --cache-backend-redis-server=redis --cache-backend-redis-db=0
    bin/magento setup:config:set --page-cache=redis --page-cache-redis-server=redis --page-cache-redis-db=1
    bin/magento setup:config:set --session-save=redis --session-save-redis-host=redis --session-save-redis-log-level=4 --session-save-redis-db=2

  Monitor Redis live: bin/redis redis-cli monitor

  ### PhpMyAdmin

  Included in compose.dev.yaml at http://localhost:8080.

  • Username: magento
  • Password: magento

  ### Xdebug & IDEs

  Xdebug runs inside a dedicated phpfpm-xdebug service. Requests containing the XDEBUG_SESSION cookie are routed to it by Nginx automatically.

  #### VS Code

  1. Install the PHP Debug extension.
  2. In .vscode/launch.json:
    {
      "version": "0.2.0",
      "configurations": [
        {
          "name": "Listen for Xdebug",
          "type": "php",
          "request": "launch",
          "port": 9003,
          "pathMappings": {
            "/var/www/html": "${workspaceFolder}"
          }
        }
      ]
    }


  #### PhpStorm

  1. Install the Xdebug Helper browser extension and set IDE Key to PHPSTORM.
  2. In Preferences > PHP > Servers, add a server named magento, host magento.test, port 80, with path mapping from src to /var/www/html.
  3. In Preferences > PHP > Debug, ensure debug port is set to 9000,9003.
  4. Create a PHP Remote Debug run configuration listening on the magento server.

  ### SSH

  To bypass host-mount filesystem latency in IDEs, copy compose.dev-ssh.yaml to compose.dev.yaml before installation. Connect via SFTP to localhost on the
  configured SSH port using username app and password app.

  ### Linux

  1. Install dependencies:
    sudo apt install curl libnss3-tools unzip rsync

  2. For host.docker.internal resolution, add "host.docker.internal:172.17.0.1" to app.extra_hosts in compose.yaml, matching your docker bridge IP (docker run --
  rm alpine ip route | awk 'NR==1 {print $3}').
  3. For Elasticsearch / OpenSearch, increase memory map count in /etc/sysctl.conf:
    vm.max_map_count=262144


  ### Multi-storefront / Multi-domain Setup

  Use compose.override.yaml to map domain names to MAGE_RUN_CODE:

    # store.map.conf
    map $http_host $MAGE_RUN_CODE {
        store1.example.test  store1_view;
        store2.example.test  store2_view;
        default              default;
    }

    # compose.override.yaml
    services:
      app:
        volumes:
          - ./store.map.conf:/etc/nginx/conf.d/store.map.conf:cached

  Generate SSL certificates: bin/setup-ssl store1.example.test store2.example.test.

  ### Blackfire.io

  1. Uncomment blackfire in compose.yaml.
  2. Put server keys in env/blackfire.env and client keys in env/phpfpm.env.
  3. Run bin/restart.

  ### Cloudflare Tunnel

  1. Create a tunnel in Cloudflare Zero Trust and save token to env/cloudflare.env.
  2. Uncomment the Cloudflare service in compose.yaml.
  3. Point the service URL to https://<app-container-name>:8443 with No TLS Verify checked.

  ### Grunt + LiveReload for Frontend Development

  1. Set up your theme under app/design/frontend/<Vendor>/<theme>.
  2. Add the LiveReload snippet to layout/default_head_blocks.xml:
    <script defer="true" src="/livereload.js?port=443" src_type="url"/>

  3. Run bin/setup-grunt.
  4. Start the file watcher: bin/grunt watch.

  ### PHP-SPX

  Access the SPX profiler UI directly at: https://magento.test/?SPX_UI_URI=/

  CLI usage:

    SPX_REPORT=full SPX_ENABLED=1 SPX_SAMPLING_PERIOD=5000 bin/magento cache:flush
  ──────
  ## Troubleshooting

  ### Failed install due to non-empty directory

  If an installation fails, subsequent attempts fail because Composer requires an empty directory:

    bin/removeall
    cd ..
    rm -rf yourproject

  Then recreate the directory and run the installer again.
  ──────
  ## Credits & License

  Originally created by Mark Shust https://github.com/markshust. Maintained and adapted by nxtmage https://github.com/nxtmage/docker-magento.

  Released under the MIT License https://opensource.org/licenses/MIT.
