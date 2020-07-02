BackupPC Ansible role
=====================

[![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-HanXHX.backuppc-blue.svg)](https://galaxy.ansible.com/HanXHX/backuppc) [![Build Status](https://travis-ci.org/HanXHX/ansible-backuppc.svg?branch=master)](https://travis-ci.org/HanXHX/ansible-nginx) 

This role installs and configures a Backuppc server. It works on Debian Jessie. It can work on Ubuntu or other Debian-based systems. But no direct support will be added (PR accepted).

Description
------------


This role installs and configures a Backuppc server, including the ssh key of the backuppc user and the passwords of web backuppc users. 

For the clients, there is also a [BackupPC client role](https://github.com/UdelaRInterior/ansible-backuppc-client/) that works with this
one to configure the the client itself as well as its account in the server configured here.  

Requirements
------------

None.

Note
----

At this time, this role only manage backup via rsync (+ ssh) method.

Role Variables
--------------

### Role var

- `backuppc_srv_server_name`: fqdn of the backup server
- `backuppc_srv_ssh_key_bits`: set length of ssh key pair (optional, default 2048 by Ansible)
- `backuppc_srv_fetch_ssh_key`: copy backkupc ssh key from server (boolean)
- `backuppc_srv_user_home`: local home dir of backuppc user (should be `/var/lib/backuppc`)
- `backuppc_srv_home_link`: if defined then `backuppc_srv_user_home` is a symbolic link to this folder

- `backuppc_srv_language`: web interface language
- `backuppc_srv_Date_Format`: date format of the web interface


### Client vars

Each client configuration overrides global configuration.

See [README of backuppc-client role](https://github.com/UdelaRInterior/ansible-backuppc-client/blob/master/README.md) for detailed client variables. 

- `users_backuppc`: Users with access to the backuppc web interface 
- `state`: (O) absent or present (default)
- `include_files:`: (O) default files (directories) list to backup.
- `exclude_files:`: (O) default files (directories) list to exclude in backup
- `xfermethod`: (O) transfer method (rsync as default)
- `more`: (O) hash with specific key/value (usefull for custom directives)

(O): Optional


### Global configuration

You should [RTFM](http://backuppc.sourceforge.net/faq/BackupPC.html) for these variables:

- `backuppc_srv_ServerPort`
- `backuppc_srv_ServerMesgSecret`
- `backuppc_srv_MaxBackups`
- `backuppc_srv_MaxUserBackups`
- `backuppc_srv_MaxBackupPCNightlyJobs`
- `backuppc_srv_BackupPCNightlyPeriod`
- `backuppc_srv_MaxOldLogFiles`
- `backuppc_srv_FullPeriod`
- `backuppc_srv_IncrPeriod`
- `backuppc_srv_PingMaxMsec`
- `backuppc_srv_RsyncClientCmd`
- `backuppc_srv_RsyncClientRestoreCmd`
- `backuppc_srv_BackupFilesOnly`
- `backuppc_srv_BackupFilesExclude`
- `backuppc_srv_XferLogLevel`
- `backuppc_srv_language`

Other global configuration can be managed (you can create issues or PR).

Notes
-----

### About HTTP

This role does now a basic installation of Apache, with htpasswd  authentication.


### About backups

This role generates a backuppc SSH public key. The backuppc_client role deploys it on each client.

Dependencies
------------

None.

Example Playbook
----------------

### Quick and dirty

You should look at [defaults/main.yml](defaults/main.yml).


```
  The management of the users is done by the role and the users have their assigned passwords which can be changed with:
  htpasswd /etc/backuppc/htpasswd user
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

Original role [Emilien M](https://github.com/HanXHX) enhanced by [Víctor Torterola and Daniel Viñar](https://github.com/UdelaRInterior)
