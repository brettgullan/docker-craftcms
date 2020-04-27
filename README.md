# DOCKER RUNTIME FOR CRAFT CMS

This repo provides a basic 'drop-in' web development runtime for Craft CMS.

It is not intended to provide a production-quality environment, it does not download or install Craft CMS, nor install or update dependencies, back up your database or any other bells and whistles.

## So what does it do?

The idea behind this boilerplate is to provide a minimal, but flexible, stack for running a local Craft CMS development environment. Web server, PHP and database.

The basic `docker-compose.yml` file configures and runs separate Apache, PHP-FPM and MySQL services.

Thus, with minimal Docker experience, it is easy to reconfigure an environment to use Postgres, or Nginx if desired.

## Motivation

I didn't want to get into highly bespoke or custom image builds. I've always found they're not worth the effort. Environments and technologies change too frequently, and I don't have time to maintain them. I wanted a 'stock' environment that would be easy to update and maintain, and that could be added to existing projects and modified per-project as and when required.

I'm not overly concerned about maintaining production fidelity -- there are simply far too many possibilities to cover. My focus is simply to create a development environment that will run Craft CMS. That said, in my experience PHP is fairly robust. Minor version differences are rarely the cause of issues between environments. So this stack should serve pretty well to be getting started with.


# HOW TO USE

Clone this repo (or copy-paste into an existing project).
If cloning, delete `.git` directory, then run `git init`.

## With an existing Craft CMS install

To use this boilerplate with an existing Craft CMS configuraiton, clone into a temporary directory, then copy the `docker` directory and `docker-compose.yml` files into your project.

Create a `database` directory in the root of your project.

Copy any relevant environment variables from the boilerplate `.env` file to your project's.

Now you should be ready to go. Just run the following from your terminal:

```shell
$ docker-compose up
```


## Docker-Compose configuration

We map the entire `./` directory to `/var/www` in both the `apache` and `php` environments.
We also modify the _default_ web directory from `/var/www/html` to `/var/www/web`, simply to be consistent with out-of-the-box Craft CMS conventions.

We expose the default MySQL database port `3306` so you can connect directly to the running database instance using your favourite database management tool.

## TIPS, TRICKS & GOTCHAS

## Database
When connecting to the database from within the Docker environment, use the Docker **service name** defined in `docker-compose.yml` -- i.e. `mysql`, not the usual `localhost` or `127.0.0.1`.

## Docker

When you really, really need to shut everything down ...
```shell
  docker-compose stop
  docker-compose kill
  docker-compose rm -f
```

# REFERENCES

https://github.com/prooph/docker-files/blob/master/php/7.4-fpm  
https://github.com/mzazon/php-apache-mysql-containerized/blob/master/README.md  
http://www.inanzzz.com/index.php/post/su76/creating-apache-mysql-and-php-fpm-containers-for-a-web-application-with-docker-compose