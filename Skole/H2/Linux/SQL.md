---
tags:
  - H2
  - Linux
  - Webserver
  - Skole
---
Bruger [[Master]] som grundlag

### Technical
IPv4 Config:
Manual
Addresse: 10.0.0.3
Gateway: 10.0.0.1
DNS Server: 10.0.0.2
Search Domain: denlange.local

## Opsætning
### SQL
Kopier config sample til aktuel config.
Ret config.inc.php med følgende ændringer:
- Blowfish_secret er fyldt ud med garbage.
- ```$cfg['Servers'][$i]['host'] = '10.0.0.3'```

### phpMyAdmin
Opret en database med navnet "tec" og tabellen "Medlemmer" med columns:
- Navn varchar(255) primary
- Adresse varchar(255)

### WebServer
På webserver, lav en virtual hostfil i mappen "/etc/httpd/conf.d/" kaldet "phpmyadmin.conf" med følgende inde i:
```
Alias /phpmyadmin /usr/share/phpMyAdmin

<Directory /usr/share/phpMyAdmin/>
	AddDefaultCharset UTF-8
	<IfModule mod_authz_core.c>
		# Apache 2.4
		<RequireAny>
		 Require all granted
		</RequireAny>
	</IfModule>
</Directory>

<Directory /usr/share/phpMyAdmin/setup/>
	<IfModule mod_authz_core.c>
		# Apache 2.4
		<RequireAny>
		 Require all granted
		</RequireAny>
	</IfModule>
</Directory>
```