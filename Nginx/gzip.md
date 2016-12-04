# Setup gzip for nginx

```
# most of the default settings are good already, but make to sure to add the following
http {
  gzip on;
  gzip_http_version 1.0;
  gzip_comp_level 4; # Try to find the right balance here. Higher number is better compression but required more processing power. 4 seems a good default
  gzip_min_length 1100;
  gzip_buffers 4 8k;
  gzip_proxied any;
  gzip_types
    # text/html is always compressed by HttpGzipModule
    text/css
    text/javascript
    text/xml
    text/plain
    text/x-component
    application/javascript
    application/json
    application/xml
    application/rss+xml
    font/truetype
    font/opentype
    application/vnd.ms-fontobject
    image/svg+xml;
  gzip_static on;

  gzip_proxied expired no-cache no-store private auth;
  gzip_disable "MSIE [1-6]\.";
  gzip_vary on;
}
```
