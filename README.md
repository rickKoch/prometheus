# Latest Prometheus in a Container Image

Latest Prometheus as a container image for learning, experimentation,
and using copying into multi-stage images.
```
docker build . -t prometheus

# load your machine's IP address into a variable - on Windows:
$hostIP = $(Get-NetIPConfiguration | Where-Object {$_.IPv4DefaultGateway -ne $null }).IPv4Address.IPAddress

# on Linux:
hostIP=$(ip route get 1 | awk '{print $NF;exit}')

# and on Mac:
hostIP=$(ifconfig en0 | grep -e 'inet\s' | awk '{print $2}')

# pass your IP address as an environment variable for the container:
docker container run -e DOCKER_HOST=$hostIP -d -p 9090:9090 prometheus:2.13.1
```


The configuration in thePrometheus image uses the DOCKER_HOST IP address to
talk to your host machine and collect the metrics you’ve configured in the
Docker Engine. It’s rare that you’ll need to access a service on the host from
inside the container, and if you do, you would usually use your server name
and Docker would find the IP address. In a development environment that might
not work, but the IP address approach should be fine.

Browse to `http://localhost:9090` and you’ll see the Prometheus web interface.
