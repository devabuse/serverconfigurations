# NGINX static files

Nginx is amazing at sending static files stored on disk. Use the following configuration to set it up correctly.

## nginx.conf
```
# most of the default settings are good already, but make to sure to add the following
http {
  server_tokens off; # Hide nginx version in error messages and server response headers. Improves security

  sendfile on; # From my understanding it sends static files immediately without the expensive operations of server software
  tcp_nodelay on; # Send data as soon as possible. It removes a slight delay 0.2 second delay. Look into Nagle's algorithm for more in depth information
  tcp_nopush on; # In addition to the above two settings optimise the amount of data sent at once
}
```
