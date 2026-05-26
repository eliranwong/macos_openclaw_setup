# SearXNG Setup

## Install Docker

Follow instruction at:

https://docs.docker.com/desktop/setup/install/mac-install/

## Trouble Shoot Malware Block for Docker on macOS

In case Docker is blocked on macOS due to malware detection, run:

```
#!/bin/bash

# Stop the docker services
echo "Stopping Docker..."
sudo pkill '[dD]ocker'

# Stop the vmnetd service
echo "Stopping com.docker.vmnetd service..."
sudo launchctl bootout system /Library/LaunchDaemons/com.docker.vmnetd.plist

# Stop the socket service
echo "Stopping com.docker.socket service..."
sudo launchctl bootout system /Library/LaunchDaemons/com.docker.socket.plist

# Remove vmnetd binary
echo "Removing com.docker.vmnetd binary..."
sudo rm -f /Library/PrivilegedHelperTools/com.docker.vmnetd

# Remove socket binary
echo "Removing com.docker.socket binary..."
sudo rm -f /Library/PrivilegedHelperTools/com.docker.socket

# Install new binaries
echo "Install new binaries..."
sudo cp /Applications/Docker.app/Contents/Library/LaunchServices/com.docker.vmnetd /Library/PrivilegedHelperTools/
sudo cp /Applications/Docker.app/Contents/MacOS/com.docker.socket /Library/PrivilegedHelperTools/
```

For details, read https://github.com/docker/for-mac/issues/7520

## Install SearXNG

Run:

```
mkdir -p ./searxng/core-config/
cd ./searxng/
curl -fsSL \
    -O https://raw.githubusercontent.com/searxng/searxng/master/container/docker-compose.yml \
    -O https://raw.githubusercontent.com/searxng/searxng/master/container/.env.example
cp -i .env.example .env
# edit the default port
sed -i '' 's/^#SEARXNG_PORT=8080/SEARXNG_PORT=4000/' .env
# start the service
docker compose up -d
```

For details, read https://github.com/searxng/searxng

Recommendation: change the default 8080 to something else, e.g. 4000, in the .env file before run the install script.