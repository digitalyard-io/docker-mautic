# Mautic Docker image and examples

_This version refers to Docker images and examples for Mautic 5. If you would like information about older versions, see https://github.com/mautic/docker-mautic/tree/mautic4._

## Versions

## Variants

The Docker images exist in 2 variants:

* `apache`: image based on the official `php:apache` images.
* `fpm`: image based on the official `php:fpm` images.

See the `examples` explanation or `examples` folder how you could use them.

## Roles

each image can be started in 3 modes:

* `web`: runs the Mautic webinterface
* `worker`: runs the worker processes to consume the messenger queues 
* `cron`: runs the defined cronjobs

This allows you to use different scaling strategies to run the workers or crons, without having to maintain separate images.  
The `cron` and `worker` require the codebase anyhow, as they execute console commands.

## Examples

The `examples` folder contains examples of `docker-compose` setups that use the Docker images.  
     
Please take into account the purpose of those examples:  
it shows how it **could** be used, not how it **should** be used.  
Do not use those examples in production without reviewing, understanding and configuring them.

* `basic`: standard example using the `apache` image with `doctrine` as async queue.
* `fpm-nginx`: standard example using the `fpm` image in combination with an `nginx` with `doctrine` as async queue.


## Building your own images

You can build your own images easily using the `docker build` command in the root of this directory:

```
docker build . -f apache/Dockerfile -t mautic/mautic:v5-apache
docker build . -f fpm/Dockerfile -t mautic/mautic:v5-fpm
```

## Persistent storage

The images by default foresee following volumes to persist data (not taking into account e.g. database or queueing data, as that's not part of these images).

 * `config`: the local config folder containing `local.php`, `parameters_local.php`, ...
 * `var/logs`: the logs foldes
 * `docroot/media`: the uploaded and generated media files

## Configuration and customizing

### Configuration

The following environment variables can be used to configure how your setup should behave.
There are 2 files where those settings can be set:

* the `.env` file: 
  Should be used for all general variables 
* the `.mautic_env` file:
  Should be used for all Mautic specific variables.

#### MySQL settings
 - `MYSQL_HOST`: the MySQL host to connect to
 - `MYSQL_PORT`: the MySQL port to use
 - `MYSQL_DATABASE`: the database name to be used by Mautic
 - `MYSQL_USER`: the MySQL user that has access to the database
 - `MYSQL_PASSWORD`: the password for the MySQL user 
 - `MYSQL_ROOT_PASSWORD`: the password for the MySQL root user that is able to configure the above users and database

#### PHP settings

 - `PHP_INI_VALUE_DATE_TIMEZONE`: defaults to `UTC`
 - `PHP_INI_VALUE_MEMORY_LIMIT`: defaults to `512M`
 - `PHP_INI_VALUE_UPLOAD_MAX_FILESIZE`: defaults to `512M`
 - `PHP_INI_VALUE_POST_MAX_FILESIZE`: defaults to `512M`
 - `PHP_INI_VALUE_MAX_EXECUTION_TIME`: defaults to `300`

#### Mautic behaviour settings

 - `DOCKER_MAUTIC_ROLE`: which role does the container has to perform.  
   Defaults to `web`, other supported values are `worker` and `cron`.
 - `DOCKER_MAUTIC_LOAD_TEST_DATA`: should the test data be loaded on start or not.  
   Defaults to `false`, other supported value is `true`.  
   This variable is only usable when using the `web` role.
 - `DOCKER_MAUTIC_RUN_MIGRATIONS`: should the Doctrine migrations be executed on start.  
   Defaults to `false`, other supported value is `true`.  
   This variable is only usable when using the `web` role.

#### Mautic worker settings

 - `DOCKER_MAUTIC_WORKERS_CONSUME_EMAIL`: Number of workers to start consuming mails.  
   Defaults to `2`
 - `DOCKER_MAUTIC_WORKERS_CONSUME_HIT`: Number of workers to start consuming hits.  
   Defaults to `2`
 - `DOCKER_MAUTIC_WORKERS_CONSUME_FAILED`: Number of workers to start consuming failed e-mails.  
   Defaults to `2`

#### Mautic settings

Technically, every setting of Mautic you can set via the UI or via the `local.php` file can be set as environment variable.

e.g. the `messenger_dsn_setting` can be set via the `MAUTIC_MESSENGER_DSN_HIT` environment variable.
See the general Mautic documentatation for more info.

### Customization

Currently this image has no easy way to extend Mautic (e.g. adding extra `composer` dependencies or installing extra plugins or themes).
This is an ongoing effort we hope to support in an upcoming 5.x release.


## Issues

If you have any problems with or questions about this image, please contact us through a [GitHub issue](https://github.com/mautic/docker-mautic/issues).

You can also reach the Mautic community through its [online forums](https://www.mautic.org/community/) or the [Mautic Slack channel](https://www.mautic.org/slack/).

## Contributing

You are invited to contribute new features, fixes, or updates, large or small; we are always thrilled to receive pull requests, and do our best to process them as fast as we can.

Before you start to code, we recommend discussing your plans through a [GitHub issue](https://github.com/mautic/docker-mautic/issues), especially for more ambitious contributions. This gives other contributors a chance to point you in the right direction, give you feedback on your design, and help you find out if someone else is working on the same thing.
