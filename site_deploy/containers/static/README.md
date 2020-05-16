# Static Site (nginx)

This container serves the static website data held in /srv/site_deploy/containers/static/data.

If you have updated the content of your static site and want to reload the docker container please do the following.
*NOTE* this has not be automated due to actually removing production software, it is best to do this manually

# Reloading site content
## Remove existing container
* Find the container that is running
```
$ sudo docker container list | grep static.site
CONTAINER ID        IMAGE                COMMAND                  CREATED             STATUS              PORTS                  NAMES
2bd0730eca50        static.site:latest   "nginx -g 'daemon of…"   0 minutes ago       Up 0 minutes        0.0.0.0:8088->80/tcp   dockers_witty_name_goes_here
```

* Remove the container
```
$ sudo docker rm -f 2bd0730eca50
2bd0730eca50
```

## Recreate container
* Run the following ansible script to redeploy
```
$ ansible-playbook -i hosts.yml static-site-rebuild.yml
```