BackupPC Ansible role
=====================

[![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-HanXHX.backuppc-blue.svg)](https://galaxy.ansible.com/HanXHX/backuppc) [![Build Status](https://travis-ci.org/HanXHX/ansible-backuppc.svg?branch=master)](https://travis-ci.org/HanXHX/ansible-nginx) 

This role installs and configures Backuppc. It works on Debian Jessie. It can work on Ubuntu or other Debian-based systems. But no direct support will be added (PR accepted).

Description
------------

This role has two functionalities:
  * install backuppc (in the file hosts add the backuppc_group)
  * add machine to the backups (in the file hosts add the backup_group)

Requirements
------------

None.

Note
----

At this time, this role only manage backup via rsync (+ ssh) method.

Role Variables
--------------

### Role var

- `backuppc_server_name`: fqdn of the backup server
- `backuppc_ssh_key_bits`: set length of ssh key pair (optional, default 2048 by Ansible)
- `backuppc_fetch_ssh_key`: copy backkupc ssh key from server (boolean)
- `backuppc_local_fetch_dir`: local dir where you fetch backuppc SSH public key
- `backuppc_language`: web interface language
- `backuppc_Date_Format`: date format of the web interface
- `mysql_user`: user from mysql
- `backuppc_group`: group of the inventory that contains the backuppc
- `backup_group`: inventory group that contains the containers to be backed


### Client vars

Each client configuration override global configuration.

- `users_backuppc`: Users with access to the backuppc web interface 
- `state`: (O) absent or present (default)
- `include_files:`: (O) default files (directories) list to backup.
- `exclude_files:`: (O) default files (directories) list to exclude in backup
- `xfermethod`: (O) transfer method (rsync as default)
- `more`: (O) hash with specific key/value (usefull for custom directives)
- `backup_mysql_dump`: determines if database backup is done (true/false)
- `backup_config_mysql`: run the installation process (true/false)
- `backup_local_users`: users who manage the backup ("user1,user2")

(O): Optional


### Global configuration

You should [RTFM](http://backuppc.sourceforge.net/faq/BackupPC.html) for these variables:

- `backuppc_ServerPort`
- `backuppc_ServerMesgSecret`
- `backuppc_MaxBackups`
- `backuppc_MaxUserBackups`
- `backuppc_MaxBackupPCNightlyJobs`
- `backuppc_BackupPCNightlyPeriod`
- `backuppc_MaxOldLogFiles`
- `backuppc_FullPeriod`
- `backuppc_IncrPeriod`
- `backuppc_PingMaxMsec`
- `backuppc_RsyncClientCmd`
- `backuppc_RsyncClientRestoreCmd`
- `backuppc_BackupFilesOnly`
- `backuppc_BackupFilesExclude`
- `backuppc_XferLogLevel`
- `backuppc_language`

Other global configuration can be managed (you can create issues or PR).

Notes
-----

### About HTTP

This role doesn't manage any webserver! You _must_ use another role to install a HTTP service _before_ this role. It doesn't manage any authentication.

Be careful, in Debian based systems, backuppc depends [httpd virtual package](https://packages.debian.org/jessie/httpd). If you don't install any HTTP server, Debian will install Apache2. However, you can use [my Nginx role](https://github.com/HanXHX/ansible-nginx) which is compatible with this role.

### About backups

This role downloads backuppc SSH public key. You must deploy it on each clients!

Dependencies
------------

None.

Example Playbook
----------------

### Quick and dirty

You should look at [defaults/main.yml](defaults/main.yml).

### With my Nginx role

```
- hosts: backup
  vars:
    nginx_vhosts:
      - name: backup.mydomain.tld
        template: _backuppc
		htpasswd: backuppc
    nginx_htpasswd:
      - name: backuppc
        description: 'Please login'
        users:
          - name: hx
            password: myPassword
    backuppc_hosts:
      - hostname: current-host
        state: present
      - hostname: removed_host
      	state: absent
      - hostname: oldserver
  roles:
    - HanXHX.nginx
    - HanXHX.backuppc
```

### With Apache2

```
  The management of the users is done by the role and the users have their assigned passwords which can be changed with:
  htpasswd /etc/backuppc/htpasswd usuario
```
 

License
-------

GPLv2

Donation
--------

If this code helped you, or if you’ve used them for your projects, feel free to buy me some :beers:

- Bitcoin: `1BQwhBeszzWbUTyK4aUyq3SRg7rBSHcEQn`
- Ethereum: `63abe6b2648fd892816d87a31e3d9d4365a737b5`
- Litecoin: `LeNDw34zQLX84VvhCGADNvHMEgb5QyFXyD`
- Monero: `45wbf7VdQAZS5EWUrPhen7Wo4hy7Pa7c7ZBdaWQSRowtd3CZ5vpVw5nTPphTuqVQrnYZC72FXDYyfP31uJmfSQ6qRXFy3bQ`

No crypto-currency? :star: the project is also a way of saying thank you! :sunglasses:

Author Information
------------------

- Twitter: [@hanxhx_](https://twitter.com/hanxhx_)
