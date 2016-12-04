# FastCGI php files

Nginx needs fastcgi to serve PHP files. You can also setup nginx to enable it to cache these requests. This makes shit fast. You can put this in httpd.conf or a site specific config, which i tend to do

## website.conf

```
server {  
  # Setup fastcgi paths and caching keys
  fastcgi_cache_path /tmp/cache levels=1:2 keys_zone=websitename:100m inactive=60m;
  fastcgi_cache_key "$scheme$request_method$host$request_uri";

  # Set a no cache variable which we will set to 1 when we want to disable fastcgi caching
  set $nocache 0;

  # Add whatever other configurations you need here:



  # Example of when to set $nocache to 1. This example is specific to Drupal, which sets a cookie called SESS for authenticated users
  if ($http_cookie ~ SESS) {
    set $nocache 1;
  }

  # How to deal with PHP requests. Set the following variable in php.ini cgi.fix_pathinfo=0
  location ~ '\.php$' {

    # Setup fastcgi caching
    fastcgi_cache websitename; # websitename must match keys_zone in fastcgi_cache_path
    fastcgi_cache_valid 200 1m; # Cache 200 responses for 1m (or however high you like it)
    fastcgi_cache_methods GET HEAD; # Cache only GET and HEAD requests.
    add_header X-Fastcgi-Cache $upstream_cache_status; # Add a header to check if the php request was cached or not
    fastcgi_cache_bypass $nocache; # Use the $nocache variable for when to bypass cache
    fastcgi_no_cache $nocache; # Use the $nocache variable for when not to cache

    fastcgi_split_path_info ^(.+?\.php)(|/.*)$;

    include fastcgi_params;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_hide_header "X-Generator"; # Depends on the PHP solution, some set headers which I tend to remove as they contain version numbers
    fastcgi_hide_header "X-Powered-By"; # Depends on the PHP solution, some set headers which I tend to remove as they contain version numbers
    fastcgi_param PATH_INFO $fastcgi_path_info;
    fastcgi_intercept_errors on;
    fastcgi_pass unix:/var/run/php/php7.0-fpm.sock; # You can also use a PHP5 fpm for legacy applications
  }
}
```
