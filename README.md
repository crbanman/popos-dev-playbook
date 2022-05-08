# Pop!_OS Developer Playbook

This playbook installs most of the software that I use on my dev machine for web and software development.

## Assumptions

Meant to be used on a fresh install of Pop!_OS 21.10, but should work on existing installs of the OS (be sure to configure only what you need). It will probably work on Ubuntu if flatpak is installed, but that hasn't been tested so your mileage may vary.

## Prerequisites

You will need `pip` and `ansible` installed on your machine which can be done with the following commands:

```console
sudo apt install python3-pip
pip install --user ansible
export PATH=$PATH:~/.local/bin
```

## Installation

Download or clone this repository to your local drive.

```console
git clone https://github.com/crbanman/popos-dev-playbook.git
cd popos-dev-playbook
```

Install dependencies:

```console
ansible-galaxy install -r requirements.yml
```

## Usage

1. Make a copy of `default.config.yml` with the name `config.yml` and change the configurations you want to use.

1. Run the playbook with the command and enter your user account password when prompted:

   ```console
   ansible-playbook main.yml --ask-become-pass
   ```

1. Restart your machine.

## Manual changes

There are some things that I haven't been able to automate yet.

### Set node version

Download and set currently used `node` version with `nvm`. This should theoretically be handled by the playbook, but it doesn't.

```console
nvm install --lts
nvm use --lts
```

### Install yarn

Yarn can be installed once npm (from setting the node version).

```console
npm install --global yarn
```

### Register SSH Keys

Add ssh keys. For better or worse I backup my keys, so I need to copy them to `~/.ssh/` and add them to the `ssh-agent`.

1. Set the the appropriate file permissions for each key:

   ```command
   chmod 400 ~/.ssh/id_ed25519
   ```

1. Start the ssh agent

    ```command
    eval "$(ssh-agent -s)"
    ```

1. For each key run `ssh-add`

   ```command
   ssh-add ~/.ssh/id_ed25519
   ```

### Run JetBrains Toolbox

Toolbox needs to be run once from the command line so the system can register the AppImage.

```command
/opt/jetbrains-toolbox
```

### Docker registry cache

Add docker image caching to avoid hitting the docker pull limit.

**Note: This solution is probably overkill, but I have run into the limit a couple of times in the past when working on some docker stuff. Just signing in to docker hub will double your pulls from 100 pull to 200 pulls every 6 hours.**

1. Pull and run the registry image on port `6000`

   ```console
   docker run -d -p 6000:5000 \
   -e REGISTRY_PROXY_REMOTEURL=https://registry-1.docker.io \
   --restart always \
   --name registry registry:2
   ```

   Note: This container will automatically restart when docker is restarted (i.e. system bootup)

1. Create the file `/etc/docker/daemon.json`

   ```json
   {
     "registry-mirrors": ["http://localhost:6000"]
   }
   ```

1. Restart the docker service

   ```console
   sudo systemctl restart docker
   ```

1. Test that the mirror is working

   1. Pull an image you don't already have locally
      ```console
      docker pull node
      ```
   1. Curl the local registry
      ```console
      curl http://localhost:6000/v2/_catalog
      ```
      Expected result:
      ```console
      {"repositories":["library/node"]}
      ```

## Author

This project was created by [Cody Banman](https://github.com/crbanman) (originally inspired by [geerlingguy/mac-dev-playbook](https://github.com/geerlingguy/mac-dev-playbook) and [staticdev/linux-developer-playbook](https://github.com/staticdev/linux-developer-playbook))
