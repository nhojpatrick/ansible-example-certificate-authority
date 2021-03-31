# Certificate Authority Tutorial

Covers creating a basic certificate authority for learning and training.

Uses Vagrant to create and configure Virtual Box VM's, then Ansible to configure those VM's.

## 1. Overview

### Hosts
  1. Touchdown
  Used to configure cahost.

  1. cahost
  Main vm we focus on, looking at creating our own CA (certificate authority).

## 2. Setup Your ssh keys
```
$ ssh-keygen -f `pwd`/provisioning/.ssh/id_rsa
```

NOTE: Press enter to skip have a password for the ssh key

## 3. Create Env

The cahost is configured to auto boot, so a simple `vagrant up` will boot.

```
$ vagrant up
```

## 4. Create Touchdown

The touchdown server is not configured to auto boot, so needs to be explictly started.

```
$ vagrant up touchdown
```

## 5. Configure CA

```
$ vagrant ssh touchdown
vagrant@touchdown:~$ cd /vagrant/setup/
vagrant@touchdown:/vagrant/setup$ ansible-galaxy collection install community.crypto
vagrant@touchdown:/vagrant/setup$ ansible-playbook -i hosts playbook.yaml
vagrant@touchdown:/vagrant/setup$
```
