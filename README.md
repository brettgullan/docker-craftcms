## DOCKER WEB HOSTING BOILERPLATE

This repo configures a basic web development stack, optimized for PHP/MYSQL and tailored for Craft CMS.

## HOW TO USE

Clone this repo (or copy-paste into an existing project).
If cloning, delete `.git` directory, then run `git init`.


## Docker-Compose configuration

We map the entire `./` directory to `/var/www` in both the `apache` and `php` environments.
We also modify the _default_ web directory from `/var/www/html` to `/var/www/web`, simply to be consistent with out-of-the-box Craft CMS conventions.

## TIPS & TRICKS

When you really, really need to shut everything down ...
```
  docker-compose stop
  docker-compose kill
  docker-compose rm -f
```

# REFERENCES

https://github.com/mzazon/php-apache-mysql-containerized/blob/master/README.md

http://www.inanzzz.com/index.php/post/su76/creating-apache-mysql-and-php-fpm-containers-for-a-web-application-with-docker-compose