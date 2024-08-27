Intro
# Steps to Launch a new wordpresse isntall: 
0. Requirement : 
	- Docker and docker compose is installed on your server
	- Apach2 HTTPD is up and runnng on your server
	- you have Let's Encrypt certbot installed on you server.
	- the DNS of YOUR_SITE.com is pointing at the IP adress of your server (Change A Record, or chnage locally your /etc/hosts with the entry YOUR_SERVER_IP YOUR_SITE.com)

1. Change site.com to YOUR_SITE.com in site.conf file and also the port to an available port on your server.

2. - Activate redirection from Apache to you new wordpress site.
    ```cp site.conf /etc/apache2/sites-available/.
    cd /etc/apache2/sites-available/
    a2ensite site.conf # For new virtual host activation.
    systemctl reload apache2
    certbot -d YOUR_SITE.com # For Let's encrypt certificate generation.
    # Some Times you need to : 
        a2dissite site-le-ssl.conf
        # add the line "ProxyPreserveHost On" on the 443 conf part. For unknow reason, Let's encrypt certbot doesnot copy the line.
        a2ensite site-le-ssl.conf
        systemctl reload apache2```

If everything is Ok, you should have a "Service unvailable" when you acces the page https://[YOUR_SITE.com]

3. change the port in the docker-compose file from 3000 to any available protected (keep the same port as in site.conf).
    `docker-compose up -d`


4. Visit the url :  https://[YOUR_SITE.com]


5. - if you see that the pages is not laoded correctl change the follwoing lines of the file wp-config.php: 

```define( 'WP_HOME', 'https://[YOUR_SITE]/' );
define( 'WP_SITEURL', 'https://[YOUR_SITE]/' );
define('FORCE_SSL_LOGIN', true);
define('FORCE_SSL_ADMIN', true);
define( 'CONCATENATE_SCRIPTS', false );
define( 'SCRIPT_DEBUG', true );
$_SERVER['HTTPS'] = 'on';```


