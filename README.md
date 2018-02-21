## Forked from [https://github.com/locnh/docker-magento(https://github.com/locnh/docker-magento)

## What is this ?
A minimal Docker container built with CentOS 7, NGINX and PHP with compatible version and modules for Magento2.

Sandbox build for Magento 2.x (`2-dev`) comes with composer, nodejs, grunt.

These are the Docker Hub autobuild images located [here](https://hub.docker.com/r/kamchanliu/docker-magento/).

![Magento Logo](http://www.elevateweb.co.uk/wp-content/themes/porto/assets/img/headers/mage-logo.png)

## How to start
### The Simplest way, NginX - PHP-FPM by default
1. Pull the latest image

  ```
  $ docker pull kamchanliu/docker-magento
  ```

2. Create container

  ```
  $ docker run --name magento -v /path/to/magento:/var/www/html -p 80:80 -d kamchanliu/docker-magento
  ```

That's it !

## More Options

#### Set the `uid` (and/or `gid`) of `apache` inside container
- To change the apache `uid`, use `-e UID=<your uid>` (you can use this).
- To change the apache `gid`, use `-e GID=<your gid>` (use at your own risk).
- Example:

  ```
  $ docker run --name magento -e UID=501 -v /path/to/magento:/var/www/html -p 80:80 -d kamchanliu/docker-magento
  ```


#### Enable Xdebug remote (Sandbox build only)
- Just add the environment variable with `-e XDEBUG_RHOST=<REMOTE_HOST>`.
- Example your IDE is running at IP `10.0.75.1`:

  ```
  $ docker run --name magento -e XDEBUG_RHOST=172.17.0.1 -v /path/to/magento:/var/www/html -p 80:80 -d kamchanliu/docker-magento
  ```

#### Mountable Volumes
Beside Webroot `/var/www/html`, you can use option `-v` to mount more volumes to the container such as services logs:
- NginX logs:   `/var/log/nginx`
- PHP-FPM logs: `/var/www/php-fpm`

#### Please keep in mind
If you don't specify tag, default tag is `latest`. In case you need the build for specified version of magento, use the version as tag. For example, if you need a container to run:
- Magento `2.x`:

  ```
  $ docker run --name magento -v /path/to/magento:/var/www/html -p 80:80 -d kamchanliu/docker-magento:2
  ```
