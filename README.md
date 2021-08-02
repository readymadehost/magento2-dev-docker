# Magento2 dev docker

A development docker for every magento2 project


## Features

- Build for magento2 projects
- Bundle of `fpm`, `cli`, `nginx`, `mariadb`, `phpmyadmin`, `redis` , `elasticsearch`, `rabbitmq`, `emailcatcher` and `varnish` containers
- Latest PHP versions
  - php7.4 (for Magento2.4.x - default)
- Latest database mariadb10.4, mariadb10.2 and other versions supported
- Latest node14.x, node13.x, node12.x, ... supported
- Latest redis-6.x, redis-5.x, ... supported
- Latest elasticsearch-7.9.x, elasticsearch-6.x, ... supported
- Latest rabbitmq-3.8.x, ... supported
- Latest varnish-6.4, varnish-6.0, ... supported
- Included n98-magerun2, composer, node cli, yarn cli and grunt cli
- Included emailcatcher with smtp and web view
- Support for PhpStorm or VSCode + WSL2/docker-desktop setup
- Support for xdebug included check `.env` file


## Docker setup

- `git clone https://github.com/readymadehost/magento2-dev-docker.git project-docker`
- `cd project-docker`
- `mkdir project` or `git clone <some_git_repo_url> project` for existing project
- `cp .env.sample .env` and review `.env` file
- `docker-compose build`
- `docker-compose up -d`
- `docker-compose exec cli bash`
- `php -v && php -m`


## New magento2 project install

- Read https://devdocs.magento.com/guides/v2.4/install-gde/composer.html
- `docker-compose exec cli bash` and make sure you are at `/var/www/project` dir
- Setup new magento2 project using composer
  - `composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition .`
  - or `composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .`
  - or `composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition={version} .`
- Run bash alis `mpp` for `/root/manage-project-permission.sh`
- Run `bin/magento` and should return list of commands
- Magento2.4.x project install command https://devdocs.magento.com/guides/v2.4/install-gde/install/cli/install-cli.html
- Setup sample data `bin/magento sampledata:deploy`
- `bin/magento setup:upgrade`
- Run bash alis `mpp` for `/root/manage-project-permission.sh`

#### Install for Magento2.4.x

```
bin/magento setup:install \
--base-url=http://localhost:8080 \
--db-host=mariadb \
--db-name=project \
--db-user=root \
--db-password=root \
--backend-frontname=admin \
--admin-firstname=admin \
--admin-lastname=admin \
--admin-email=admin@admin.com \
--admin-user=admin \
--admin-password=admin@123 \
--language=en_US \
--currency=USD \
--timezone=America/Chicago \
--use-rewrites=1 \
--search-engine=elasticsearch7 \
--elasticsearch-host=elasticsearch \
--elasticsearch-port=9200
```


## Notes

- Magento2 project URL: http://{localhost/any_valid_host}:8080/
- PhpMyAdmin URL: http://{localhost/any_valid_host}:8081/
- Mailcatcher URL: http://{localhost/any_valid_host}:8082/
- Varnish URL: http://{localhost/any_valid_host}:8088/
- For more info and change, check `.env` and `docker-compose.yml`
- Manage permission inside container using bash alias `mpp` or `/root/manage-project-permission.sh`
- Mariadb default:- host: `mariadb` user: `root`, password: `root`, database: `project`
- RabbitMQ default:- host: `rabbitmq` user: `root`, password: `root`

```text
- <docker_root_dir> <-- docker root dir
- <docker_root_dir>/data <-- all docker data persist
- <docker_root_dir>/nginx <-- nginx
- <docker_root_dir>/php* <-- php cli and fpm containers
- <docker_root_dir>/.env <-- docker environment configuration

- <docker_root_dir>/project <-- project root dir

- <docker_root_dir>/project* <-- added in .gitignore
- <docker_root_dir>/*.sql <-- added in .gitignore
```

## Elasticsearch support

- mkdir data/elasticsearch
- sudo chown -R 1000:1000 -R data/elasticsearch

#### Elasticsearch container keeps stopping? Try followings:-

#### Temp solution

- sudo sysctl -w vm.max_map_count=262144

#### For debian host persistence

- sudo vi /etc/sysctl.conf
- add new line `vm.max_map_count=262144`
- sudo sysctl -p

#### For arch host persistence

- sudo vim /usr/lib/sysctl.d/10-arch.conf
- add new line vm.max_map_count=262144
- after reboot, it should persist


## Mailcatcher support

Mailcatcher service is included, can be accessed using URL and can be configured using smtp:-

```
smtp://mailcatcher:1025
```


## Sample project
A magento2 sample project using magento2 dev docker + github actions for continuous integration.

- https://github.com/readymadehost/magento2-dev-docker-sample


## Phpstorm setup

Simply add remote docker-compose php cli interpreter (exec with docker-compose.yml), change path mapping and configure remote interpreter for composer, phpunit, phpcs, phpcbf, phpmd and php-cs-fixer.


## Remote container extension + vscode

With vscode's remote container extension, we can simply connect into cli container.


## For development usages

- Clone this repo and pull on update. ReadyMadeHost cli tool coming soon...

## For production usages

- Current docker setup is for development only. Planning for ReadyMadeHost coming soon...
