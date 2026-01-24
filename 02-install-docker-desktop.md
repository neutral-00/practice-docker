# Install Docker Desktop

## Reference
> https://docs.docker.com/desktop/setup/install/windows-install/

## Pre-requsite Checks
1. [x] wsl 2.1.5 or later | I have(wsl --version) `2.6.3`
2. [x] 4 GB of RAM
3. [x] Enable Hardware virtualization in BIOS
    - check Windows Features and check
    - Virtual Machine Platform and Windows Subsystem for Linux are checked

## Installation
1. Download the msi from https://docs.docker.com/desktop/setup/install/windows-install/
2. Open cmd in the Downloads folder and run
```
start /w "" "Docker Desktop Installer.exe" install --installation-dir=D:\ProgramData\Docker
```
3. Follow the installation wizard and complete the installation
