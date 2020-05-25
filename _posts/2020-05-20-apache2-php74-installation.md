---
layout: post
title: PHP 7.4 and Apache2 Installation on Ubuntu 16.04
tags: [Linux, PHP]
---

Recently I installed the recent versions of `PHP` and `Apache2`. In this post I will describe the steps I took to run the latest version of these tools on `Ubuntu 16.04`. I used to experiment a lot with `Linux` `Apache` `MySQL` and `PHP` (`LAMP`) configurations when I was studying my Bachelor's Degree.  The installation procedure remains very similar to the one ten years ago.


#### Install Personal Package Archives PPA

Here, I used the repository from ondrej.

```bash
$ sudo add-apt-repository -y ppa:ondrej/php
$ sudo apt-get update -y
```

#### Install packages

To list all packages available in the repository we can use `apt-cache` with option `pkgnames` and then filter the ones that contain the string `php7.4`. If we want the description of every package, we can use the option `search` followed with the keyword `php` and then filter by `7.4`.

```bash
$ apt-cache search php | grep 7.4

libapache2-mod-php7.4 - server-side, HTML-embedded scripting language (Apache 2 module)
libphp7.4-embed - HTML-embedded scripting language (Embedded SAPI library)
php7.4 - server-side, HTML-embedded scripting language (metapackage)
php7.4-bcmath - Bcmath module for PHP
php7.4-bz2 - bzip2 module for PHP
php7.4-cgi - server-side, HTML-embedded scripting language (CGI binary)
php7.4-cli - command-line interpreter for the PHP scripting language
php7.4-common - documentation, examples and common module for PHP
php7.4-curl - CURL module for PHP
php7.4-dba - DBA module for PHP
php7.4-dev - Files for PHP7.4 module development
php7.4-enchant - Enchant module for PHP
php7.4-fpm - server-side, HTML-embedded scripting language (FPM-CGI binary)
php7.4-gd - GD module for PHP
php7.4-gmp - GMP module for PHP
php7.4-imap - IMAP module for PHP
php7.4-interbase - Interbase module for PHP
php7.4-intl - Internationalisation module for PHP
php7.4-json - JSON module for PHP
php7.4-ldap - LDAP module for PHP
php7.4-mbstring - MBSTRING module for PHP
php7.4-mysql - MySQL module for PHP
php7.4-odbc - ODBC module for PHP
php7.4-opcache - Zend OpCache module for PHP
php7.4-pgsql - PostgreSQL module for PHP
php7.4-phpdbg - server-side, HTML-embedded scripting language (PHPDBG binary)
php7.4-pspell - pspell module for PHP
php7.4-readline - readline module for PHP
php7.4-snmp - SNMP module for PHP
php7.4-soap - SOAP module for PHP
php7.4-sqlite3 - SQLite3 module for PHP
php7.4-sybase - Sybase module for PHP
php7.4-tidy - tidy module for PHP
php7.4-xml - DOM, SimpleXML, XML, and XSL module for PHP
php7.4-xmlrpc - XMLRPC-EPI module for PHP
php7.4-xsl - XSL module for PHP (dummy)
php7.4-zip - Zip module for PHP
```
Here, we can install all related packages or only the ones we need for our application. To start experimenting, I always prefer the minimal installation, then add the new modules according to my needs. In this way I learn the purpose of each module.


```bash
$ sudo apt-get install php7.4 php7.4-cli php7.4-common
...
...
...
The following NEW packages will be installed:
  apache2 apache2-bin apache2-data apache2-utils libapache2-mod-php7.4
  libaprutil1-dbd-sqlite3 libaprutil1-ldap libargon2-0 libpcre2-8-0 libsodium23
  libssl1.1 php7.4 php7.4-cli php7.4-common php7.4-json php7.4-opcache
  php7.4-readline
```

As we can see in the previous output, a minimal `Apache2` server has been installed automatically as a dependency. If not, we can install the package with the following:

```bash
sudo apt-get install apache2
```

Additionally, I will install the required packages to work with the `symfony` `PHP` framework.

```bash
$ sudo apt-get install php7.4 php7.4-cli php7.4-common php7.4-xml php7.4-mbstring php7.4-intl
```

#### Test the installation

At this point a web server with `PHP` support is running. We can go to `http://127.0.0.1` and see the old good `It works!`. On this page we can get useful information about the configuration files in an `Ubuntu` system.


To test our `PHP` installation we can use the `cli` as follows:

```bash
$ php -v
PHP 7.4.6 (cli) (built: May 14 2020 10:02:18) ( NTS )
Copyright (c) The PHP Group
Zend Engine v3.4.0, Copyright (c) Zend Technologies
    with Zend OPcache v7.4.6, Copyright (c), by Zend Technologies
```

We can also get detail information about its configuration by creating the following script on the default Document Root `/var/www/html/`:

```php
<?php
phpinfo();
?>
```

After saving the script with the name `info.php`, we can access it with the URL `http://127.0.0.1/info.php`.












