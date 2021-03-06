# dreamland_docker

Dockerfile repository for maintaining DreamLand MUD Docker images.
All these container images are published under dreamland username on the Docker Hub, at
> https://hub.docker.com/r/dreamland/

## Local: DreamLand MUD instance for development

This image contains a fully functioning game server for DreamLand MUD, with limited number of areas (zones). 
Source code and objs files are also included, allowing to quickly modify and recompile required files.

You can either download a ready-to-run dev image from Docker Hub (~2Gb), or build one yourself using Dockerfile in this repository.

Steps used in Dockerfile to create the image:
* This image is based on Ubuntu Bionic 18.04.
* Download and compile latest code from https://github.com/dreamland-mud/dreamland_code.
* Download latest configuration files and a set of areas for local development from https://github.com/dreamland-mud/dreamland_world.
* Set up game server ready to run on ports 9000, 9001, 9127 and Web Socket port 1234. 
* Game server will be launched on container startup

Steps you need to build it locally:
* Clone this repository and
```bash
cd local
docker build -t dreamland/local .
```

* Launch game server container, calling it 'dreamland'. Host port 8001 is going to be mapped to the game port 9001. 
```bash
docker run --name dreamland -p 8001:9001 -it dreamland/local
```
Instead of mapping each port individually, you can use -P option, that will expose MUD ports 9001 (backdoor), 1234 (WebSocket) and 9000 (normal greeting) to the host machine. 

* Connect to the game, user Kadm for Russian config, Cadmus for English config. Password: KadmKadm. 
```bash
telnet localhost 8001
```
For example, to use KOI8-R encoding, simply input: 1 Cadmus KadmKadm and hit Enter.

That's it. If you want to get a shell access to the running container:
```bash
docker exec -it dreamland /bin/bash
```
All MUD files are installed under /home/dreamland.

To restart a stopped container and continue your work:
```bash
docker restart dreamland
```


