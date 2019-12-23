FROM ubuntu:beaver as dreamland_stripped

# Install required runtime libraries and clean up as much as possible. Still TODO.
RUN apt-get update && \
apt-get install -y libcrypto++9v5 libjsoncpp1 libdb5.3 libdb5.3++  zlib1g  libssl1.0.0 binutils libfl-dev && \
rm -rf /var/lib/apt/lists/* && \
rm -rf /var/log/*.log /var/log/*/*.log

# Copy runtime and configuration files from the full build image.
WORKDIR /home/dreamland
COPY --from=dreamland/local:latest /home/dreamland/runtime runtime
COPY --from=dreamland/local:latest /home/dreamland/dreamland_world dreamland_world

# More cleanup.
RUN cd runtime && \
for i in lib/*.so.0.0.0 libexec/plugins/*.so.0.0.0; do strip $i; done && \
apt-get remove -y binutils

# Run DreamLand on startup, with logs in stdout.
CMD cd /home/dreamland/runtime && ./bin/dreamland etc/dreamland.xml
