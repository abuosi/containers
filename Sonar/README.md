# Sonarqube for Development Environment

Objective is Deploy the Sonarqube in develop environment to validate your project code.

This project was based in [Sonarqube Documentation](https://docs.sonarqube.org).

## Prerequisites

- Windows 10 Professional;
- [Minimum configuration for Hyper-V](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/reference/hyper-v-requirements);
- [Minimum configuration for Docker](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install).

## Deploy Sonarqube Server

### 1. Install docker

Follow the steps in Docker Official Documentation.

[Get Started with Docker](https://www.docker.com/get-started)

### 2. Enable WSL2 in your Windows

Follow the steps in WSL Offcial Documentation.

[Windows Subsystem for Linux Installation Guide for Windows 10](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

### 3. Install Ubuntu in your WSL

Follow the steps to install Ubuntu distribuition.

[Install your Linux distribution of choice](https://docs.microsoft.com/en-us/windows/wsl/install-win10#step-6---install-your-linux-distribution-of-choice)

### 4. Adjust your Ubuntu Linux Environment 

Execute the commands bellow.

```sh
sysctl -w vm.max_map_count=262144
sysctl -w fs.file-max=65536
```
### 5. Clone the repository 

Clone the repository in your machine

```bash
git clone https://github.com/abuosi/sonar-docker.git
cd sonar-docker
```

### 6. Start the Sonarqube Server

Execute the command bellow

```bash
docker-compose up -d
```
### 7. Access your server 

Execute the command in your linux bash prompt to get your IP in WLS.

```bash
ip addr show eth0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}'
```
Open the browser to access the administration console using the URL bellow.

```html
http://<wls-ip>:9000
```

## How to use Sonnarqube Scanner

### 1. Execute Scanner for DotNet Applications

Install the Scanner Tool for dotnet

```bash
dotnet tool install --global dotnet-sonarscanner
```
Execute the scanner in project. It´s important execute this command in your project root directory.

```bash
dotnet sonarscanner begin /d:sonar.login=<username> /d:sonar.password=<password> /k:<project-name>

dotnet sonarscanner begin /d:sonar.login=<project-token> /k:<project-name>

dotnet build

dotnet sonarscanner end /d:sonar.login=<project-token>
```
### 2. Execute Scanner in Docker for all kind of projects  

Create the file `sonar-project.properties` in your project root directory.

```properties
# must be unique in a given SonarQube instance
sonar.projectKey=<project-token>

# --- optional properties ---

# defaults to project key
sonar.projectName=<project-name>
 
# Path is relative to the sonar-project.properties file. Defaults to .
#sonar.sources=.
 
# Encoding of the source code. Default is default system encoding
#sonar.sourceEncoding=UTF-8
```

Execute scanner image with command. 

```bash
docker run --rm -e SONAR_HOST_URL="<server-url>" -e SONAR_LOGIN="<project-token>" -v "<src-path>:/usr/src" sonarsource/sonar-scanner-cli
```

