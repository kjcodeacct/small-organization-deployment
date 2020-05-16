# Organiation Site Structure

# Requirements
* ansible  (on remote machine)
* ubuntu 16.04 - 20.04
* atleast 1gb ram (if enough users, then more)

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


### Vagrant Testing
Modify the 'deploy.yml' with the following to point to the local host
```yaml
- hosts: localhost
  connection: local
```
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

And to 'pop' those changes, run the following
```sh
$ vagrant snapshot pop
```


## Issues and Features
If there are any bugs with this, or the containers are in *severe* updates please put out a PR or create an issue.
Like wise if there are any new containers/services worth adding please create a PR or issue.