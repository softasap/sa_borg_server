sa_borg_server
==============

[![Build Status](https://travis-ci.com/softasap/sa_borg_server.svg?branch=master)](https://travis-ci.com/softasap/sa_borg_server)

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
  - libssl-dev
  - openssl
  - libacl1-dev
  - libacl1
  - build-essential
  - libfuse-dev
  - fuse
  - pkg-config
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
