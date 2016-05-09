# Yii 2 Another Project Template

[![Latest Stable Version](https://poser.pugx.org/delagics/yii2-app-another/v/stable.svg)](https://packagist.org/packages/delagics/yii2-app-another)
[![Total Downloads](https://poser.pugx.org/delagics/yii2-app-another/downloads)](https://packagist.org/packages/delagics/yii2-app-another)
[![Latest Unstable Version](https://poser.pugx.org/delagics/yii2-app-another/v/unstable.svg)](https://packagist.org/packages/delagics/yii2-app-another)
[![Code Climate](https://codeclimate.com/github/delagics/yii2-app-another/badges/gpa.svg)](https://codeclimate.com/github/delagics/yii2-app-another)
[![License](https://poser.pugx.org/delagics/yii2-app-another/license.svg)](https://packagist.org/packages/delagics/yii2-app-another)

Yii 2 Another Project Template is a skeleton [Yii 2](http://www.yiiframework.com/) application best for
developing complex Web applications with multiple tiers.

The template includes three tiers: front, back, and console, each of which
is a separate Yii application.

The template is designed to work in a team development environment. It supports
deploying the application in different environments.

## What's inside:

- Adjusted language management through URLs with help of `codemix/yii2-localeurls` [[see](https://github.com/codemix/yii2-localeurls)];
- Flexible user registration and authentication module `dektrium/yii2-user` [[see](https://github.com/dektrium/yii2-user)];
- RBAC management module `dektrium/yii2-rbac` [[see](https://github.com/dektrium/yii2-rbac)];
- Added `yii app` command with help of which it is easier to prepare your database [[see](https://github.com/delagics/yii2-app-another/blob/master/app/console/controllers/AppController.php)].


## Installation

### Requirements

The minimum requirement by this project template is that your Web server supports PHP 5.4.0.
Recommended is PHP 7.

### Installing using Composer

If you do not have [Composer](http://getcomposer.org/), follow the instructions in the
[Installing Yii](https://github.com/yiisoft/yii2/blob/master/docs/guide/start-installation.md#installing-via-composer) section of the definitive guide to install it.

With Composer installed, you can then install the application using the following commands:

    composer global require "fxp/composer-asset-plugin:~1.1.3"
    composer create-project --prefer-dist delagics/yii2-app-another another.dev

The first command installs the [composer asset plugin](https://github.com/francoispluchino/composer-asset-plugin/)
which allows managing bower and npm package dependencies through Composer. You only need to run this command
once for all. The second command installs the _another_ application in a directory named `another.dev`.
You can choose a different directory name if you want.

## Install by `git clone`

Clone repository as a web root.
```
git clone git@github.com:delagics/yii2-app-another.git another.dev
```

A directory named `another.dev` is your Web root.
Then `cd` to the `app` folder and run:
```
composer install
```
Then follow the instructions given in the next subsection.


## Preparing application

After you install the application, you have to conduct the following steps to initialize
the installed application. You only need to do these once for all.

1. Open a console terminal in `app` directory, and execute the `php init` command and select `dev` as environment.
```
php init
```
Otherwise, in production execute `php init` in non-interactive mode.
```
php init --env=Production --overwrite=All
```

2. Create a new database and adjust the `components['db']` configuration in `base/config/main-local.php` accordingly.

3. Open a console terminal in `app` folder, and run command:
```
php yii app/set-up
```

4. Set document roots of your web server:

   - for `/path/to/another.dev/public/` and using the URL `http://another.dev/`

   For **nginx** it could be the following:

```nginx
    server {
        listen 80;
        charset utf-8;
        client_max_body_size 128M;
        root /var/www/another.dev/public;
        server_name another.dev www.another.dev;
        index index.php index.html index.htm;
        # access_log /var/www/another.dev/logs/access_log.txt;
        # error_log /var/www/another.dev/logs/error_log.txt error;
        location / {
            try_files $uri $uri/ /index.php?$args;
        }
        location /admin {
            try_files $uri $uri/ /admin/index.php?$args;
        }
        location ~ \.php$ {
            #NOTE: You should have "cgi.fix_pathinfo = 0;" in php.ini
            try_files $uri =404;
            include fastcgi_params;
            fastcgi_index index.php;
            fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        }
        location /storage {
            # Deny access to any files with a .php extension in the storage directory
            location ~ \.php$ {
                deny all;
            }
            location ~* ^/.+\.(ogg|ogv|svg|svgz|eot|otf|woff|mp4|ttf|rss|atom|jpg|jpeg|gif|png|ico|zip|tgz|gz|rar|bz2|doc|xls|exe|ppt|tar|mid|midi|wav|bmp|rtf)$ {
                expires max;
                add_header Cache-Control public;
                log_not_found off;
                access_log off;
            }
        }
        # Deny all attempts to access hidden files and folders such as .git, .htaccess, .htpasswd, .DS_Store.
        location ~ /\. {
            deny all;
        }
        location = /favicon.ico {
            log_not_found off;
            access_log off;
        }
        location = /robots.txt {
            allow all;
            log_not_found off;
            access_log off;
        }
    }
```

5. Change the hosts file to point the domain to your server.

   - Windows: `c:\Windows\System32\Drivers\etc\hosts`
   - Linux: `/etc/hosts`

   Add the following lines:

   ```
   127.0.0.1   another.dev
   ```

To login into the application, use your username and password created when running `php yii app/set-up` command, or sing up with new credentials.

## Generating apps translations

```
# For frontend:
php yii message front/messages/config.php

# For backend:
php yii message back/messages/config.php
```

DIRECTORY STRUCTURE
-------------------

```
app/
    base/                    contains classes shared between all apps
        config/              contains shared configurations
        mail/                contains view files for e-mails
        models/              contains model classes used in both backend and frontend apps
    console/                 contains classes for the console app
        config/              contains console configurations
        controllers/         contains console controllers (commands)
        migrations/          contains database migrations
        models/              contains console-specific model classes
        runtime/             contains files generated during runtime
    back/                    contains classes for the backend app
        assets/              contains backend app assets
        config/              contains backend configurations
        controllers/         contains backend Web controller classes
        models/              contains backend-specific model classes
        runtime/             contains files generated during backend app runtime
        views/               contains view files for the backend Web app
    front/                   contains classes for the frontend app
        assets/              contains frontend app assets
        config/              contains frontend configurations
        controllers/         contains frontend Web controller classes
        models/              contains frontend-specific model classes
        runtime/             contains files generated during frontend app runtime
        views/               contains view files for the frontend Web app
        widgets/             contains frontend widgets
    vendor/                  contains dependent 3rd-party packages
    env/                     contains environment-based overrides
public/                      contains the entry script and Web resources for the frontend app
    storage/                 contains files such as user uploaded images, docs, archives shared between all appliactions
    admin/                   contains the entry script and Web resources for the backend app
        assets/              contains Web resources published by AssetBundles of backend app
        index.php            contains the entry script of the backend app
    assets/                  contains Web resources published by AssetBundles of the frontend app
    index.php                contains the entry script of the frontend app
```

*Made with :heart: by @delagics*