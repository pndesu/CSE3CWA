FROM node:18.16.0-alpine
RUN apk add --no-cache tini
ENTRYPOINT ["/sbin/tini", "--"]
RUN apk add --no-cache fortune
# Create a working directory for our application.
RUN mkdir -p /app
WORKDIR /app
# Install the project's NPM dependencies.
COPY package.json /app/
RUN npm --silent install
RUN mkdir /deps && mv node_modules /deps/node_modules
# Set environment variables to point to the installed NPM modules.
ENV NODE_PATH=/deps/node_modules \
PATH=/deps/node_modules/.bin:$PATH
# Copy our application files into the image.
COPY . /app
# Switch to a non-privileged user for running commands.
RUN chown -R node:node /app /deps
USER node
# Expose container port 3000.
EXPOSE 3000
# Set the default command to use for `docker run`.
# `npm start` simply starts our server.
CMD [ "npm", "start" ]