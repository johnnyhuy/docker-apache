# Docker Apache

An Apache image that uses the httpd image with Alpine as the OS.

## Enabled Modules

This list does not include the default loaded modules.

- mod_ssl
- mod_proxy
- mod_proxy_balancer
- mod_proxy_http
- mod_slotmem_shm
- mod_lbmethod_byrequests
- mod_rewrite
- mod_authn_socache
- mod_cache
- mod_socache_shmcb
- mod_socache_dbm
- mod_socache_memcache

## Docker Compose File

Using this file is easier to configure compared to running it via commands with `docker`.

```
version: "3.1"
services:
  web:
    image: johnnyhuy/apache
    ports:
      - 80:80
      - 443:433
```

## Editing httpd.conf

To edit the main configuration file, you must copy the httpd.conf file from the container and mount the file using volumes. Run the following commands:

### Step #1 (Option #1): If you have a `docker-compose.yml` file

```
docker-compose up -d
```

### Step #1 (Option #2): Run with docker commands

```
docker run -d johnnyhuy/apache
```

### Step #2: Copy config file

Replace tokens in [] to required information.

```
docker cp [CONTAINER NAME]:/usr/local/apache2/conf/httpd.conf [DESTINATION]
```

### Step #3 (Option #1): If you have a `docker-compose.yml` file

Add the `volumes` option in the config to mount. Replace tokens in [] to required information.

```
version: "3.1"
services:
  web:
    image: johnnyhuy/apache
    ports:
      - 80:80
      - 443:433
    volumes:
      - [LOCATION OF COPIED .CONF]:/usr/local/apache2/conf/httpd.conf
```

### Step #3 (Option #2): Run with docker commands

```
docker run -d -v [LOCATION OF COPIED .CONF]:/usr/local/apache2/conf/httpd.conf johnnyhuy/apache
```

## Using SSL/HTTPS

Follow the same steps as the **Editing httpd.conf** section above, but with some additional steps.

### Step #1: Edit httpd.conf

Edit the config file to include the `httpd-ssl.conf` file that includes all SSL related configurations. This file is located at `/usr/local/apache2/conf/extra/httpd-ssl.conf` on the image. Uncomment the include line as shown:

```
</IfModule>
...
# Secure (SSL/TLS) connections
# Include conf/extra/httpd-ssl.conf
#
# Note: The following must must be present to support
...
```

```
...
# Secure (SSL/TLS) connections
Include conf/extra/httpd-ssl.conf
#
# Note: The following must must be present to support
...
```

## Step #2: Follow "Editing httpd.conf" section with some modifications

Replace `/usr/local/apache2/conf/httpd.conf` to `/usr/local/apache2/conf/extra/httpd-ssl.conf` .

## Step #3: Edit httpd-ssl.conf

Be sure to check where your SSL certificates are located and mount the files using volumes.