# PHP-Examples
PHP Code examples



## Installations

https://php.watch/articles/php-84-install-upgrade-guide-debian-ubuntu

PHP

```
sudo LC_ALL=C.UTF-8 add-apt-repository ppa:ondrej/php
sudo apt update
sudo apt install php8.4-cli php8.4-fpm
```

VS Code formatter

`composer global require friendsofphp/php-cs-fixer`

Extension *php cs fixer*

```json
"php-cs-fixer.executablePath": "/home/jano/.config/composer/vendor/bin/php-cs-fixer",
"[php]": {
    "editor.defaultFormatter": "junstyle.php-cs-fixer"
},
```

Composer & Symfony

```
sudo mv composer.phar /usr/local/bin/composer
curl -sS https://get.symfony.com/cli/installer | bash
sudo mv /home/jano/.symfony5/bin/symfony /usr/local/bin/symfony
symfony check:requirements
sudo apt install php8.4-intl
sudo apt install php8.4-mbstring
sudo apt install php8.4-simplexml
sudo apt install php8.4-sqlite3
sudo apt install php8.4-pgsql
```

PHP ini at `/etc/php/8.4/cli/php.ini`
