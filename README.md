# DOCKER RUNTIME FOR CRAFT CMS

This repo provides a basic 'drop-in' web development runtime for Craft CMS.

It is not intended to provide a production-quality environment, it does not download or install Craft CMS, nor install or update plugins or dependencies, back up your database or any other bells and whistles.

## So what does it do?

The idea behind this boilerplate is to provide a minimal, but flexible, stack for running a local Craft CMS development environment. Web server, PHP and database.

The basic `docker-compose.yml` file configures and runs separate Apache, PHP-FPM and MySQL services, using minimal defaults and volume mappings. Just enough to get you going.

Thus, with minimal Docker experience, it is easy to reconfigure an environment to use Postgres, or Nginx if desired. Or to add Redis support or whatever else you might need.

Check out the `use-nginx` branch of this repo for a boilerplate that uses Nginx instead of Apache.

## Motivation

I didn't want to get into highly bespoke or custom image builds. I've always found they're not worth the effort. Environments and technologies change too frequently, and I don't have time to maintain them. I wanted a 'stock' environment that would be easy to update and maintain, and that could be added to existing projects and modified per-project as and when required.

I'm not overly concerned about maintaining production fidelity -- there are simply far too many possibilities to cover. My focus is simply to create a development environment that will run Craft CMS. That said, in my experience PHP is fairly robust. Minor version differences are rarely the cause of issues between environments. So this stack should serve pretty well to be getting started with.


# HOW TO USE

Clone this repo (or copy-paste into an existing project).
If cloning, delete `.git` directory, then run `git init`.

## With an existing Craft CMS install

To use this boilerplate with an existing Craft CMS configuraiton, clone into a temporary directory, then copy the `docker` directory and `docker-compose.yml` files into your project.

Easiest way to do this is via terminal, from within the cloned `docker-craftcms` directory issue:

```shell
$ cp -r ./docker* <path-to-destination-directory>
```

Create a `database` directory in the root of your project.

```shell
$ mkdir database
```

This is used to store the internal database data files (used by MySQL or Postgres) from within the Docker container. It allows the database to persist across Docker restarts. Otherwise, any changes to your Craft database tables would be lost.

To populate the database instance with an existing database dump, add the following line to your `docker-compose.yml` in the database `volumes` configuration:

```yaml
    - ./sql:/docker-entrypoint-initdb.d
```

Where `./sql` is the path to a project directory that contains the database dump file(s).  
Any `.sql` or `.zip` files (containing `.sql` files) will be processed alphabetically and imported into your database.  
Note this only occurs on container initialization. 

Your database configuration should look like this:

```yaml
  mysql:
    image: mariadb:${MYSQL_VERSION:-latest}
    networks:
      - backend
    ports:
      - "3306:3306"
    environment:
      MYSQL_ALLOW_EMPTY_PASSWORD: 'yes'
      MYSQL_USER: ${DB_USER}
      MYSQL_PASSWORD: ${DB_PASSWORD}
      MYSQL_DATABASE: ${DB_DATABASE}
    volumes:
      - ./docker/mysql/my.cnf:/etc/mysql/conf.d/custom.cnf
      - ./database:/var/lib/mysql:delegated
      - ./sql:/docker-entrypoint-initdb.d
    container_name: db
```


Copy any relevant environment variables from the boilerplate `.env` file to your project's.

Now you should be ready to go. Just run the following from your terminal:

```shell
$ docker-compose up
```

## Docker-Compose configuration

We mount each of the Craft CMS directories into the `php` 
We map the entire `./` directory to `/var/www` in both the `apache` and `php` environments.

We also modify the _default_ web directory from `/var/www/html` to `/var/www/web`, simply to be consistent with out-of-the-box Craft CMS conventions.

We expose the default MySQL database port `3306` so you can connect directly to the running database instance using your favourite database management tool.

## TIPS, TRICKS & GOTCHAS

## Using Nginx instead of Apache

To use Nginx instead of Apache, replace the `apache` service definition in `docker-compose.yml` with the following:
```yaml
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    depends_on:
      - php
      - mysql
    networks:
      - frontend
      - backend
    volumes:
      - ./docker/nginx/default.conf:/etc/nginx/conf.d/default.conf
      - .env:/var/www/.env
      - ./web:/var/www/web
    links:
      - php
    container_name: nginx
```

## Database
When connecting to the database from within the Docker environment, use the Docker **service name** defined in `docker-compose.yml` -- i.e. `mysql`, not the usual `localhost` or `127.0.0.1`.

## Docker

When you really, really need to shut everything down ...
```shell
  docker-compose stop
  docker-compose kill
  docker-compose rm -f
```

# LIMITATIONS

Currently **does not support** database backups from Craft Control Panel.

# TODO

Add database backup capabilities to resolve limitation above. i.e.: install mysql client binaries.  
@see https://github.com/craftcms/cms/issues/2364

# REFERENCES

https://github.com/prooph/docker-files/blob/master/php/7.4-fpm  
https://github.com/mzazon/php-apache-mysql-containerized/blob/master/README.md  
http://www.inanzzz.com/index.php/post/su76/creating-apache-mysql-and-php-fpm-containers-for-a-web-application-with-docker-compose