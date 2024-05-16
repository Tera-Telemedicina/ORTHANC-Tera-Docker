# A robust PACS configuration for ORTHANC

## Docker Installation

### Installing KVM

> Use this command for Intel processors:
```shell
$    modprobe kvm_intel
```
> Use this command for AMD processors:
```shell
$    modprobe kvm_amd
```

To check if the KVM modules are enabled, run:
```shell
$    lsmod | grep kvm
```

The output should be this:
```shell
$    kvm_amd 167936 0
     ccp 126976 1 kvm_amd
     kvm 1089536 1 kvm_amd
     irqbypass 16384 1 kvm
```

Use this code too:
```shell
$    ls -al /dev/kvm
```

The output should be this:
```shell
$    crw-rw----+ 1 root kvm 10, 232 mai 14 08:09 /dev/kvm
```

Add current user to kvm group
```shell
$    sudo usermod -aG kvm $USER
```

Download Docker:
[Direct download link for Docker version 4.24](https://desktop.docker.com/linux/main/amd64/docker-desktop-4.24.0-amd64.deb?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-linux-amd64&gl=1*1nbuhdq*_ga*MTU5MDg5Mjg5My4xNjk2NjEyNjE5*_ga_XJWPQMJYHQ*MTY5NjYxNzEwNC4yLjEuMTY5NjYxNzc5Ni42MC4wLjA)

### Preparing the machine
Install the terminal or update it
```shell
$    sudo apt install -y gnome-terminal
```

Remove old docker installations
```shell
$    sudo apt remove docker-desktop

$    rm -r $HOME/.docker/desktop

$    sudo rm /usr/local/bin/com.docker.cli

$    sudo apt purge docker-desktop
```

#### NEVER REPEAT THIS STEP!!!
**Once done successfully, it should not be repeated.**
```shell
$    sudo apt-get update

$    sudo apt-get install -y ca-certificates curl gnupg

$    sudo install -m 0755 -d /etc/apt/keyrings

$    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

$    sudo chmod a+r /etc/apt/keyrings/docker.gpg

    # Add the repository to Apt sources:
$    echo \ "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \ "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \ sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

$    sudo apt-get update
```

In this case, the file you downloaded at the beginning of this step.
It should be in the download folder.

```shell
$    cd ~/Downloads
```

install apt on your computer:
```shell
$    sudo apt-get install -y ./docker-desktop-<version>-<arch>.deb
```

Exit the folder (if you want)
```shell
$    cd ..
```

Make docker start automatically when the computer turns on (if you want)
```shell
$    systemctl --user start docker-desktop
```

Press **Accept**

Either option will cause an error.
Either to create an account or log in with one that already exists.

### Time to create a GPG key:

How to create a GPG key:

```shell
$    gpg --generate-key
```

A CLI will start, asking for your full name, your email (in this case, corporate) and **then it will ask you to move the mouse and click on the keyboard**.

> Computers nowadays are very fast so I don't know if you will be able to help him enough. (Not that it's a problem.)

This is to generate entropy.

Your key will then print on the screen.

Your GPG Key is the large number in the result above your name and email.

Then you need to copy this key to use the command:

```shell
$    pass init <GPG Key>
```

Login to Docker Desktop should work perfectly.

To start when turning on Linux:
```shell
$    systemctl --user enable docker-desktop
```

## Configuring the project:

Create a `.env` file with the same keys as `.env.example`

Create the **tls** folder and container log folders
```shell
$    mkdir -p tls temp/orthancADM-logs-docker temp/orthancSHARE-logs-docker
```

Create a certificate in [Let's Encrypt](https://letsencrypt.org/) and place the .pem and .key file in the tls folder.

So just use 

```shell
$    docker compose up --build
```