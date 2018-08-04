FROM ubuntu:xenial as dreamland

# Install build and dev tools, and dependency libraries.
RUN apt-get update
RUN apt-get install -y git g++ gcc make automake libtool bison flex telnet cron
RUN apt-get install -y libcrypto++-dev libjsoncpp-dev libdb5.3 libdb5.3-dev libdb5.3++ libdb5.3++-dev zlib1g zlib1g-dev libssl-dev

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
make -j 8 && make install

# Download and install configuration files and areas
RUN cd ${HOME} && \
git clone https://github.com/dreamland-mud/dreamland_world.git && \
ln -s ${HOME}/dreamland_world ${HOME}/runtime/share/DL

# Configure startup script to try and run every minute
ENV CRON="*/1    *       *   *   *   (cd /home/dreamland/runtime && ./bin/startup-local 2>&1 > /dev/null)"
RUN echo ${CRON} | crontab -

# Enable external client connections
EXPOSE 9000
EXPOSE 9127
EXPOSE 1234
