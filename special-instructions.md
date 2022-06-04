# Special instructions

* Install vagrant
* Install virtualbox
* clone repository

		git clone git@github.com:kiklop74/homestead.git [somepath]/homestead
		cd [somepath]/homestead
		git checkout release

		# On linux/mac
		bash init.sh

		# On Windows
		init.bat

* Open the file [somepath]/homestead/after.sh in text editor and make it look like this:

		#!/bin/bash

		sudo apt-get -y purge nodejs
		sudo rm -rf /usr/lib/node_modules/npm/lib
		sudo rm -rf //etc/apt/sources.list.d/nodesource.list

		curl -o- 'https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh' | bash
		export NVM_DIR='/home/vagrant/.nvm'
		. /home/vagrant/.nvm/nvm.sh
		nvm install v8.9.3
		npm install --global yarn

* Open the file [somepath]/homestead/Homestead.yaml in text editor and modify following elements:
  * change authorize field to point to valid SSH key pub file
  * modify keys to point to valid private SSH key
  * modify folders, subkey map to point to your directory where uhcp code repository is cloned

* Adjust Laravel .env file elements to look like this:
  
		APP_URL=https://homestead.test
		# ...
		DB_CONNECTION=mysql
		DB_HOST=localhost
		DB_PORT=3306
		DB_DATABASE=homestead
		DB_USERNAME=homestead
		DB_PASSWORD=secret
		DB_CHARSET=utf8mb4
		DB_COLLATION=utf8mb4_unicode_ci  
        # ...
		BROADCAST_DRIVER=log
		CACHE_DRIVER=file
		SESSION_DRIVER=file
		SESSION_DOMAIN=homestead.test
		QUEUE_DRIVER=sync
		UNHOSTING_APPLICATION_MOODLE_CACHE_DIR=/home/vagrant/code/storage/moodlecache
		UNHOSTING_APPLICATION_FREE_DOMAIN=*.uhcpdev.net
		UNHOSTING_SSL_CERTIFICATE_DIR=/home/vagrant/code/storage/certs

## post config

* Execute vagrant up
* Enter the VM (vagrant ssh)
* Execute these steps:
		
		# Set php 7.3 as default php (execute only once)
		sudo update-alternatives --config php

		cd /home/vagrant/code
		composer install
		# you will be asked this:
		# Do you trust "slince/composer-registry-manager" to execute code and wish to enable it now? (writes "allow-plugins" to composer.json) [y,n,d,?
		# Respond n
		php artisan uhcp:devdata
		npm install
		npm run dev
* After some additional configuration access the site in the browser using https://homestead.test


 		