# Configuration

| container | ports | volumes |
|-----------|:-----:|---------|
| mariadb   | 3306  |  config/mariadb:/var/lib/mysql |
| phpmyadmin | 8001:80 | |
| php | - | sites:/usr/local/apache2/htdocs |
| httpd | 80 | config/apache/httpd.conf:/usr/local/apache2/conf/httpd.conf <br> config/apache/vhosts:/usr/local/apache2/conf/vhosts <br> sites:/usr/local/apache2/htdocs |

# Usage

## Adding a website

1. Create a directory in `sites/`
2. Create a vhost configuration file in `config/apache/vhosts`
3. Add the defined servername to your hosts file (eg : `/etc/hosts`)
4. Go to `compose` and run `docker-compose up -d`

## Adding PHP extensions

1. Update `compose/build/php/Dockerfile` according to your need (cf : PHP image documentation)
2. Rebuild the image using `docker-compose build`

[PHP image documentation](https://hub.docker.com/_/php)