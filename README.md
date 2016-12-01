
# Vagrant + Ansible development VM for Drupal 8 on PHP7
This project creates a VM for local Drupal development, built with Vagrant + Ansible. This is customized version of the the [Drupal VM](https://www.drupalvm.com/) project designed specifically for [Monarch Digital's](https://www.monarchdigital.com) workflow.

By default, the following will be installed on an Ubuntu 16.04 linux VM:

  - Apache 2.4.x
  - PHP 7.0.x
  - MySQL 5.7.x
  - Drush
  - Drupal 8.x.x
  - Drupal Console
  - XHProf
  - XDebug, for debugging your code
  - MailHog, for catching and debugging email

## Quick Start

To install a Drupal 8 site locally:

  1. Install [Vagrant](https://www.vagrantup.com/downloads.html) and [VirtualBox](https://www.virtualbox.org/wiki/Downloads).
  2. Download or clone this project.
  3. `cd` into this project directory and run `vagrant up`.

>Note: This project is meant to create a simple, cross-platform VM that installs the necessary software to run a Drupal site. By default, it does not install or configure vhosts, database, or Drupal.

To add a new development site:

  1. Create a `sites/example.com` directory under the project root for the website
  2. Download or clone the website code
  3. Create a new database on the VM
  4. (optional) Import an existing database
  5. Add a new vhost configuration at `/etc/apache2/sites-available/example.com.conf

```
<VirtualHost *:80>
  ServerName scug-debug.local
  DocumentRoot /var/www/example.local

  <Directory "/var/www/example.local">
    AllowOverride All
    allow from all
    Options +Indexes +FollowSymLinks
  </Directory>
  # PHP-FPM support
  ProxyPassMatch ^/(.*\.php(/.*)?)$ "fcgi://127.0.0.1:9000/var/www/example.local"
</VirtualHost>
```
  6. Add an entry in your hosts file on the host machine (`/etc/hosts` on Linux)


## License

This project is licensed under the MIT open source license.
