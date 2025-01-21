# Tech Train Installation Instructions

## 1. Create a New Devbox
1. Go to [https://devbox.microsoft.com/](https://devbox.microsoft.com/)
2. Press the "New dev box" button
    - Choose a name for the dev box and select one of the dev box pool machines
    - Wait for the dev box to start

## 2. Kubectl Install
1. Run PowerShell as admin and run the command:
   ```powershell
   wsl --update
   Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
   choco install kubernetes-cli -y
   cd ~; mkdir .kube; cd .kube; New-Item config -type file
    ```
2. Verify the installation by running the command: 
   ```powershell
   kubectl version --client
   ```

## 3. Ubuntu Install and Configuration
1. Download the Ubuntu app from the Microsoft Store - [Ubuntu](https://apps.microsoft.com/detail/9PDXGNCFSCZV?hl=en-us&gl=IL&ocid=pdpshare)
2. Launch the Ubuntu app, you will be asked to enter a username and password
3. Run the command:
   ```bash
   sudo apt update && sudo apt install net-tools -y
   [ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
   chmod +x ./kind
   sudo cp ./kind /usr/local/bin/kind
   rm -rf kind
   ```

## 4. Docker Desktop Install
1. Download [Docker Desktop](https://www.docker.com/products/docker-desktop)
2. Install Docker (Verify the "Use WSL 2" checkbox is checked)
3. Restart the devbox so changes could take effect