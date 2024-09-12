---
tags:
  - H2
  - Linux
  - Wordpress
  - Skole
---
Installeret på [[Webserver]].

## Technical
#### Wordpress Config
Filnavn: 'wp-config.php' stammer fra 'wp-config-sample.php'
- DB_NAME: 'wordpress'
- DB_USER: 'root'
- DB_PASS: 'Passw0rd'
- DB_HOST: 'sql.denlange.local'
- DB_CHARSET: 'utf8'
- DB_COLLATE: ''

Inde i config filen findes også nogle unikke nøgler der skal have deres salt sat.
Dette kan gøres med: https://api.wordpress.org/secret-key/1.1/salt/

Når hjemmesiden virker og spørger om sprog og bruger opsætning udfyld med dette:
Sprog: Dansk
Sidetitel: Denlange
Brugernavn: admin
Password: Passw0rd
Email: patrick@denlange.local

#### Database
Lavet en ny database der hedder:
- wordpress

Granted all adgang til:
- root@'www.denlange.local'

#### DNS
Lavet en ny vhost entry i '/etc/httpd/conf.d/vhost.conf' for wordpress installation:
```conf
<VirtualHost *:80 *:443>
	DocumentRoot /var/www/html/wordpress
	ServerName www.wordpress.local
	ErrorLog logs/wordpress-error_log
	CustomLog logs/wordpress-access_log common
	<Directory "/var/www/html/wordpress">
		DirectoryIndex index.html index.php
		Options FollowSymLinks
		AllowOverride All
		Require all granted
	</Directory>
</VirtualHost>
```


#### Wordpress Plugins
- All-In-One Intranet
- The Events Calendar
- UpStream
- WpDevArt Organization Chart

#### Wordpress Tema
Navnet på Temaet er:
- Astra

Primær navigation indeholder:
- Home
- WebServer 
- Begivenheder

Home siden indeholder:
- Et pænt billede
- En gul knap der linker til wordpress projects
- Længere nede finder man et organizations chart
- Til sidst finder man en simple liste af planlagte begivenheder.

WebServer indeholder linket til projects/WebServer.
Begivenheder indeholder linket til alle planlagte begivenheder.