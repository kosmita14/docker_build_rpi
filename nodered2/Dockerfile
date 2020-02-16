#FROM nodered/node-red-docker:rpi
FROM arm32v7/node:10-slim

USER root

#RUN apt-get update && apt-get upgrade && apt-get install -y git make g++ python net-tools
RUN apt-get update && apt-get install -y git make g++ python net-tools

#RUN apt-get update && apt-get install -y libudev-dev

#RUN wget https://github.com/OpenZWave/open-zwave/archive/v1.6.tar.gz && tar zxvf v1.6.tar.gz
RUN git clone https://github.com/OpenZWave/open-zwave.git
WORKDIR /open-zwave
#WORKDIR /open-zwave-1.6
RUN make && make install && ldconfig

#RUN ls -ltra /usr/local/etc

#RUN apt-get update && apt-get install -y \
#    libopenzwave1.5-dev

# Home directory for Node-RED application source code.
RUN mkdir -p /usr/src/node-red

# User data directory, contains flows, config and nodes.
RUN mkdir /data

WORKDIR /usr/src/node-red

# Add node-red user so we aren't running as root.
RUN useradd --home-dir /usr/src/node-red --no-create-home node-red \
    && chown -R node-red:node-red /data \
    && chown -R node-red:node-red /usr/src/node-red

USER node-red

# package.json contains Node-RED NPM module and node dependencies
COPY package.json /usr/src/node-red/
RUN npm install

# User configuration directory volume
VOLUME ["/data"]
EXPOSE 1880

# Environment variable holding file path for flows configuration
ENV FLOWS=flows.json
ENV NODE_PATH=/usr/src/node-red/node_modules:/data/node_modules

RUN npm install node-red-contrib-slack google-home-notify google-tts-api node-red-contrib-actionflows node-red-contrib-amqp node-red-contrib-chromecast node-red-contrib-google-home-notify \
	node-red-contrib-google-tts node-red-contrib-ibm-watson-iot node-red-contrib-play-audio node-red-contrib-redis node-red-contrib-slack node-red-node-darksky node-red-node-ledborg \
	node-red-node-pi-sense-hat node-red-node-ping node-red-node-pushbullet node-red-node-random node-red-node-serialport node-red-node-smooth openzwave-shared node-red-contrib-openzwave \
	node-red-contrib-harmony node-red-node-email node-red-node-feedparser node-red-node-sentiment node-red-node-email


#CMD ["npm", "start", "--", "--safe", "--userDir", "/data"]
CMD ["npm", "start", "--", "--userDir", "/data"]

