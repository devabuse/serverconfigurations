# Nginx Caching

These are sensible defaults for caching images, css and other static files. I tend to place these in website specific configuration but can also be placed in httpd.conf

# website.conf

```
server {

  # Add whatever other configurations you need here


  # Don't expire the xml and json files.
  location ~* \.(?:xml|json)$ {
   expires -1;
  }

  # expire rss and atom files once per hour
  location ~* \.(?:rss|atom)$ {
   expires 1h;
   add_header Cache-Control "public";
  }

  # expires various image, fonts and media files every 30 days
  location ~* \.(?:jpg|jpeg|gif|png|ico|cur|gz|svg|svgz|mp4|ogg|ogv|webm|htc)$ {
   expires 30d;
   access_log off; # I don't feel the need to pollute my access log when these files are accessed. Personal preference.
   add_header Cache-Control "public";
  }

  # Expire CSS and JS files once a year. Yes, once a year. I use cache busting techniques for these files when needed.
  location ~* \.(?:css|js)$ {
   expires 1y; # expires max; is also an option
   access_log off;
   add_header Cache-Control "public";
  }  
}
```
