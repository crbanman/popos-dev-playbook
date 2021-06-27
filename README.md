# Pop!_OS Developer Playbook

This playbook installs most of the software that I use on my dev machine for web and software development.

## Assumptions

Meant to be used on a install of Pop!_OS 20.10, but should work on existing systems (be sure to configure only what you need). It will probably work on Ubuntu, but that hasn't been tested so your mileage may vary.

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
git clone git@github.com:crbanman/popos-dev-playbook.git
cd popos-dev-playbook
```

Install dependencies:

```console
ansible-galaxy install -r requirements.yml
```

## Usage

1. Make a copy of `default.config.yml` with the name `config.yml` and change the configurations you want to use.

2. Run the playbook with the command and enter your user account password when prompted:

```console
   ansible-playbook main.yml --ask-become-pass
```

## Author

This project was created by [Cody Banman](https://github.com/crbanman) (originally inspired by [geerlingguy/mac-dev-playbook](https://github.com/geerlingguy/mac-dev-playbook) and [staticdev/linux-developer-playbook](https://github.com/staticdev/linux-developer-playbook))
