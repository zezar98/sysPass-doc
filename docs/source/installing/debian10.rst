Installation Debian 10
====================

Prerequisites
-------------

* Web server (Apache/Nginx/Lighttpd) with SSL enabled.
* MariaDB >= 10.1
* PHP >= 7.0
* PHP modules
    * mysql
    * curl
    * json
    * gd
    * xml
    * mbstring
    * intl
    * readline
    * ldap (optional)
    * mcrypt (optional for importing older XML export files)
* Latest sysPass version https://github.com/nuxsmin/sysPass/releases

Installation
------------

Debian GNU/Linux package installation.

.. code:: bash

  $ sudo apt install mariadb-server locales apache2 libapache2-mod-php php-pear php php-cgi \
    php-cli php-common php-fpm php-gd php-json php-mysql php-readline curl php-intl \
    php-ldap php-xml php-mbstring
  $ sudo /etc/init.d/apache2 restart


Optional for enabling SSL.

In order to increase your sysPass instance security, please consider to use SSL. See :doc:`/application/security` and the following resources for Debian:

* Sites only accessible from LAN: https://wiki.debian.org/Self-Signed_Certificate
* Sites accessible from Internet, you could use Let's Encrypt, see https://certbot.eff.org/

Directories and permissions
---------------------------

Create a directory for sysPass within the web server root.

.. code:: bash

  $ sudo mkdir /var/www/html/syspass

Unpack sysPass files.

.. code:: bash

  $ cd /var/www/html/syspass
  $ sudo tar xzf syspass.tar.gz

Setup directories permissions. The owner should match the web server running user.

.. code:: bash

  $ sudo chown www-data -R /var/www/html/syspass
  $ sudo chmod 750 /var/www/html/syspass/app/config /var/www/html/syspass/app/backup

Installing dependencies
-----------------------

From sysPass root directory, download and install Composer (https://getcomposer.org/doc/faqs/how-to-install-composer-programmatically.md)

Create a bash script called "install_composer.sh" and paste this code in it:

.. code:: bash

  #!/bin/sh
  EXPECTED_SIGNATURE="$(wget -q -O - https://composer.github.io/installer.sig)"
  php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
  ACTUAL_SIGNATURE="$(php -r "echo hash_file('sha384', 'composer-setup.php');")"

  if [ "$EXPECTED_SIGNATURE" != "$ACTUAL_SIGNATURE" ]
  then
      >&2 echo 'ERROR: Invalid installer signature'
      rm composer-setup.php
      exit 1
  fi

  php composer-setup.php --quiet
  RESULT=$?
  rm composer-setup.php
  exit $RESULT

.. code:: bash

  $ chmod +x install_composer.sh
  $ ./install_composer.sh

Then install sysPass dependencies

.. code:: bash

  $ php composer.phar install --no-dev

Environment configuration
-------------------------

Please, point your web browser to the following URL and follow the installer steps

https://IP_OR_SERVER_ADDRESS/syspass/index.php


.. note::

  More information about how sysPass works on :doc:`/application/index`

.. warning::

  It's very advisable to take a look to security advices on :doc:`/application/security`
