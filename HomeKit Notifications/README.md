# Home Assistant notifications to HomeKit
This is a working demo that triggers a notification on a HomeKit device.

Tested HA-version found in [.HA_VERSION](./.HA_VERSION)

![demo](./README-images/demo.gif?raw=true)


# Demo in Docker
The [example_docker-compose.yaml](./example_docker-compose.yaml) file can be used to create a container that only expose port 9123 with the web interface.

Copy the config-files in this repo to some path and modify the volume-mapping in the compose-file, renamed to docker-compose.yaml

Then start the container with
```bash
docker-compose up -d
```

Now you should be able to access the demo from http://ip.of.docker.host:9123/
