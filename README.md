# noVNC Display Container
```
```
This image is intended to be used for displaying X11 applications from other containers in a browser. A stand-alone demo as well as a [Version 3](https://docs.docker.com/compose/compose-file/#version-3) composition.

The [original image](https://github.com/theasp/docker-novnc) for use by the [Wisconsin Autonomous](https://wa.wisc.edu) student organization.

## Image Contents

* [Xvfb](http://www.x.org/releases/X11R7.6/doc/man/man1/Xvfb.1.xhtml) - X11 in a virtual framebuffer
* [x11vnc](http://www.karlrunge.com/x11vnc/) - A VNC server that scrapes the above X11 server
* [noNVC](https://kanaka.github.io/noVNC/) - A HTML5 canvas vnc viewer
* [Fluxbox](http://www.fluxbox.org/) - a small window manager
* [xterm](http://invisible-island.net/xterm/) - to demo that it works
* [supervisord](http://supervisord.org) - to keep it all running

## Usage

### Password

**The password for the vnc container is `wa`!!!!** VNC cannot be run password-less.

### Stand-alone Demo

Run:
```bash
$ docker run --rm -it -p 8080:8080 -p 5900:5900 wiscauto/vnc
```
Open a browser and see the desktop at `http://<server>:8080/`. If you're running this locally, `<server>` will most likely be `localhost`. Further, you can also access the VNC instance using a vnc viewer at `http://<server>:5900`.

### V3 Composition
A version of the V3 docker-compose example is shown below to illustrate how this image can be used to greatly simplify the use of X11 applications in other containers. With just `docker-compose up -d`, your favorite IDE can be accessed via a browser.

Some notable features:
* An `x11` network is defined to link the IDE and novnc containers
* The IDE `DISPLAY` environment variable is set using the novnc container name
* The screen size is adjustable to suit your preferences via environment variables
* The only exposed port is for HTTP browser connections

```
version: '3'
services:
  ide:
    image: psharkey/intellij:latest
    environment:
      - DISPLAY=novnc:0.0
    depends_on:
      - novnc
    networks:
      - x11
  novnc:
    image: wiscauto/vnc:latest
    ports:
      - "8080:8080"
			- "5900:5900"
    networks:
      - x11
networks:
  x11:
```

**If the IDE fails to start simply run `docker-compose restart <container-name>`.**

## On DockerHub / GitHub
___
* DockerHub [wiscauto/vnc](https://hub.docker.com/r/wiscauto/vnc/)
* GitHub [wiscauto/docker-vnc](https://github.com/wisconsinautonomous/docker-novnc)

# Thanks
___
This is based on the alpine container by @psharkey: https://github.com/psharkey/docker/tree/master/novnc
Based on [wine-x11-novnc-docker](https://github.com/solarkennedy/wine-x11-novnc-docker) and [octave-x11-novnc-docker](https://hub.docker.com/r/epflsti/octave-x11-novnc-docker/).
