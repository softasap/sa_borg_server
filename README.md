sa_borg_server
==============

[![Build Status](https://travis-ci.com/softasap/sa_borg_server.svg?branch=master)](https://travis-ci.com/softasap/sa_borg_server)
[![Build Status](https://github.com/softasap/sa_borg_server/workflows/CI/badge.svg?event=push)](https://github.com/softasap/sa_borg_server/actions?query=workflow%3ACI)

Configures borg server per https://borgbackup.readthedocs.io/en/stable/deployment/central-backup-server.html

Example of usage:

Simple

```YAML
vars:
    box_borg_clients:
      - name: server_1
        key: "{{ lookup('file', '/path_to_pub_key/server_1.pub') }}"
      - name: server_2
        key: "{{ lookup('file', '/path_to_pub_key/server_2.pub') }}"
        append_only: true

roles:
     - {
         role: "sa_borg_server",
         borg_clients: "{{ box_borg_clients }}"
       }
```

Advanced

```YAML
vars:
    box_borg_clients:
      - name: server_1
        key: "{{ lookup('file', '/path_to_pub_key/server1_rsa.pub') }}"
      - name: server_2
        key: "{{ lookup('file', '/path_to_pub_key/server2_rsa.pub') }}"
        append_only: true

roles:
     - {
         role: "sa_borg_server",
         borg_clients: "{{ box_borg_clients }}"
       }
```

Side notes
----------

If for any reason you want to install package using python , you would need following deps:

```
  - python3
  - python3-dev
  - python3-pip
  - python-virtualenv
  - libffi-dev
  - libssl-dev
  - openssl
  - libacl1-dev
  - libacl1
  - build-essential
  - libfuse-dev
  - fuse
  - pkg-config
```

Borgmatic client install
------------------------

```sh
# Custom notification backend (optional) https://github.com/dschep/ntfy
pip3 install --upgrade ntfy[pid,emoji,xmpp,telegram,instapush,slack,rocketchat]

# Borgmatic itself
pip3 install --upgrade borgmatic
```

install client borg (recommended as self contained binary)

```sh
  sudo curl -sLo /usr/bin/borg https://github.com/borgbackup/borg/releases/download/1.1.15/borg-linux64
  sudo chmod +x /usr/bin/borg

```

curl -sLo ~/dotfiles/bin/borg https://github.com/borgbackup/borg/releases/download/1.1.15/borg-linux64
chmod +x ~/dotfiles/bin/borg

depending on your scenario you are about to install it either to some user (recommended),
or directly to root - if you are dealing with multiple user system, and do not want to deal
with granting access to "backup" group.

You can also install both tools locally for current user (i.e. including "--user") if needed.

Next step would be either render or generate config.

```sh
sudo generate-borgmatic-config --destination /etc/borgmatic/config.yaml
```

You may find yourself wanting to create different backup policies for different
applications on your system. For instance, you may want one backup configuration
for your database data directory, and a different configuration for your user
home directories.

The way to accomplish that is pretty simple: Create multiple separate configuration
files and place each one in a /etc/borgmatic.d/ directory. For instance:

```sh
sudo mkdir /etc/borgmatic.d
sudo generate-borgmatic-config --destination /etc/borgmatic.d/app1.yaml
sudo generate-borgmatic-config --destination /etc/borgmatic.d/app2.yaml
```

If you encrypt your Borg repository with a passphrase or a key file, you'll either
need to set the borgmatic encryption_passphrase configuration variable or set
the BORG_PASSPHRASE environment variable.

If you'd like to validate that your borgmatic configuration is valid, the
following command is available for that:

```sh
sudo validate-borgmatic-config -c path-to-config-folder/
```
This command's exit status ($? in Bash) is zero when configuration is valid and
non-zero otherwise.

Configuring ntfy:

```sh
   echo -e 'backends: ["pushover"]\npushover: {"user_key": "t0k3n"}' > ~/.ntfy.yml
```

or in extended form, ntfy config looks like

```yaml
---
backends:
    - pushover
pushover:
    user_key: hunter2
pushbullet:
    access_token: hunter2
simplepush:
    key: hunter2
slack:
    token: slacktoken
    recipient: "#slackchannel"
xmpp:
     jid: "user@gmail.com"
     password: "xxxx"
     mtype: "chat"
     recipient: "me@jit.si"
```

before running first backup, you need to initialize repo, kind of
(set enryption on for prod use)
```sh
borg init backup@46.137.224.247:/opt/borg/repos/rocketracoon/sample --encryption none
```

### Backing up postgres databases.

You would need client tools installed, like
`apt install postgresql-client-9.6` for ubuntu:

```sh
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | \
sudo apt-key add -

sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main" >> /etc/apt/sources.list.d/postgresql.list'

sudo apt update

sudo apt install postgresql-client-9.6
```

Usage with ansible galaxy workflow
----------------------------------

If you installed the `sa_borg_server` role using the command


`
   ansible-galaxy install softasap.sa_borg_server
`

the role will be available in the folder `softasap.sa_borg_server`
Please adjust the path accordingly.

```YAML

     - {
         role: "softasap.sa_borg_server"
       }

```




Copyright and license
---------------------

Code is dual licensed under the [BSD 3 clause] (https://opensource.org/licenses/BSD-3-Clause) and the [MIT License] (http://opensource.org/licenses/MIT). Choose the one that suits you best.

Reach us:

Subscribe for roles updates at [FB] (https://www.facebook.com/SoftAsap/)

Join gitter discussion channel at [Gitter](https://gitter.im/softasap)

Discover other roles at  http://www.softasap.com/roles/registry_generated.html

visit our blog at http://www.softasap.com/blog/archive.html
