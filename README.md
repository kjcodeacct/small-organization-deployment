# Organiation Site Structure

# NOTE 
This playbook and guide assumes the end user (you) are running ubuntu both on your host, and server.
With the latest ansible version

# Requirements
* ansible  (on local machine)
* sshpass  (on local machine)
* ubuntu 16.04 - 20.04 (tested with 20.04)
* atleast 1gb ram (if enough users, then more)
* smtp relay service

# Requirement Installation
## Install Ansible
```sh
$ sudo apt install software-properties-common
$ sudo apt-add-repository ppa:ansible/ansible
$ sudo apt update
$ sudo apt install ansible
```

## Install SSHPass
```sh
$ sudo apt install sshpass
```

# Pre Deployment
Modify vars/default.yml
* set smtp_server to existing smtp relay service
    * recommend <https://mailjet.com> - free up to 200 emails per day
* modify any variable changes needed to match domain

Create and store credentials for your 'sudo' user (ie non root user)

Modify DNS records as needed

# Deployment
install base of applications and services with the following
```sh
$ ansible-playbook deploy.yml -i hosts --ask-become-pass -u root --become --ask-pass
```

run applications and services as *NON* root user created in deploy.yml, this example is 'admin' user
```sh
$ ansible-playbook run.yml -i hosts --ask-become-pass -u admin --become --ask-pass
```

if any static site changes need to occur, copy static site to '/srv/site_deploy/containers/static/data'
and run
```sh
$ ansible-playbook static-site-rebuild.yml -i hosts --ask-become-pass -u admin --become --ask-pass
```


# Site directory structure
```
/srv
    /site_deploy
        /log
            /SERVICENAME
        /containers
            /SERVICENAME
                /data
        /bin
            /SERVICENAME
        /template
            /SERVICENAME
```

## Containers
The main site functionality is provided by the following containers

* mailcow
* grav
* discourse
* static html (nginx)

### Container Storage
All containers that require external volumes have 'data' dirs in their respective directories.

## Binaries
The processes not containerized are as follows

* caddy

## Logs
Currently there are 2 logs rotated of 50mb size per container

## HTTPS
All TLS/HTTP is handled automatically with caddy and lets encrypt

## Security

* Fail2ban is installed by default
* Please copy any ssh key you want to /vars/default.yml under the ssh_pub_key variable
* Root SSH is disabled once secure.yml is ran

### Ports
* port 587 is open for for SMTP
* port 143 is open for IMAP

## Ansible Playbooks

### Vagrant Testing

There is an included Vagrant file for testing locally if you would like. To do so just run the following command in the repo base directory.
```sh
$ vagrant up
```

This will bring up a an ubuntu server 20.04 instance with 2gb ram, 2 virtual cores under the name 'ubu_server_test_2020'.
To ensure it has been created run the following
```sh
$ vagrant global-status
```

And you should see something similar to
```sh
id       name    provider   state    directory                                    
----------------------------------------------------------------------------------
e0c53d4  default virtualbox running  /home/you/small-organization-deployment
```

Ensure you have installed ansible locally on the vm for testing
```sh
$ sudo apt install ansible
```

To temporarily save (push) the state of the vm, run the following
```sh
$ vagrant snapshot push
```

To view pushed snapshos
```sh
$ vagrant snapshot list
```

And to 'pop' those changes, run the following (and not delete the old snapshot)
```sh
$ vagrant snapshot pop --no-delete
```

Run the ansible playbooks below, specifying the vagrant user
```sh
$ ansible-playbook deploy.yml -i hosts --ask-become-pass -u vagrant --become --ask-pass
$ ansible-playbook run.yml -i hosts --ask-become-pass -u vagrant --become --ask-pass
```

## Issues and Features
If there are any bugs with this, or the containers are in need of *severe* updates please put out a PR or create an issue.
Like wise if there are any new containers/services worth adding please create a PR or issue.