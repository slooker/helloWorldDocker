## This tells us what to use as a base.  The node docker containers are good, so might as well use them
FROM node:4-slim

## Make a directory for our app
RUN mkdir -p /opt/app


## Set the default directory we're using on our container
WORKDIR /opt/app

## This will use the cache if package.json hasn't been changed
ADD package.json /tmp/package.json

## Add build dependencies so we can build our module with node-gyp, and assume "yes" to any questions
## Then run npm install from the /tmp directory
## Then remove build dependencies and apt-get cache
RUN apt-get update  && \
 apt-get install python build-essential -y && \
 cd /tmp && npm install && \
 cp -a /tmp/node_modules /opt/app/ && \
 apt-get remove python build-essential -y && \
 apt-get autoremove -y && \
 rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

## Add our code.  I like to be explicit and not use wildcards, since it's easy to get the node_modules 
## directory which may need to build things which won't work on different architectures (i.e. mac to linux)
## We want to add this as close to teh end as possible, because this is what's most likely to change.
## Once a layer invalidates the cache by changing, everything after that layer is rebuilt, even if it hasn't changed.
ADD index.js /opt/app/

## Expose a port that we can access our web app on
EXPOSE 3013

## Tell our app how to start when we run the docker container.  This has to be an array of a command and its arguments
CMD ["npm", "start"]
