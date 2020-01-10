FROM ubuntu:bionic as dreamland

# Install build and dev tools, and dependency libraries.
RUN apt-get update
RUN apt-get update --fix-missing
RUN apt-get install -y git g++ gcc make automake libtool bison flex telnet vim bzip2
RUN apt-get install -y libcrypto++-dev libjsoncpp-dev libdb5.3 libdb5.3-dev libdb5.3++ libdb5.3++-dev zlib1g zlib1g-dev libssl-dev libfl-dev

# Setup home folder
ENV HOME=/home/dreamland

# Download and build server code
RUN mkdir ${HOME} && \
cd ${HOME} && \
git clone https://github.com/dreamland-mud/dreamland_code.git && \
cd dreamland_code && \
make -f Makefile.git && \
mkdir ${HOME}/objs && \
cd ${HOME}/objs && \
../dreamland_code/configure --prefix=${HOME}/runtime && \
ls -la /home/dreamland/runtime/var/db/fenia && \
make -j 8 && make install

# Download and install configuration files and areas
RUN cd ${HOME} && \
git clone https://github.com/dreamland-mud/dreamland_world.git && \
ln -s ${HOME}/dreamland_world ${HOME}/runtime/share/DL

# Expose all MUD ports that listen to external connections.
EXPOSE 9000
EXPOSE 9001
EXPOSE 1234

# Run DreamLand on startup, with logs in stdout.
CMD cd /home/dreamland/runtime && ./bin/dreamland etc/dreamland.xml
