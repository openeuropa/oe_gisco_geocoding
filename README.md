# OpenEuropa GISCO Geocoder

[![ci](https://github.com/openeuropa/oe_gisco_geocoding/actions/workflows/ci.yml/badge.svg)](https://github.com/openeuropa/oe_gisco_geocoding/actions/workflows/ci.yml)

Provides a [Geocoder](https://www.drupal.org/project/geocoder) geocoding Drupal plugin integrating the Eurostat [GISCO](https://ec.europa.eu/eurostat/web/gisco) service via the [GISCO Geolocation Provider](https://github.com/openeuropa/gisco-geocoding-provider) PHP library.

**Table of contents:**

- [Installation](#installation)
- [Development](#development)
  - [Project setup](#project-setup)
  - [Using Docker Compose](#using-docker-compose)
  - [Disable Drupal 8 caching](#disable-drupal-8-caching)
- [Demo module](#demo-module)
- [Contributing](#contributing)
- [Versioning](#versioning)

## Installation

The recommended way of installing the OpenEuropa GISCO Geocoder module is via [Composer][1].

```bash
composer require openeuropa/oe_gisco_geocoding
```

### Enable the module

In order to enable the module in your project run:

```bash
./vendor/bin/drush en oe_gisco_geocoding
```

## Development

The OpenEuropa GISCO Geocoder project contains all the necessary code and tools for an effective development process, such as:

- All PHP development dependencies (Drupal core included) are required by [composer.json](composer.json)
- Project setup and installation can be easily handled thanks to the integration with [composer.json](composer.json), [GNU Make][4] and [Drush][3].
- All system requirements are containerized using [Docker Composer][2]

### Project setup

Download all required PHP code by running:

```bash
composer install
```

This will build a fully functional Drupal test site in the `./build` directory that can be used to develop and showcase the module's functionality.

This will also:

- Symlink the module in  `./build/modules/oe_gisco_geocoding` so that it's available for the test site
- Setup `./Makefile` file, which is the project's [GNU Make][4] configuration file. Note that `./Makefile` in not under VCS control and can be customized to meet a specific development environment conditions.

After the codebase is successfully deployed, and you've made all customizations to `./Makefile` file, install the site by running:

```bash
composer setup
```

This will:

- Install the test site, under `./build`
- Enable the OpenEuropa GISCO Geocoder module

### Using Docker Compose

Alternatively, you can build a development site using [Docker][5] and [Docker Compose][2] with the provided configuration.

Docker provides the necessary services and tools such as a web server and a database server to get the site running, regardless of your local host configuration.

#### Requirements:

- [Docker][5]
- [Docker Compose][2]

#### Configuration

By default, Docker Compose reads two files, a `docker-compose.yml` and an optional `docker-compose.override.yml` file. By convention, the `docker-compose.yml` contains your base configuration, and it's provided by default. The override file, as its name implies, can contain configuration overrides for existing services or entirely new services. If a service is defined in both files, Docker Compose merges the configurations.

Find more information on Docker Compose extension mechanism on [the official Docker Compose documentation](https://docs.docker.com/compose/extends/).

#### Usage

To start, run:

```bash
PHP_VERSION="8.1" docker-compose up
```

It's advised to not daemonize `docker-compose` so you can turn it off (`CTRL+C`) quickly when you're done working. However, if you'd like to daemonize it, you have to add the flag `-d`:

```bash
PHP_VERSION="8.1" docker-compose up -d
```

Note that you can pass a different PHP version as environment variable.

Then:

```bash
docker-compose exec -T web composer install
docker-compose exec -T web composer setup
```

Using default configuration, the development site files should be available in the `./build` directory and the development site should be available at: [http://127.0.0.1:8080/build](http://127.0.0.1:8080/build).

#### Running the tests

To run the PHP coding standards checks:

```bash
docker-compose exec web ./vendor/bin/phpcs
```

To run the PHPUnit tests:

```bash
docker-compose exec web ./vendor/bin/phpunit
```

### Step debugging

To enable step debugging from the command line, pass the `XDEBUG_SESSION` environment variable with any value to the container:

```bash
docker-compose exec -e XDEBUG_SESSION=1 web <your command>
```

Please note that, starting from XDebug 3, a connection error message will be outputted in the console if the variable is set but your client is not listening for debugging connections. The error message will cause false negatives for PHPUnit tests.

To initiate step debugging from the browser, set the correct cookie using a browser extension or a bookmarklet like the ones generated at https://www.jetbrains.com/phpstorm/marklets/.

## Contributing

Please read [the full documentation](https://github.com/openeuropa/openeuropa) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the available versions, see the [tags on this repository](https://github.com/openeuropa/oe_gisco_geocoding/tags).

[1]: https://www.drupal.org/docs/develop/using-composer/using-composer-to-manage-drupal-site-dependencies#managing-contributed
[2]: https://docs.docker.com/compose
[3]: https://www.drush.org/
[4]: https://www.gnu.org/software/make/
[5]: https://www.docker.com/get-docker
