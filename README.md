# Organiation Site Structure

# Requirements
* ansible  (on remote machine)
* ubuntu 16.04 - 20.04
* atleast 1gb ram (if enough users, then more)

# Site directory structure

```
/srv
    /log
        /SERVICENAME
    /containers
        /SERVICENAME
    /bin
        /SERVICENAME
```

## Containers
The main site functionality is provided by the following containers

* mailcow
* grav
* discourse
* static

### Container Storage
All containers that require external volumes have 'data' dirs in their respective directories.

## Binaries
The processes not containerized are as follows

* caddy

## Logs
Currently there are 2 logs rotated of 50mb size per container

## HTTPS
All TLS/HTTP is handled automatically with caddy and lets encrypt