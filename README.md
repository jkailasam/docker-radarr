# Radarr - A fork of Sonarr to work with movies à la Couchpotato. 

[![radarr](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/radarr.png)][appurl]

## Usage

```
docker create \
  --name=radarr \
	-v <path to data>:/config \
	-v <path to data>:/downloads \
	-v <path to data>:/movies \
	-v /etc/localtime:/etc/localtime:ro \
	-e TZ=<timezone> \
	-e PGID=<gid> -e PUID=<uid>  \
	-p 7878:7878 \
  jeeva420/radarr
```

## Parameters

`The parameters are split into two halves, separated by a colon, the left hand side representing the host and the right the container side. 
For example with a port -p external:internal - what this shows is the port mapping from internal to external of the container.
So -p 8080:80 would expose port 80 from inside the container to be accessible from the host's IP on port 8080
http://192.168.x.x:8080 would show you what's running INSIDE the container on port 80.`


* `-p 7878` - the port(s)
* `-v /config` - Radarr Application Data
* `-v /downloads` - Downloads Folder
* `-v /movies` - Movie Share
* `-v /etc/localtime` for timesync - see [Localtime](#localtime) for important information
* `-e TZ` for timezone information, Europe/London - see [Localtime](#localtime) for important information
* `-e PGID` for for GroupID - see below for explanation
* `-e PUID` for for UserID - see below for explanation

It is based on alpine linux with s6 overlay, for shell access whilst the container is running do `docker exec -it radarr /bin/bash`.

## Localtime

It is important that you either set `-v /etc/localtime:/etc/localtime:ro` or the TZ variable, mono will throw exceptions without one of them set.

### User / Group Identifiers

Sometimes when using data volumes (`-v` flags) permissions issues can arise between the host OS and the container. We avoid this issue by allowing you to specify the user `PUID` and group `PGID`. Ensure the data volume directory on the host is owned by the same user you specify and it will "just work" ™.

In this instance `PUID=1001` and `PGID=1001`. To find yours use `id user` as below:

```
  $ id <dockeruser>
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

## Setting up the application

Access the webui at `<your-ip>:7878`, for more information check out [Radarr][appurl].

## Info

* Shell access whilst the container is running: `docker exec -it radarr /bin/bash`
* To monitor the logs of the container in realtime: `docker logs -f radarr`

* container version number 

`docker inspect -f '{{ index .Config.Labels "build_version" }}' radarr`

* image version number

`docker inspect -f '{{ index .Config.Labels "build_version" }}' jeeva420/radarr`
