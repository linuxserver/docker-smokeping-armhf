[linuxserverurl]: https://linuxserver.io
[forumurl]: https://forum.linuxserver.io
[ircurl]: https://www.linuxserver.io/irc/
[podcasturl]: https://www.linuxserver.io/podcast/
[appurl]: http://oss.oetiker.ch/smokeping/
[hub]: https://hub.docker.com/r/lsioarmhf/smokeping/

THIS IMAGE IS DEPRECATED. PLEASE USE THE MULTI-ARCH IMAGES AT `linuxserver/smokeping`

[![linuxserver.io](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/linuxserver_medium.png)][linuxserverurl]

The [LinuxServer.io][linuxserverurl] team brings you another container release featuring easy user mapping and community support. Find us for support at:
* [forum.linuxserver.io][forumurl]
* [IRC][ircurl] on freenode at `#linuxserver.io`
* [Podcast][podcasturl] covers everything to do with getting the most from your Linux Server plus a focus on all things Docker and containerisation!

# lsioarmhf/smokeping
[![](https://images.microbadger.com/badges/version/lsioarmhf/smokeping.svg)](https://microbadger.com/images/lsioarmhf/smokeping "Get your own version badge on microbadger.com")[![](https://images.microbadger.com/badges/image/lsioarmhf/smokeping.svg)](https://microbadger.com/images/lsioarmhf/smokeping "Get your own image badge on microbadger.com")[![Docker Pulls](https://img.shields.io/docker/pulls/lsioarmhf/smokeping.svg)][hub][![Docker Stars](https://img.shields.io/docker/stars/lsioarmhf/smokeping.svg)][hub][![Build Status](https://ci.linuxserver.io/buildStatus/icon?job=Docker-Builders/armhf/armhf-smokeping)](https://ci.linuxserver.io/job/Docker-Builders/job/armhf/job/armhf-smokeping/)

Smokeping keeps track of your network latency. For a full example of what this application is capable of visit [UCDavis](http://smokeping.ucdavis.edu/cgi-bin/smokeping.fcgi).


[![smokeping](http://oss.oetiker.ch/smokeping/inc/smokeping-logo.png)][appurl]

## Usage

```
docker create \
      --name smokeping \
      -p 80:80 \
       -e PUID=<UID> -e PGID=<GID> \
       -e TZ=<timezone> \
       -v <path/to/smokeping/data>:/data \
       -v <path/to/smokeping/config>:/config \
       lsioarmhf/smokeping
```

## Parameters

`The parameters are split into two halves, separated by a colon, the left hand side representing the host and the right the container side. 
For example with a port -p external:internal - what this shows is the port mapping from internal to external of the container.
So -p 8080:80 would expose port 80 from inside the container to be accessible from the host's IP on port 8080
http://192.168.x.x:8080 would show you what's running INSIDE the container on port 80.`


* `-p 80` - the port for the webUI
* `-v /data` - Storage location for db and application data (graphs etc)
* `-v /config` - Configure the `Targets` file here
* `-e PGID` for for GroupID - see below for explanation
* `-e PUID` for for UserID - see below for explanation
* `-e TZ` for timezone setting, eg Europe/London

This container is based on alpine linux (armhf) with s6 overlay, for shell access whilst the container is running do `docker exec -it smokeping /bin/bash`.

### User / Group Identifiers

Sometimes when using data volumes (`-v` flags) permissions issues can arise between the host OS and the container. We avoid this issue by allowing you to specify the user `PUID` and group `PGID`. Ensure the data volume directory on the host is owned by the same user you specify and it will "just work" <sup>TM</sup>.

In this instance `PUID=1001` and `PGID=1001`. To find yours use `id user` as below:

```
  $ id dockeruser
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

## Setting up the application
`IMPORTANT... THIS IS THE ARMHF VERSION`

Once running the URL will be `http://<host-ip>:80/smokeping/smokeping.cgi`.

Basics are, edit the Targets file to ping the hosts you're interested in to match the format found there.
Wait 10 minutes.

## Info

* To monitor the logs of the container in realtime `docker logs -f smokeping`.

* container version number 

`docker inspect -f '{{ index .Config.Labels "build_version" }}' smokeping`

* image version number

`docker inspect -f '{{ index .Config.Labels "build_version" }}' lsioarmhf/smokeping`


**Versions**

+ **20.11.18:** This image is deprecated. Please use the multi-arch images at linuxserver/smokeping
+ **09.04.18:** Add bc package.
+ **08.04.18:** Add tccping script and tcptraceroute package (thanks rcarmo).
+ **26.01.18:** Rebase to alpine 3.7, expose httpd.conf.
+ **30.05.17:** Rebase to alpine 3.6.
+ **07.05.17:** Expose smokeping.conf in /config/site-confs to allow user customisations
+ **12.04.17:** Fix cropper.js path, thanks nibbledeez.
+ **09.02.17:** Rebase to alpine 3.5.
+ **17.10.16:** Add ttf-dejavu package as per [LT forum](http://lime-technology.com/forum/index.php?topic=43602.msg507875#msg507875).
+ **14.10.16:** Add version layer information.
+ **11.09.16:** Add layer badges to README.
+ **05.09.16:** Add badges to README and add curl package.
+ **25.07.16:** First version.
