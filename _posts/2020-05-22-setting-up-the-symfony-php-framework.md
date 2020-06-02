---
layout: post
title: Setting up the Symfony PHP Framework and Composer
tags: [Linux, PHP]
---

I have been thinking for a while about adopting a `PHP` Framework. In the past, I heard about Zend, CakePHP, and Laravel. A few years ago I read about Yii and `symfony`. Unfortunately, my experience with `PHP` Frameworks comes from a company's own private development. The good thing is that the concepts I learned can be applied to learn any other Framework easily. This time I would like to dig deeper into the inner workings of the `symfony` Framework.

#### Requirements

Fortunately, the project [1] is very well documented and there were no major issues during the installation process. In the documentation is specified that a `PHP` version >= 7.2.5  and certain `PHP` extensions are required to run `symfony`. Additionally, I installed the `PHP composer` dependency manager. The steps are described as follows:

##### Install required packages

```bash
$ sudo apt-get install php7.4 php7.4-cli php7.4-common php7.4-xml php7.4-mbstring php7.4-intl
```

##### Install composer

`Symfony` will download a copy of `composer` if not existent. It is recommended to install it yourself. The installation steps are very well described on the project's page [2].

Get the installation file:

```
$ php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
```

Verify integrity:

```
$ php -r "if (hash_file('sha384', 'composer-setup.php') === 'e0012edf3e80b6978849f5eff0d4b4e4c79ff1609dd1e613307e16318854d24ae64f26d17af3ef0bf7cfb710ca74755a') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"

Installer verified:
```

Install:

Install in `/usr/local/bin/` with name `composer` and make it available for all users:

```
sudo php composer-setup.php --filename=composer --install-dir=/usr/local/bin/

All settings correct for using Composer
Downloading...

Composer (version 1.10.6) successfully installed to: /usr/local/bin/composer
Use it: php /usr/local/bin/composer

```

Clean the installation:

```
$ php -r "unlink('composer-setup.php');"
```

Finally, we test the installation:

```
$ composer
   ______
  / ____/___  ____ ___  ____  ____  ________  _____
 / /   / __ \/ __ `__ \/ __ \/ __ \/ ___/ _ \/ ___/
/ /___/ /_/ / / / / / / /_/ / /_/ (__  )  __/ /
\____/\____/_/ /_/ /_/ .___/\____/____/\___/_/
                    /_/
Composer version 1.10.6 2020-05-06 10:28:10

```

#### Install Symfony CLI.

We can install the `Symfony` Command Line Interface (CLI) as follows:

```
$ wget https://get.symfony.com/cli/installer -O - | bash

...
...
...

Symfony CLI installer

Environment check
  [*] cURL is installed
  [*] Gzip is installed
  [*] Git is installed
  [*] You architecture (amd64) is supported
...
...
...
The Symfony CLI v4.15.0 was installed successfully!

```

After executing the installation script a binary file called `symfony` was created in `~/.symfony/bin/`. To configure the environment for the current user we add `export PATH="$HOME/.symfony/bin:$PATH"` to the `~/.bashrc` file. To make the changes take effect, we can reopen the shell or execute `source ~/.bashrc`. 

Reminder: `~/` refers to the user's `home` directory.


Another alternative that was suggested is making available the binary to all the users in the system as follows:

```
mv ~/.symfony/bin/symfony /usr/local/bin/symfony
```

At this point `symfony` is ready. We can test it on our shell as follows:

```
$ symfony
Symfony CLI version v4.15.0 (c) 2017-2020 Symfony SAS
Symfony CLI helps developers manage projects, from local code to remote infrastructure
...
...
...
```

Finally, we check if all the necessary modules to start creating applications are installed.

```
$ symfony check:requirements

Symfony Requirements Checker
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

> PHP is using the following php.ini file:
/etc/php/7.4/cli/php.ini

> Checking Symfony requirements:

............................W

 [OK]
 Your system is ready to run Symfony projects

```

Note: the first time I ran the checker, `symfony` could not find my `PHP` installation. I also had to install the package `php-fpm7.4`.

#### References

[1] https://symfony.com/doc/current/setup.html#technical-requirements

[2] https://getcomposer.org/
