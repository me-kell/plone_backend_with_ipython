## Plone backend with IPython

Project structure:
```
.
├── compose.yaml
└── app
    ├── Dockerfile
    └── ipythonconsole
```

[compose.yaml](https://github.com/me-kell/plone_backend_with_ipython/blob/c60fbb67db27940e0a9d9fc901121b921d64d796/compose.yaml#L1-L21)
https://github.com/me-kell/plone_backend_with_ipython/blob/c60fbb67db27940e0a9d9fc901121b921d64d796/compose.yaml#L1-L21

## Deploy with docker compose

```console
$ docker compose up -d
[+] Running 1/1
...
[+] Building 0.0s (10/10) FINISHED                                           docker:default
...
[+] Running 4/4
 ✔ backend_with_ipython                      Built                                     0.0s 
 ✔ Volume "plone_backend_with_ipython_data"  Created                                   0.0s 
 ✔ Container zeo                             Started                                   0.2s 
 ✔ Container my_container                    Started                                   0.2s 
```

## Expected result

```console
$ docker compose ps
NAME           IMAGE                                  COMMAND                  SERVICE                CREATED         STATUS         PORTS
my_container   plone-server-dev-with-ipython:latest   "/app/docker-entrypo…"   backend_with_ipython   3 minutes ago   Up 3 minutes   0.0.0.0:8080->8080/tcp, [::]:8080->8080/tcp
zeo            plone/plone-zeo:6.0.0                  "/app/start-zeo.sh"      zeo                    3 minutes ago   Up 3 minutes   0.0.0.0:8100->8100/tcp, [::]:8100->8100/tcp
```

## Create Plone classic site

```console
$ docker exec -e SITE_ID="Plone" my_container ./docker-entrypoint.sh create-classic
INFO:Plone Site Creation:Creating a new Plone site  @ Plone
INFO:Plone Site Creation: - Using the classic distribution and answers from /app/scripts/default.json
INFO:Plone Site Creation: - Stopping site creation, as there is already a site with id Plone. Set DELETE_EXISTING=1 to delete the existing site before creating a new one.
INFO:Plone Site Creation: - Site created!
```

## Inspect the IP-addresses of the running containers

```bash
for i in $(docker container ls --format '{{.Names}}' -a);
    do echo "$i $(docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $i)";
done
```
```console
my_container 172.23.0.3
zeo 172.23.0.2
```
