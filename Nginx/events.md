# NGINX Event handling

Base settings for dealing with events in nginx configurations that are often setup incorrectly.

## nginx.conf

```
worker-processes: x; # use the output from the following command for the correct value: grep processor /proc/cpuinfo | wc -l

events {
  worker_connections 1024; # use the output from: ulimit -n
  multi_accept on; # allows for multiple connections at the same time
  use epoll; # use a scalable I/O for event notifications
}
```
