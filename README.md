# Yourls Docker

Using a Docker Container to run a YOURLS instance with CentOS, Nginx and fpm.

Image available at [Docker Hub](https://hub.docker.com/r/gabrielrf/docker-yourls/).

Visit [YOURLS official website](http://yourls.org) for more information.

This repository is a fork from [tsgoff/docker-yourls](https://github.com/tsgoff/docker-yourls).

* [Docker Installation](#docker-installation)
* [Docker-Compose Installation](#docker-compose-installation)
* [Quick Install Using Docker-Compose](#quick-install-using-docker-compose)
  * [Quick-tip](#quick-tip)
* [Options](#options)

## Docker Installation

```
curl -ssl https://get.docker.com | sh
```

## Docker-Compose Installation
[suggested]
```
curl -L https://github.com/docker/compose/releases/download/1.6.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
curl -L https://raw.githubusercontent.com/docker/compose/$(docker-compose version --short)/contrib/completion/bash/docker-compose > /etc/bash_completion.d/docker-compose
```

## Quick Install Using Docker-Compose:

Create a file named `docker-compose.yml`
```
vi docker-compose.yml
```
Then paste
```
yourls:
    image: gabrielrf/docker-yourls
    environment:
        - YOURL_USER=admin
        - YOURL_PASSWORD=PASSWORD
        - YOURLS_SITE=http://DOMAIN.TLD
        - YOURLS_URL_CONVERT=62
    links:
        - yourlsmysql:mysql
    ports:
        - 80:80
    volumes:
        - /home/plugins:/usr/share/host
    restart: always
yourlsmysql:
    image: mysql
    environment:
        - MYSQL_ROOT_PASSWORD=PASSWORD
        - MYSQL_DATABASE=yourls
    restart: always
```
Set `YOURL_PASSWORD`, `YOURLS_SITE` and `MYSQL_ROOT_PASSWORD` as you prefer.

Finally, run
```
docker-compose up
```

YOURLS must be available on the port 80 of your machine. It will be installed when you first run it and must be fine after that.

By now, your container must be up and running properly! Easy, huh?

### Quick-tip:
To run it as a daemon, simply add a `-d` to the end of the command
### Quick-tip2:
The command "restart: always" make your container persistent on failure or server reboot, use it with the previous tip.  
```
docker-compose up -d
```

## Options:

#### `YOURLS_SITE`
Your (hopefully short) domain URL, no trailing slash, lowercase. If you pick the non-www version of your domain, don't use the www version in your browser (and vice-versa).
Example: define( 'YOURLS_SITE', 'http://ozh.in' );

#### `YOURL_USER`
User or Users allowed to log in.

#### `YOURL_PASSWORD`
Password of the user or users listed on `YOURL_USER`.

#### `YOURLS_URL_CONVERT` _optional_
URL shortening method. Options are `36` and `62`.

Base 36 __(default)__ encoding uses _0123456789abcdefghijklmnopqrstuvwxyz_.

Base 62 encoding uses _0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz_.

#### `REDIRECT_MAIN_PAGE` _optional_
The webpage users will be redirected in case of a misspelled link or and attempt to access the url defined on `YOURLS_SITE`.

#### `HTTPS` _optional_
If `true` users will only be able to visit links using `https://`

To turn this option off, simply remove it from `docker-compose.yml`

#### `FQDN` _optional_
Parameter present on the file `/etc/nginx/conf.d/default.conf`

#### `SSL_SELFSIGNED` _optional_
Used to generate self signed certificates.

#### `SSL_PROTOCOLS` _optional_
__Depends on `SSL_SELFSIGNED`__.
Protocols used on self signed certificates.

#### `SSL_CIPHERS` _optional_
__Depends on `SSL_SELFSIGNED`__.
SSL ciphers used.

#### `DB_PREFIX` _optional_
Prefix of every database. Default value is `root` for MYSQL and `postgres` for PostgreSQL.

#### `DB_HOST` _optional_
Host used for external databases.

#### `DB_USER` _optional_
__Depends on `DB_HOST`.__ User allowed to log on databases. Default is `yourls`.

#### `DB_PASSWORD` _optional_
__Depends on `DB_HOST`.__ Password to access the database. It can (I suggest it must) be different from `YOURL_PASSWORD`.

#### `DB_NAME` _optional_
__Depends on `DB_HOST`.__ Database name.

#### `YOURLS_PUB_DUMMY` _optional_
__Do not use this if using `REDIRECT_MAIN_PAGE`.__ Creates a public dummy site.
