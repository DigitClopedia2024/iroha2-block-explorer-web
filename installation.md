Here's our own revised `INSTALLATION.md`incorporating some relevant detailed requirements about the Web UI Explorer (frontend) for the private blockchain ledger Iroha V2 (version=2.0.0-rc.2.0 git_commit_sha=5ba8a6ccf) and the Web Explorer as backend (version 0.3.0) with installation date on 15th May 2025.

-----

# Installation Guide

This document provides a step-by-step guide to set up and run the Web3 project "Iroha V2" applications on a Linux/Debian machine using Docker and Nginx. This guide is designed to be comprehensive, ensuring a smooth installation process from system preparation to verifying the running application. It emphasizes security, version control, and clear instructions for the Docker container management.

**Table of Contents:**

1.  [System Requirements and Preparation](https://www.google.com/search?q=%231-system-requirements-and-preparation)
      * [Supported Operating Systems](https://www.google.com/search?q=%23supported-operating-systems)
      * [Essential System Updates](https://www.google.com/search?q=%23essential-system-updates)
      * [Install Git](https://www.google.com/search?q=%23install-git)
      * [Install Node.js (v20+) and Corepack](https://www.google.com/search?q=%23install-nodejs-v20-and-corepack)
      * [Install Docker Engine](https://www.google.com/search?q=%23install-docker-engine)
      * [Configure Docker Permissions](https://www.google.com/search?q=%23configure-docker-permissions)
      * [Install Gedit (Graphical Text Editor)](https://www.google.com/search?q=%23install-gedit-graphical-text-editor)
        
2.1  Installation of Iroha V2 Blockchain ledger ... see full documentation [here](https://docs.iroha.tech/get-started/)

2.2  Installation of Iroha BLock Explorer (backend) ... see full documentation [here](https://github.com/soramitsu/iroha2-block-explorer-backend)

2.3  [Repository Setup and Version Freezing](https://www.google.com/search?q=%232-repository-setup-and-version-freezing)
      * [Clone the Repository](https://www.google.com/search?q=%23clone-the-repository)
      * [Verify and Freeze Repository Version (Commit Hash Check)](https://www.google.com/search?q=%23verify-and-freeze-repository-version-commit-hash-check)
      * [Navigate to Project Directory](https://www.google.com/search?q=%23navigate-to-project-directory)
      
3.  [Building the Frontend Application](https://www.google.com/search?q=%233-building-the-frontend-application)

4.  [Docker Configuration and Deployment](https://www.google.com/search?q=%234-docker-configuration-and-deployment)
      * [Understanding the Configuration Files](https://www.google.com/search?q=%23understanding-the-configuration-files)
      * [Building the Docker Image](https://www.google.com/search?q=%23building-the-docker-image)
      * [Initial Container Deployment (`docker run`)](https://www.google.com/search?q=%23initial-container-deployment-docker-run)
      * [Verifying the Running Container](https://www.google.com/search?q=%23verifying-the-running-container)
      * [Accessing the Web UI](https://www.google.com/search?q=%23accessing-the-web-ui)

5.  [Configuring Nginx Inside the Docker Container](https://www.google.com/search?q=%235-configuring-nginx-inside-the-docker-container)
      * [Identifying the Nginx Configuration File](https://www.google.com/search?q=%23identifying-the-nginx-configuration-file)
      * [Exporting the Default Nginx Configuration](https://www.google.com/search?q=%23exporting-the-default-nginx-configuration)
      * [Editing the Configuration File with Gedit](https://www.google.com/search?q=%23editing-the-configuration-file-with-gedit)
      * [Importing the Modified Configuration](https://www.google.com/search?q=%23importing-the-modified-configuration)
      * [Restarting Nginx Inside the Container](https://www.google.com/search?q=%23restarting-nginx-inside-the-container)
      * [Final Verification](https://www.google.com/search?q=%23final-verification)

6.  [Managing Docker Containers (`run`, `stop`, `start`, `rm`, `up`, `down`)](https://www.google.com/search?q=%236-managing-docker-containers-run-stop-start-rm-up-down)
      * [`docker run`](https://www.google.com/search?q=%23docker-run)
      * [`docker stop` and `docker start`](https://www.google.com/search?q=%23docker-stop-and-docker-start)
      * [`docker rm`](https://www.google.com/search?q=%23docker-rm)
      * [Note on `docker compose up` and `docker compose down`](https://www.google.com/search?q=%23note-on-docker-compose-up-and-docker-compose-down)

7.  [Post-Installation Checks and Troubleshooting](https://www.google.com/search?q=%237-post-installation-checks-and-troubleshooting)
      * [Checking Container Logs](https://www.google.com/search?q=%23checking-container-logs)
      * [Common Issues](https://www.google.com/search?q=%23common-issues)

8.  [Backup and Maintenance](https://www.google.com/search?q=%238-backup-and-maintenance)
      * [Committing Changes](https://www.google.com/search?q=%23committing-changes)
      * [Creating a Backup](https://www.google.com/search?q=%23creating-a-backup)

9.  [For Developers](https://www.google.com/search?q=%239-for-developers)
      * [Frontend API Requests](https://www.google.com/search?q=%23frontend-api-requests)
      * [Backend API Communication](https://www.google.com/search?q=%23backend-api-communication)
-----

## 1\. System Requirements and Preparation

Before you begin, ensure your system meets the following requirements and is properly prepared.

### Supported Operating Systems

This guide is tested on and assumes a Debian-based Linux distribution, such as:

  * Ubuntu 22.04 LTS (Jammy Jellyfish) as host system
  * Oracle Virtual Box - Version 7.1.6 r167084 (Qt6.5.3) and
  * Debian 12 (Bookworm) inside Virtual Box.

Further:
  * Docker Engine version 23.0.6 ef23cbc
    (build with API version 1.42, containerd v1.7.27, runc v1.2.5, docker-init v 0.19.0)
  *  Node.js v20.19.2 with npm v10.8.2 and corepack v0.31.0
  * curl 7.88.1
  * gedit v44.2
  * git v2.39.5

### Essential System Updates

It's crucial to update your system's package lists and upgrade existing packages to ensure you have the latest security patches and software versions.

```bash
sudo apt update
sudo apt upgrade -y
sudo apt autoremove -y
```

### Install Git

Git (version control system) is required to clone the repository and manage code versions.

```bash
sudo apt install git -y
```

**Installed Git Version Example:** `git version 2.39.5` (Your version may vary)

### Install Node.js (v20+) and Corepack

The frontend application requires Node.js v20 or higher, along with Corepack for `pnpm`.

```bash
# Add Node.js PPA (Personal Package Archive)
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -

# Install Node.js
sudo apt install nodejs -y

# Verify Node.js version
node -v # Expected: v20.x.x or higher (e.g., v23.0.6)

# Enable Corepack (for pnpm)
corepack enable
corepack -v # Expected: a version number (e.g., v0.31.0)

# Verify pnpm version
pnpm -v # Expected: a version number (e.g., v10.8.2)
```

### Install Docker Engine

Docker is used to containerize and run the Nginx web server with the frontend application.

```bash
# Install necessary packages
sudo apt install ca-certificates curl gnupg -y
curl -v # Expected: a version number (e.g., 7.88.1)

# Add Docker's official GPG key
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Add Docker repository to Apt sources
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Update Apt package index
sudo apt update

# Install Docker Engine, containerd, and Docker Buildx (CLI plugin)
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin -y

# Verify Docker installation
sudo docker run hello-world
```

If the `hello-world` command runs successfully, Docker is installed correctly.

check with command:
docker -v # Expected: a version number

**Installed Docker Version Example:**
```
Client: Docker Engine - Community
 Version:           23.0.6 
 API version:       1.42
 ...
Server: Docker Engine - Community
 Engine:
  Version:          version 23.0.6
  API version:      1.42 (minimum version 1.24)
  ...
```

(Your version may vary, but ensure it's a recent stable release.)

### Configure Docker Permissions

To avoid using `sudo` with every Docker command, add your user to the `docker` group.

```bash
sudo usermod -aG docker $USER
```

**Important:** You need to log out and log back in (or reboot your system) for this change to take effect. After re-logging in, you should be able to run `docker` commands without `sudo`.

### Install Gedit (Graphical Text Editor)

Gedit is a user-friendly graphical text editor that will be used for editing configuration files outside the Docker container.

```bash
sudo apt install gedit -y
gedit -v # Expected: a version number (e.g., v44.2)

```
## 2.1.  Installation of Iroha V2 Blockchain ledger
... see full documentation [here](https://docs.iroha.tech/get-started/)

## 2.2\.  Installation of Iroha BLock Explorer (backend, version 0.3.0)
... see documentation [here](https://github.com/soramitsu/iroha2-block-explorer-backend)

## 2.3\. Repository Setup and Version Freezing

This section guides you through getting the Iroha project files for the Web UI (frontend) onto your machine and crucially, "freezing" the code to a specific, verified version to ensure a stable and reproducible test environment. This is especially important for open-source projects with independent development streams.

### Clone the Repository

Clone the project repository from GitHub.

```bash
# common structure: git clone https://github.com/[YourGitHubUsername]/[your-repository-name].git
git clone https://github.com/DigitClopedia2024/iroha2-block-explorer-web.git```

**Replace `[YourGitHubUsername]/[your-repository-name].git` with the actual path to the official repository.**
```bash
git clone https://github.com/soramitsu/iroha2-block-explorer-web.git```

### Verify and Freeze Repository Version (Commit Hash Check)

IMPORTANT: After cloning, it's critical to verify the integrity and exact version of the downloaded files. We will "freeze" your local repository to a specific "commit hash" to ensure consistency, especially in test environments. This prevents unintended updates from affecting your installation.

1.  **Get the Desired Commit Hash from GitHub:**
    Go to your GitHub repository in your web browser. Navigate to the "Code" tab, then click on "commits" (e.g., `https://github.com/soramitsu/iroha2-block-explorer-web/commits/develop/ ). Identify the specific commit you wish to use for this installation (e.g., a release version or a known stable point). Click on the commit message to view its details, and copy the full SHA-1 hash (e.g., 7dc9f916cc9250e158e96d0a7f633135a2627c12`).

    **Example:** Let's assume you've chosen commit hash `7dc9f916cc9250e158e96d0a7f633135a2627c12` for this installation.

2.  **Navigate to the Cloned Repository:**

    ```bash
    cd [your-repository-name] # Replace with your actual repository name
    ```

3.  **Verify Local Commit Hash:**
    First, confirm that your local repository has the desired commit:

    ```bash
    git log -1 --pretty=format:"%H"
    ```

    IMPORTANT: Compare this output with the desired commit hash you copied from GitHub. They should match if you just cloned the repository. If not, you might need to fetch and pull or identify why your local branch isn't aligned.

4.  **"Freeze" Your Repository to the Specific Commit Hash:**
    To ensure your working directory remains at this specific commit, even if the `main` branch moves forward, you can checkout that commit directly. This puts you in a "detached HEAD" state, which is fine for a stable test installation.

    ```bash
    git checkout 7dc9f916cc9250e158e96d0a7f633135a2627c12
    ```

    **Replace `7dc9f916cc9250e158e96d0a7f633135a2627c12` with the actual commit hash you want to use.**

    You will see a message indicating you are in a 'detached HEAD' state. This is expected and desirable for "freezing" the installation to a specific version. Any new commits will not automatically affect your current working directory.

### Navigate to Project Directory

Ensure you are in the root directory of your cloned and "frozen" repository.

```bash
cd [your-repository-name] # Replace with your actual repository name (if you are not already there)
```

## 3\. Building the Frontend Application

Before creating the Docker image, you need to build the frontend application using the specific Node.js and pnpm versions installed.

```bash
corepack enable
pnpm i
pnpm build
```
The `build` command will generate the static web files in the `dist` directory. This `dist` directory will then be served by the Nginx web server inside the Docker container:

```bash
sudo pnpm build

> iroha2-block-explorer-web@ build /home/username/Iroha-Projects/RC20/webexplorer
> run-s typecheck build:vite

> iroha2-block-explorer-web@ typecheck /home/username/Iroha-Projects/RC20/webexplorer
> vue-tsc --noEmit

> iroha2-block-explorer-web@ build:vite /home/username/Iroha-Projects/RC20/webexplorer
> vite build

vite v6.3.3 building for production...
✓ 566 modules transformed.
dist/index.html                                                               1.39 kB │ gzip:   0.56 kB
dist/_assets/iroha-logo-DBCdre8L.png                                          7.93 kB
dist/_assets/JetBrainsMono-Regular-DBdzmLf9.woff2                            43.84 kB
dist/_assets/JetBrainsMono-Regular-BZfyC4dZ.woff                             59.37 kB
dist/_assets/Sora-VariableFont_wght-BUZ7hlw5.ttf                            110.22 kB
dist/_assets/TransactionsList-D6u8QSzJ.css                                    0.04 kB │ gzip:   0.06 kB
dist/_assets/ContextTooltip-C_OcJ8rL.css                                      0.50 kB │ gzip:   0.30 kB
dist/_assets/NFTDetails-ClKOyxuB.css                                          0.70 kB │ gzip:   0.31 kB
dist/_assets/AccountsList-Da2juLCf.css                                        0.70 kB │ gzip:   0.33 kB
dist/_assets/DomainsList-BNAdIotV.css                                         0.71 kB │ gzip:   0.32 kB
dist/_assets/BaseContentBlock-DZvlfUH1.css                                    0.79 kB │ gzip:   0.39 kB
dist/_assets/useAdaptiveHash-BM71XkJ1.css                                     0.91 kB │ gzip:   0.45 kB
dist/_assets/AssetsList-BMUWifZ7.css                                          0.99 kB │ gzip:   0.36 kB
dist/_assets/BlocksList-FPu7VKXX.css                                          1.25 kB │ gzip:   0.44 kB
dist/_assets/BaseTabs-Bml_haX7.css                                            1.36 kB │ gzip:   0.53 kB
dist/_assets/BlockDetails-xACyv2SY.css                                        1.46 kB │ gzip:   0.57 kB
dist/_assets/TransactionDetails-GBMssuu0.css                                  1.56 kB │ gzip:   0.45 kB
dist/_assets/TransactionStatus-RFTuyGYE.css                                   1.93 kB │ gzip:   0.64 kB
dist/_assets/TransactionsTable-CsjLGFI3.css                                   2.20 kB │ gzip:   0.61 kB
dist/_assets/AssetDetails-DRxerhPp.css                                        2.73 kB │ gzip:   0.61 kB
dist/_assets/InstructionsTable-CGkYmn44.css                                   2.98 kB │ gzip:   0.74 kB
dist/_assets/DataField-DG1xBfYz.css                                           3.27 kB │ gzip:   1.09 kB
dist/_assets/BaseTable-BoPKY7qS.css                                           3.56 kB │ gzip:   0.94 kB
dist/_assets/DomainDetails-DpsUgzO3.css                                       3.76 kB │ gzip:   0.76 kB
dist/_assets/AccountDetails-DhGRhnx_.css                                      4.84 kB │ gzip:   0.79 kB
dist/_assets/HomePage-DvpP01k2.css                                            6.65 kB │ gzip:   1.47 kB
dist/_assets/index-CwSiI2GB.css                                              17.61 kB │ gzip:   3.35 kB
dist/_assets/json-emIcYs4Y.js                                                 0.07 kB │ gzip:   0.09 kB
dist/_assets/tiny-invariant-CopsF_GD.js                                       0.08 kB │ gzip:   0.10 kB
dist/_assets/model-D1QfSY9v.js                                                0.11 kB │ gzip:   0.10 kB
dist/_assets/NotFound-Bwelor54.js                                             0.27 kB │ gzip:   0.23 kB
dist/_assets/ContextTooltip-CxXk8nL1.js                                       0.30 kB │ gzip:   0.24 kB
dist/_assets/arrows-chevron-left-rounded-24-M0d8cD2a.js                       0.51 kB │ gzip:   0.34 kB
dist/_assets/BaseContentBlock.vue_vue_type_style_index_0_lang-DdTrtGZx.js     0.56 kB │ gzip:   0.34 kB
dist/_assets/index-CNAxw2Bg.js                                                0.69 kB │ gzip:   0.43 kB
dist/_assets/TransactionsList-BWVk0Tlf.js                                     0.89 kB │ gzip:   0.47 kB
dist/_assets/TimeStamp.vue_vue_type_script_setup_true_lang-_mAKuTl6.js        1.17 kB │ gzip:   0.63 kB
dist/_assets/NFTDetails-C3i-v-1E.js                                           1.42 kB │ gzip:   0.73 kB
dist/_assets/BaseTabs.vue_vue_type_script_setup_true_lang-Bql1rnj7.js         1.58 kB │ gzip:   0.81 kB
dist/_assets/clock-DaHaAVc2.js                                                1.77 kB │ gzip:   0.93 kB
dist/_assets/AccountsList-Cr2_hT60.js                                         2.65 kB │ gzip:   1.03 kB
dist/_assets/DomainsList-wpk9tztW.js                                          2.83 kB │ gzip:   1.03 kB
dist/_assets/TransactionsTable.vue_vue_type_style_index_0_lang-BtgR77IB.js    3.16 kB │ gzip:   1.30 kB
dist/_assets/TransactionStatus.vue_vue_type_style_index_0_lang-CLTUnlR0.js    3.22 kB │ gzip:   1.44 kB
dist/_assets/BlocksList-1j7eC90N.js                                           3.51 kB │ gzip:   1.25 kB
dist/_assets/BlockDetails-CsKXevsE.js                                         3.69 kB │ gzip:   1.38 kB
dist/_assets/TransactionDetails-Jy_1n_aD.js                                   4.05 kB │ gzip:   1.44 kB
dist/_assets/AssetsList-CY22xwP4.js                                           4.76 kB │ gzip:   1.48 kB
dist/_assets/AssetDetails-D51o5y_q.js                                         4.77 kB │ gzip:   1.55 kB
dist/_assets/InstructionsTable.vue_vue_type_style_index_0_lang-BM6xo4cL.js    5.52 kB │ gzip:   2.00 kB
dist/_assets/DomainDetails-BbY3AN44.js                                        7.14 kB │ gzip:   2.10 kB
dist/_assets/AccountDetails-CCKcS0FI.js                                       8.90 kB │ gzip:   2.37 kB
dist/_assets/BaseTable.vue_vue_type_style_index_0_lang-DRtbDqqK.js            9.17 kB │ gzip:   3.26 kB
dist/_assets/DataField.vue_vue_type_style_index_0_lang-A2qhIOKQ.js           17.51 kB │ gzip:   5.95 kB
dist/_assets/HomePage-hHwa0ohO.js                                            18.27 kB │ gzip:   7.03 kB
dist/_assets/time-BaQwkfRZ.js                                                26.93 kB │ gzip:   7.95 kB
dist/_assets/index-DJetIDQB.js                                              189.92 kB │ gzip:  69.41 kB
dist/_assets/useAdaptiveHash-aQXDxELX.js                                    630.85 kB │ gzip: 219.87 kB

(!) Some chunks are larger than 500 kB after minification. Consider:
- Using dynamic import() to code-split the application
- Use build.rollupOptions.output.manualChunks to improve chunking: https://rollupjs.org/configuration-options/#output-manualchunks
- Adjust chunk size limit for this warning via build.chunkSizeWarningLimit.
✓ built in 29.67s
```
## 4\. Docker Configuration and Deployment

This section details how to build and initially run the application using Docker.

### Understanding the Configuration Files

  * **`Dockerfile`**: This file contains instructions for building a Docker image. It sets up the Nginx server and copies your built frontend application into the Nginx web root.
  * **`nginx.conf`**: This file, provided in the repository, contains a general configuration for Nginx. **However, for this specific setup, the Nginx configuration *inside* the Docker container (typically `/etc/nginx/conf.d/default.conf` or `/etc/nginx/nginx.conf`) will be the primary file to modify for port routing.** The provided `nginx.conf` might serve as a reference or a base to be copied into the container later.
  * **`.config` (Dummy Configuration File)**: This file is provided as a dummy example for potential future configurations.

HINT: It's contents are not directly used in our specific Docker setup for Nginx port configuration. Instead we use the default.config file inside the Docker container after building the docker image !

### Building the Docker Image

Now, build the Docker image for your application. This process compiles the `Dockerfile` and includes your built frontend artifacts. The general command for a frozen repository version:

```bash
docker build -t [your-app-name]:frozen-$(git rev-parse HEAD | cut -c1-8) .
```
Yet we used for our installation following command:
```bash
sudo docker build -t iroha2-explorer-web .
```
(Important: Don't forget the "dot" at the end of the command line. The "." denotes the current directory as the build context, ensuring Docker has access to all necessary files during the build.)

**Explanation of the image tag:**

  * `-t [your-app-name]:frozen-$(git rev-parse HEAD | cut -c1-8)`: This command tags your Docker image with a descriptive name and includes a short version of the current commit hash (e.g., `my-vue-frontend:frozen-abcdef12`). This ensures that your Docker image is directly linked to the "frozen" code version you are using.
  * `.`: Signifies that the Dockerfile is in the current directory.

This command "docker build -t iroha2-explorer-web ." will take some time as it downloads the base Nginx image and copies your files. You will see output indicating each step of the build process.

```bash
[+] Building 34.4s (7/7) FINISHED                                                               docker:default
 => [internal] load build definition from Dockerfile                                                      1.6s
 => => transferring dockerfile: 123B                                                                      0.0s
 => [internal] load .dockerignore                                                                         1.3s
 => => transferring context: 109B                                                                         0.0s
 => [internal] load metadata for docker.io/nginxinc/nginx-unprivileged:mainline                           4.6s
 => [internal] load build context                                                                         2.6s
 => => transferring context: 1.50MB                                                                       1.0s
 => [1/2] FROM docker.io/nginxinc/nginx-unprivileged:mainline@sha256:773b6546272c808baf4dd5f8da71f61c56  22.6s
 => => resolve docker.io/nginxinc/nginx-unprivileged:mainline@sha256:773b6546272c808baf4dd5f8da71f61c561  1.9s
 => => sha256:773b6546272c808baf4dd5f8da71f61c561adba04b1c627883d5c1da67e1f1ef 6.01kB / 6.01kB            0.0s
 => => sha256:fbc39be54dde25a96aa87a453bcc61051a2ffb87a54cba5151881f4c62da4826 2.42kB / 2.42kB            0.0s
 => => sha256:a3a72b0e140c50cff79f215500b301057cf91348d61ba1dac0e284fff1c7badb 10.68kB / 10.68kB          0.0s
 => => sha256:665998f75b4e48d5b2385739b76837a5dd3e3fe723acaff9ccaca5306dd6d949 44.15MB / 44.15MB          8.0s
 => => sha256:4b59edf3bdf550a391a2fb0133e58eaa957fd707169ee368bcf455dd2ac8c9e7 2.75kB / 2.75kB            1.7s
 => => sha256:855796cf3e3dd025990d479a33841ae553ecc5b092cced4720ad035052f7960d 626B / 626B                2.0s
 => => sha256:4ee4459af13224f7539370cd8fc2196b13af915fb1cfbfd9c7ac770bd52582f6 956B / 956B                2.4s
 => => sha256:794af71d9353881b24b8ea1db65350e2d728b0d1b48655ab88ed274ff72429d5 404B / 404B                2.7s
 => => sha256:e91aeda96c013c4f0ac5b47e764b6e8d425d04c0f1989459394a60af2f91d7e4 1.20kB / 1.20kB            3.7s
 => => sha256:52d59448d16ffe900455b9ab4d6ca2ce8173bbdb5531ac87eb2fdc4da04a8aae 1.40kB / 1.40kB            3.9s
 => => extracting sha256:665998f75b4e48d5b2385739b76837a5dd3e3fe723acaff9ccaca5306dd6d949                 1.1s
 => => extracting sha256:4b59edf3bdf550a391a2fb0133e58eaa957fd707169ee368bcf455dd2ac8c9e7                 0.0s
 => => extracting sha256:855796cf3e3dd025990d479a33841ae553ecc5b092cced4720ad035052f7960d                 0.0s
 => => extracting sha256:4ee4459af13224f7539370cd8fc2196b13af915fb1cfbfd9c7ac770bd52582f6                 0.0s
 => => extracting sha256:794af71d9353881b24b8ea1db65350e2d728b0d1b48655ab88ed274ff72429d5                 0.0s
 => => extracting sha256:e91aeda96c013c4f0ac5b47e764b6e8d425d04c0f1989459394a60af2f91d7e4                 0.0s
 => => extracting sha256:52d59448d16ffe900455b9ab4d6ca2ce8173bbdb5531ac87eb2fdc4da04a8aae                 0.0s
 => [2/2] COPY ./dist /usr/share/nginx/html                                                               3.2s
 => exporting to image                                                                                    1.9s
 => => exporting layers                                                                                   1.6s
 => => writing image sha256:214790ac33a2eb3107de7a0c2e32d57401e2e431eba0c06004070c0b5be027e8              0.1s
 => => naming to docker.io/library/iroha2-explorer-web
```

### Initial Container Deployment & 1st start of the Docker Cointainer (`docker run`)

Once the image is built, you can run a container from it. 

1. We'll map a host port (e.g., `8085`) to the standard Nginx port inside the container (e.g., `80`). take the container name from the build process (naming to docker.io/library/...), here "iroha2-explorer-web":

```bash
docker run -d -p 127.0.0.1:8085:80 --name [your-container-name] [your-app-name]:frozen-$(git rev-parse HEAD | cut -c1-8)
```
As Docker container runs also the nodes for the Iroha V2 blockchain, you can check with following command docker ps command inside the IrohaV2 Blockchain ./defaults sub directory the different port addresses:

```bash
cd /path/to/IrohaV2/defaults/directory/
docker ps
```
You will see the list with the running cointainers:
```bash
CONTAINER ID   IMAGE                   COMMAND                   CREATED      STATUS                        PORTS                                                                                  NAMES
e2764a522050   hyperledger/iroha:dev   "irohad"                  2 days ago   Up About a minute (healthy)   0.0.0.0:1340->1340/tcp, :::1340->1340/tcp, 0.0.0.0:8083->8083/tcp, :::8083->8083/tcp   defaults-irohad3-1
de644beb9cf0   hyperledger/iroha:dev   "irohad"                  2 days ago   Up About a minute (healthy)   0.0.0.0:1338->1338/tcp, :::1338->1338/tcp, 0.0.0.0:8081->8081/tcp, :::8081->8081/tcp   defaults-irohad1-1
89465582a220   hyperledger/iroha:dev   "irohad"                  2 days ago   Up About a minute (healthy)   0.0.0.0:1339->1339/tcp, :::1339->1339/tcp, 0.0.0.0:8082->8082/tcp, :::8082->8082/tcp   defaults-irohad2-1
3dd57b04a8a5   hyperledger/iroha:dev   "/bin/bash -c '\n    …"   2 days ago   Up About a minute (healthy)   0.0.0.0:1337->1337/tcp, :::1337->1337/tcp, 0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   defaults-irohad0-1
```
At port 8080 the blockchain is listening with the API to offer the backend (Block Explorer) the records (e.g. assets, domains, transactions etc ...). Port 8081 till 8083 are the 3 nodes which are being installed by default.

!! Warning: Avoid using latest tag for starting the docker image. The latest tag (= latest commit hash version) can change over time, leading to unexpected updates !!

We advice to ensure stability and prevent unintended updates from the upstream repository, it's crucial to tag your Docker images appropriately. Tag the image with the specific commit hash of your cloned version !

To prevent Docker from automatically pulling the latest image from the repository use this specific tag. Generally: always build and tag your images with unique identifiers (e.g., commit hashes or version numbers).

```bash
docker tag [your-container-name] [your-app-name]:[Commit hash]
```
(a) For our installation we took the command:
```bash
sudo docker tag iroha2-explorer-web iroha2-explorer-web:68572a7
```
(Rec.: See upper commit hash check via the command git log -1 --pretty=format:"%H")

This tags your image with the specific commit hash (e.g. 68572a7), ensuring you can always refer to this exact version.

(b) Alternatively we tried to give it a different name within our project folder structure, e.g. 
```bash
sudo docker tag webexplorer iroha2-explorer-web:68572a7 
```
(Hint: This naming yet didn't work, so we went back to (a))

!! Build, tag, and run the Docker image with the commit hash to ensure a stable and reproducible environment !!

3. Run the Docker container:

This command runs the container with the specified tag, ensuring consistency across deployments.

sudo docker run -d -p 8085:80 --name webexplorer iroha2-explorer-web:68572a7 

Hint: Rename into webexplorer didnt work, so we chose:)
```bash
sudo docker run -d -p 8085:80 --name iroha2-explorer-web iroha2-explorer-web:68572a7
```
Let's break down this command:

  * `-d`: Runs the container in "detached" mode (in the background).
  * `-p 127.0.0.1:8085:80`: This is the port mapping.
      * `127.0.0.1`: This means the application will only be accessible from your local machine (localhost).
      * `8085`: This is the port on your host machine that you will use to access the application.
      * `80`: This is the standard port that Nginx listens on *inside* the Docker container by default.
  * `--name [your-container-name]`: Assigns a human-readable name to your container (e.g., `my-frontend-container`). This makes it easier to manage.
  * `[your-app-name]:frozen-$(git rev-parse HEAD | cut -c1-8)`: Specifies the image to use, linking it to your "frozen" commit.

(Hint: Later then we simply used the command to start the application / container again:) 
```bash
sudo docker start iroha2-explorer-web
```
### Verifying the Running Container

To confirm that your Docker container is running, use the `docker ps` command:

```bash
docker ps
```
You will see a long string of characters (the container ID) if the command executes successfully.
You should see output similar to this, confirming your container is up and running:

```bash
CONTAINER ID   IMAGE                                     COMMAND                  CREATED         STATUS         PORTS                      NAMES
a1b2c3d4e5f6   my-vue-frontend:frozen-abcdef12           "nginx -g 'daemon of…"   5 seconds ago   Up 4 seconds   127.0.0.1:8085->80/tcp     my-frontend-container
```

  * **`CONTAINER ID`**: The unique identifier for your running container. Make a note of this if you need to reference it later.
  * **`IMAGE`**: The Docker image used to create the container, now clearly showing the frozen commit hash.
  * **`COMMAND`**: The command executed inside the container.
  * **`STATUS`**: Should be `Up` followed by a duration.
  * **`PORTS`**: Shows the port mapping (e.g., `127.0.0.1:8085->80/tcp`).
  * **`NAMES`**: The name you assigned to your container.

### Accessing the Web UI

If the Web UI is up successfully by starting the container, you should have access to via browser locally with http://127.0.0.1:8085 (or any other port address you have decided for.) - Yet at this stage, Nginx inside the container is likely listening on port `80`, but its default configuration might not correctly serve your Vue.js application or route to your backend. 

You will need to complete the Nginx configuration steps first below before the web UI is fully functional.

## 5\. Configuring Nginx Inside the Docker Container

The Nginx server running inside the Docker container requires specific configuration to correctly serve the frontend and handle API routing. This process involves exporting its default configuration, editing it on your host machine, importing it back into the container, and restarting Nginx.

**Important Note:** Modifying files directly inside a running container is generally discouraged for production environments, as changes are lost if the container is removed. For a robust setup, these configurations should ideally be baked into the `Dockerfile` using `COPY` commands. However, for the purpose of a clear "dummy-friendly" installation guide that explains manual intervention, we will proceed with this method.

### Identifying the Nginx Configuration File

The primary Nginx configuration file within the container is typically located at `/etc/nginx/conf.d/default.conf` or `/etc/nginx/nginx.conf`. We will focus on `default.conf` as it's common for serving web applications.

1.  **Get the Container ID or Name:**
    If you don't already know it, find your container's ID or name:

    ```bash
    docker ps
    ```

    (e.g., `my-frontend-container` or `a1b2c3d4e5f6`)

2.  **Access the Container Shell (as root):**
    First, verify the path of the Nginx configuration file inside the container. We'll use `docker exec` to run commands inside the container.

    ```bash
    docker exec -it [your-container-name] /bin/bash
    ```

    You are now inside the container. You'll see a prompt like `root@[container-id]:/app#`.

    **Check Nginx configuration paths:**

    ```bash
    ls /etc/nginx/conf.d/
    ls /etc/nginx/
    ```

    You'll likely find `default.conf` in the sub directory `/etc/nginx/conf.d/`. This is the file we need to modify. We will do this outside the container for safety issues to avoid any overwriting by typos.

    **Exit the container shell:**

    ```bash
    exit
    ```

### Exporting the Default Nginx Configuration

Now, we'll copy the `default.conf` file from the running container to your host machine. This allows you to edit it with a graphical editor like Gedit.

```bash
docker cp [your-container-name]:/etc/nginx/conf.d/default.conf ./default.conf
```

This command copies `default.conf` from the container's `/etc/nginx/conf.d/` directory to your current directory on the host machine. It should be the main directory you have build the Web UI Block explorer (e.g.  /home/username/Iroha-Projects/RC20/webexplorer/... )

### Editing the Configuration File with Gedit

Open the copied `default.conf` file using Gedit on your host machine in the terminal.

```bash
sudo gedit default.conf
```

**Key Modifications to `default.conf`:**

1.  **Listen Port:** Ensure Nginx listens on the correct port *inside the container*. Given your `docker run -p 127.0.0.1:8085:80` command, Nginx needs to listen on `80`. and also add the port address the backend (Block explorer) is listening, e.g. at 127.0.0.:4123 port

```nginx

server {
    listen       80; # change Nginx listens on port 80 instead of port 8080 (default) inside the container
    server_name  localhost;

    #access_log  /var/log/nginx/host.access.log  main;

    # Frontend SPA routing
    location / {
        root   /usr/share/nginx/html; # This is where the Dockerfile copies your built dist files
        index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # Backend API Proxy (Example)
    # Adjust this to match your backend's actual address and port
    location /api/v1/ {
       proxy_pass http://127.0.0.1:4123/api/v1/; # Proxy requests to your backend API
       proxy_set_header Host $host;
       proxy_set_header X-Real-IP $remote_addr;
       proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
       proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
      * **`listen 80;`**: This confirms Nginx is listening on port 80 inside the container.
      * **`root /usr/share/nginx/html;`**: This is the standard location where Nginx serves static files in a default Nginx Docker image. Your `Dockerfile` should be copying your `dist` folder contents to this location.
      * **`location / { ... }`**: This block ensures that all frontend routes are handled by serving `index.html` (crucial for Single Page Applications like Vue.js).
      * **`location /api/v1/ { ... }`**: This is an example of how to proxy requests from your frontend (`/api/v1/`) to your separate backend API. **You MUST adjust `http://127.0.0.1:4123/api/v1/` to the actual address and port of your backend API server.** If your backend is also in Docker, consider using Docker networking to allow container-to-container communication by container name instead of `127.0.0.1` (which would refer to the frontend container itself). For a simple test, ensure your backend is accessible at this address from within the container's network context.

Save the `default.conf` file in Gedit and close it. And return back to your terminal.

### Importing the Modified Configuration

Now, copy the modified `default.conf` file back into the container, overwriting the original via terminal command:

```bash
docker cp ./default.conf [your-container-name]:/etc/nginx/conf.d/default.conf
```

### Restarting Nginx Inside the Container

For the new Nginx configuration to take effect, you must restart the Nginx service *inside* the container.

1.  **Access the Container Shell (as root):**

    ```bash
    docker exec -it [your-container-name] /bin/bash
    ```

2.  **Test Nginx Configuration (Optional but Recommended):**
    Before restarting, you can test the configuration syntax for errors:

    ```bash
    nginx -t
    ```

    You should see `syntax is ok` and `test is successful`. If there are errors, go back and edit `default.conf` on your host, re-copy, and re-test.

3.  **Restart Nginx Service:**

    ```bash
    nginx -s reload
    # OR if reload doesn't work (less common for Nginx in Docker)
    # service nginx restart
    ```

4.  **Exit the container shell:**

    ```bash
    exit
    ```

### Final Verification

Now, open your web browser and navigate to:
```
http://127.0.0.1:8085 #(or use the port address you have chosen during the "docker run ..." command)
```
Your frontend application should now load correctly, and any API calls to `/api/v1/` from the frontend should be proxied to your configured backend.

## 6\. Important for managing Docker Containers (`run`, `stop`, `start`, `rm`, `up`, `down` commands)

Understanding the different Docker commands for managing containers is crucial.

  * **`docker run`**:

      * **Purpose**: This command is used to **create and start a *new* container** from an image.
      * **When to use**:
          * The very first time you deploy your application.
          * If you have stopped and *removed* a container using `docker rm`.
          * If you need to start a fresh instance of the application with potentially different parameters (e.g., different port mappings, different volume mounts).
      * **Behavior**: It will create a new container instance, assign it a new ID, and start it.

  * **`docker stop [container-name/id]`**:

      * **Purpose**: Gracefully stops a running container. The container still exists but is not running.
      * **When to use**: When you want to temporarily shut down your application without deleting its configuration or state.
      * **Behavior**: Sends a `SIGTERM` signal to the container's main process, allowing it to shut down gracefully. If it doesn't shut down within a timeout, it sends a `SIGKILL`.

  * **`docker start [container-name/id]`**:

      * **Purpose**: Starts a stopped container.
      * **When to use**: After you have used `docker stop` and want to resume the container's operation without re-creating it.
      * **Behavior**: Resumes the execution of an existing, stopped container. It retains its previous configuration, including any persistent data (if volumes are used).

  * **`docker rm [container-name/id]`**:

      * **Purpose**: Removes a stopped container.
      * **When to use**: When you no longer need a specific container instance, or if you need to clean up resources before creating a new one with the same name.
      * **Behavior**: Deletes the container and all its associated data (unless volumes are explicitly managed and retained). You cannot `start` a container after it has been `rm`'d.

  * **Note on `docker compose up` and `docker compose down`**:

      * These commands are part of **Docker Compose**, which is used to define and run multi-container Docker applications. Your current setup uses single `docker` commands.
      * `docker compose up`: Builds (if necessary), creates, and starts services defined in a `docker-compose.yml` file.
      * `docker compose down`: Stops and removes containers, networks, and volumes created by `docker compose up`.
      * **This guide does not use `docker compose`, so these commands are not directly applicable to the described installation.** If you were to adopt Docker Compose in the future, these commands would be central to your workflow.

**In summary for this guide:**

  * Use `docker run` **once** for initial deployment or after `docker rm`.
  * Use `docker stop` to temporarily halt your application.
  * Use `docker start` to resume your application after stopping it.
  * Use `docker rm` to completely remove a container instance.

## 7\. Post-Installation Checks and Troubleshooting

### Checking Container Logs

If you encounter issues, the first place to look is the Docker container logs. This will show any output from the Nginx server or the application itself.

```bash
docker logs [your-container-name]
```

**Replace `[your-container-name]` with the actual name of your container (e.g., `my-frontend-container`) or its `CONTAINER ID`.**

### Common Issues

  * **Port already in use**: If you get an error like `Error starting userland proxy: listen tcp 127.0.0.1:8085: bind: address already in use`, it means another process on your machine is already using host port `8085`.
      * **Solution**: Change the host port in the `docker run` command (e.g., `-p 127.0.0.1:8086:80`). Remember to access the application on the new port (e.g., `http://127.0.0.1:8086` and check in advance after starting the Iroha2 blockchain with the "docker ps" command the docker containers and relevant port addresses of the blockchain and default nodes).
  * **`dist` directory not found**: Ensure you have successfully run `pnpm build` and that the `dist` directory exists in your project root before building the Docker image.
  * **`docker build` or `docker run` errors**: Carefully read the error messages in your terminal. They often provide clues about what went wrong. Check for typos in commands or incorrect file paths.
  * **Application not loading in browser or API calls failing**:
      * Verify the Docker container is running (`docker ps`).
      * Check your browser's developer console (F12) for any JavaScript errors or network request failures.
      * Ensure the port mapping is correct (`-p host_port:container_port`).
      * Most importantly, double-check your `default.conf` Nginx configuration for correct `listen` directives, `root` paths, and especially the `proxy_pass` URL for your backend API.
      * Check Nginx logs inside the container:
        ```bash
        docker exec [your-container-name] cat /var/log/nginx/error.log
        docker exec [your-container-name] cat /var/log/nginx/access.log
        ```
        These logs are crucial for debugging Nginx configuration issues.

## 8\. Backup and Maintenance

### Committing Changes

If you make any local changes to the project files (e.g., `.config`, `nginx.conf`, or even the modified `default.conf` on your host machine for future reference), remember to commit them to your Git repository to track your changes and allow for easy restoration.

```bash
git add .
git commit -m "Description of your changes"
git push origin main # Or your default branch
```

### Creating a Backup

To create a full backup of your project folder, including your code, `node_modules`, and built `dist` directory:

```bash
tar -czvf [your-project-name]-backup-$(date +%Y%m%d_%H%M%S).tar.gz [your-project-name]/
```

**Replace `[your-project-name]` with the actual name of your project directory.** This command creates a gzipped tar archive.

## 9\. For Developers

This section provides additional details for developers who might need to customize or debug the application.

### Frontend API Requests

By default, the frontend application sends API requests to its own base URL (`<base>/api/v1`). When served via Vite (for development), it is automatically proxied to `localhost:4000`.

  * **During Development (Vite)**: The `vite.config.mts` file controls the proxy settings for API requests. You can modify the proxy target there if your backend API is running on a different address or port during development.

### Backend API Communication

The frontend application is designed to communicate with a separately designed backend via a backend API port address, for example, `127.0.0.1:4123` (see documentation via upper link to the Block Explorer).

  * **Configuration**: The specific backend API URL that the frontend uses is likely configured within the frontend's environment variables or a dedicated configuration file (e.g., `.env` files or a JavaScript config file). Refer to the frontend's source code for details on how this backend URL is set and used.
  * **Docker Networking**: When your backend is also running in a Docker container, `127.0.0.1` inside the frontend container refers to the frontend container itself. To communicate with another Docker container (your backend), you should use Docker's internal networking. This often involves:
    1.  Creating a custom Docker network: `docker network create my-app-network`
    2.  Running both your frontend and backend containers on this network, referencing them by their container names (e.g., `docker run --network my-app-network --name my-backend-container my-backend-image ...` and `docker run --network my-app-network --name my-frontend-container my-frontend-image ...`).
    3.  Configuring the frontend's `proxy_pass` in Nginx to point to the backend container's name (e.g., `proxy_pass http://my-backend-container:4123/api/v1/;`).

This advanced Docker networking configuration ensures robust and scalable communication between your frontend and backend services within a Dockerized environment.
