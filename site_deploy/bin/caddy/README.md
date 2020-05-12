# Caddy

Version: 2.0 RC3 (final)
This is the web proxy for this server that handles reverse proxy handling, ssl, etc.

## Usage

All of this should be done via ansible, but if it needs to be done by hand.

Simple

```
$ caddy -conf /srv/cfg/caddy/Caddyfile
```

As a service
```
$ sudo cp /srv/site_deploy/bin/caddy/caddy.service /etc/systemd/system/
$ sudo systemctl daemon-reload
// to enable the new service
$ sudo systemctl enable caddy
// to start/restart the new service
$ sudo systemctl start/restart caddy
```