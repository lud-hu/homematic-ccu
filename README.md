# homematic-ccu

This is a little docker container for providing a local server with the homematic ccu environment on a Raspberry Pi.

It is basically built on the hassio homematic addon: https://github.com/home-assistant/hassio-addons/tree/master/homematic

I've just built a docker image from the code and provide it with some information on how to run it. You can find it here: https://hub.docker.com/r/luhudroid/homematic-ccu

# How to use it

Just run the docker image with:

```bash
$ docker run -d \
    -v ~/MY_LOCAL_CONFIG_FOLDER:/data \
    --name=homematic-ccu \
    -p 2010:2010 \
    -p 2000:2000 \
    -p 2001:2001 \
    --device=/PATH/TO/MY/HARDWARE:/dev/ttyUSB0:rwm \
    luhudroid/homematic-ccu:1.0
```
* MY_LOCAL_CONF_FOLDER: Put your options.json (see this repo) here and configure which hardware should be used and where to find it. Configuration data will be saved here.
* /PATH/TO/MY/HARDWARE: Put your local path to your hardware here (for homematic rf and ip). Adapt the path after that to the path you've set in the config.json for the targeted device.

I'm using it with the HMIP-RFUSB stick from ELV, connected at /dev/ttyUSB0:
```bash
$ docker run -d \
    -v ~/homematic:/data \
    --name=homematic-ccu \
    -p 2010:2010 \
    -p 2000:2000 \
    -p 2001:2001 \
    --device=/dev/ttyUSB0:/dev/ttyUSB0:rwm \
    luhudroid/homematic-ccu:1.0
```

After the container is started up you can configure it and connect your devices with the Homematic Manager: https://github.com/hobbyquaker/homematic-manager

# How it's built

```
$ docker build \
    --tag=homematic-ccu \
    --build-arg BUILD_FROM=homeassistant/armhf-base-ubuntu:18.04 \
    --build-arg OCCU_VERSION=3.41.11 .
```