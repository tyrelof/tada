#A lightweight node image
FROM mhart/alpine-node:6.5.0

#PM2 will be used as PID 1 process
RUN npm install -g pm2@1.1.3

# Copy package json files for services

COPY app1/package.json /var/www/app1/package.json
COPY app2/package.json /var/www/app2/package.json

# Set up working dir
WORKDIR /var/www

# Install packages
RUN npm config set loglevel warn \
# To mitigate issues with npm saturating the network interface we limit the number of concurrent connections
    && npm config set maxsockets 5 \
    && npm config set only production \
    && npm config set progress false \
    && cd ./app1 \
    && npm i \
    && cd ../app2 \
    && npm i


# Copy source files
COPY . ./

# Expose ports
EXPOSE 3000
EXPOSE 3001

# Start PM2 as PID 1 process
ENTRYPOINT ["pm2", "--no-daemon", "start"]

# Actual script to start can be overridden from `docker run`
CMD ["process.json"]
