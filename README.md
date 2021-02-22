## Set up local development

This requires Docker and Docker-Compose to be installed in order to run.

* Clone the repository to a local project directory
* Create a `.env` file in the `cms/` directory, based off the provided `example.env`
* Start up the site with `docker-compose up`

## Seed a database

Create a root folder `db-seed` and insert an existing database backup to have it imported on the initial build of the database volume.

## Troubleshooting

If Craft CMS hasn't previosly been installed in the project directory, or there is not existing database you may run into errors accessing the site after running `docker-compose up`. If this is the case, try the following from within the docker_php_container for the project (replace DOCKER_PHP_CONTAINER with your container's name).

In a new terminal:
```
docker exec -it DOCKER_PHP_CONTAINER bash
```

Proceed through the following steps, stopping once the site works correctly.

### Ensure Craft is installed
1. Try running `./craft`, which should return a list of commands. If you don't see these commands, there are other errors that need to be resolved - follow the error messages and repeat this until `./craft` returns the list of commands.
2. Run `./craft setup/welcome` to ensure an application ID and security key have been set.
3. If a new install and/or no database seed was provided, run the `./craft setup` command, match your settings if changed from the default values:
```
Which database driver are you using? [mysql,pgsql,?]: mysql
Database server name or IP address: [127.0.0.1] mariadb
Database port: [3306] 3306
Database username: [root] project
Database password: project
Database name: project
```

### Set file permissions
1. Ensure the proper [file permissions](https://craftcms.com/docs/3.x/installation.html#step-2-set-the-file-permissions) are set for Craft.
```
chmod -R 777 .env composer.json composer.lock config/license.key config/project/* storage/* vendor/* web/cpresources/*
```

### Rebuild the volumes
If the site still has errors when loading, try rebuilding the volumes now that the above issues are corrected.

1. Remove the existing volumes: `docker-compose down -v`
2. Start and recreate the volumes: `docker-compose up`
