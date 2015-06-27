#yaamp

Required:

	memcached

Config for nginx:

	location / {
		try_files $uri @rewrite;
	}

	location @rewrite {
		rewrite ^/(.*)$ /index.php?r=$1;
	}

	location ~ \.php$ {
		fastcgi_pass unix:/var/run/php5-fpm.sock;
		fastcgi_index index.php;
		include fastcgi_params;
	}

Apache should be something like:

	RewriteEngine on

	RewriteCond %{REQUEST_FILENAME} !-f
	RewriteRule ^(.*) index.php?r=$1 [QSA]

Recommended install folder for the stratum engine is in /var/stratum. Copy all the .conf files, run.sh, the stratum binary and the blocknotify binary to this folder. Some scripts are expecting the web folder to be /var/web. 

Add your exchange API keys in:

	web/yaamp/core/exchange/*

Edit web/serverconfig.php

You need three backend shells (in screen) running these scripts:

	web/main.sh
	web/loop2.sh
	web/block.sh

Start one stratum per algo using the run.sh script in the config folder, where the x11.conf is located as in:

	stratum x11

Edit each .conf file with proper values.

Look at rc.local, it starts all three backend shells and all stratum processes.

All your coin's config files need to blocknotify their corresponding stratum using something like:

	blocknotify=/var/stratum/blocknotify yaamp.com:port coinid %s

On the website, go to http://server.com/site/frottedessus to login as admin. You have to change it to something different in the code (web/yaamp/modules/site/SiteController.php).


More instructions coming as needed.


