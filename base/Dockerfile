# Use the base App Engine Docker image, based on debian jessie.
FROM gcr.io/google_appengine/base

# Install updates and dependencies
RUN apt-get update -y && apt-get install --no-install-recommends -y -q curl python build-essential git ca-certificates libkrb5-dev && \
    apt-get clean && rm /var/lib/apt/lists/*_*

# Use nvm for installing node binaries (and verifying checksums)
ADD nvm /nvm

# TODO(ofrobots) use a mirror. The following isn't actually a mirror
#ENV NVM_NODEJS_ORG_MIRROR https://storage.googleapis.com/gae_node_packages

# Install the latest LTS release of nodejs
#RUN mkdir /nodejs && curl https://nodejs.org/dist/v4.4.4/node-v4.4.4-linux-x64.tar.gz | tar xvzf - -C /nodejs --strip-components=1
RUN /bin/bash -c "source /nvm/nvm.sh && nvm install 4.4.5 && ln -s /nvm/versions/node/v4.4.5 /nodejs"
ENV PATH $PATH:/nodejs/bin

# Install semver, as required by the node version install script.
RUN npm install https://storage.googleapis.com/gae_node_packages/semver.tar.gz

# Add the node version install script
# TODO(ofrobots): Need to update install_node to use nvm as well
ADD install_node /usr/local/bin/install_node

# Set common env vars
ENV NODE_ENV production

WORKDIR /app

# start
CMD ["npm", "start"]
