FROM ubuntu:xenial as dreamland_stripped

# Install required runtime libraries.
RUN apt-get update
RUN apt-get install -y libcrypto++ libjsoncpp1 libdb5.3 libdb5.3++  zlib1g  libssl1.0.0
# Clean up as much as possible. Still TODO.
RUN rm -rf /var/lib/apt/lists/* && \
rm -rf /var/log/*.log /var/log/*/*.log

# Copy runtime and configuration files from the full build image.
WORKDIR /home/dreamland
COPY --from=dreamland/local:latest /home/dreamland/runtime runtime
COPY --from=dreamland/local:latest /home/dreamland/dreamland_world dreamland_world

# Run DreamLand on startup, with logs in stdout.
CMD cd /home/dreamland/runtime && ./bin/dreamland etc/dreamland.xml
