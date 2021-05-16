FROM arm32v7/node:10-stretch-slim

USER root

RUN apt-get update && apt-get install -y git make g++ python net-tools

RUN git clone https://github.com/OpenZWave/open-zwave.git
WORKDIR /open-zwave
RUN make && make install && ldconfig

RUN mkdir -p /usr/src/node-red

RUN mkdir /data

WORKDIR /usr/src/node-red

RUN useradd --home-dir /usr/src/node-red --no-create-home node-red \
    && chown -R node-red:node-red /data \
    && chown -R node-red:node-red /usr/src/node-red

USER node-red

COPY package.json /usr/src/node-red/
RUN npm install

VOLUME ["/data"]
EXPOSE 1880

ENV FLOWS=flows.json
ENV NODE_PATH=/usr/src/node-red/node_modules:/data/node_modules

WORKDIR /usr/src/node-red/
RUN git clone https://github.com/OpenZWave/node-red-contrib-openzwave.git
WORKDIR /usr/src/node-red/node-red-contrib-openzwave

RUN sed -i "s|1.6.2|1.7.2|g" ./package.json

RUN sed -i "s|=5.1|=5.17.0|g" ./package.json

WORKDIR /usr/src/node-red
RUN npm install ./node-red-contrib-openzwave

RUN npm install node-red-contrib-slack node-red-contrib-amqp \
	node-red-contrib-redis node-red-node-darksky \
	node-red-node-ping node-red-node-pushbullet node-red-node-random node-red-node-serialport node-red-node-smooth \
	node-red-contrib-harmony node-red-node-email node-red-node-feedparser node-red-node-sentiment node-red-contrib-cast \
        node-red-contrib-arp node-red-contrib-function-npm

#CMD ["npm", "start", "--", "--safe", "--userDir", "/data"]
CMD ["npm", "start", "--", "--userDir", "/data"]

