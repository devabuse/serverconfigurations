# PHP 7 Opcache

## Setup

When compiling PHP7 compile with the following to enable opcache: --enable-opcache --enable-opcache-file. This will setup PHP to cache in shared memory, but also on disk as a backup which it doesn't normally do.

## php.ini


```
expose_php = Off # not required for opcache, but very sensible to set as it will hide PHP version when checking website headers

opcache.enable=1
opcache.memory_consumption=512M # Depends on amount of Ram and the application
opcache.max_accelerated_files=8000 # Depends on the number of files: find . -type f -print | grep php | wc -l
opcache.interned_strings_buffer=16 # Sensible default as base 4 is a bit on the low end
opcache.revalidate_freq=1 # Set this to 0 for your development environment
opcache.huge_code_pages=1 # Your OS will need to have support for huge pages
opcache.consistency_checks=1
opcache.fast_shutdown=0 # This should be default, but setting this to 1 can cause segfaults
opcache.validate_timestamps=1

opcache.enable_cli=0
opcache.file_cache=/var/tmp/php/opcache # This needs to be writable folder for PHP --enable-opcache-file to work
opcache.file_cache_only=0
opcache.file_cache_consistency_checks=1
```
