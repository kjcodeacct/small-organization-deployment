
## Restart grav and static site
```sh
$ sudo docker container list | grep static.site
$ sudo docker container list | grep grav
$ docker container restart CONTAINER _container_id_goes_here
```
## Restart Mailcow & Discourse
```sh
$ docker-compose down
$ docker-compose up -d
```