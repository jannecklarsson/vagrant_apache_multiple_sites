# Vagrant apache osx localhost multiple sites

In order to have multiple development sites localy on a vagrant-server with apache you need to setup apache and etc/hosts correctly.

## Step by step (in vagrant box)
```
$ cd /etc/apache2/sites-available/
$ nano default.conf
$ paste below "Virtual host code" (edit code folders to match your desires)
$ sudo a2dissite 000-default.conf (disable previous default file)
$ sudo a2ensite default.conf
$ sudo a2enmod vhost_alias
$ sudo service apache2 reload
```

## Virtual host code
```
# Default virtual host
<VirtualHost *:80>
	ServerName vagrant.dev
	ServerAdmin webmaster@localhost
	ServerAlias vagrant.dev
	DocumentRoot /var/www/html
	ErrorLog /var/www/log/error.log
	CustomLog /var/www/log/access.log combined
</VirtualHost>
# Variable virtual host
<Virtualhost *:80>
    VirtualDocumentRoot "/var/www/html/%1/public"
    ServerName vagrant.dev
    ServerAlias *.dev
    ErrorLog /var/www/log/error.log
    CustomLog /var/www/log/access.log combined
    <Directory "/var/www/html/*">
        Options Indexes FollowSymLinks MultiViews
        AllowOverride All
        Order allow,deny
        Allow from all 
    </Directory>    
</Virtualhost>
```
## Edit /etc/hosts (osx)
vagrant.dev is used to present whats in /var/www/html and test.dev points to /var/www/html/test/public.
```
127.0.0.1	vagrant.dev
127.0.0.1	test.dev
```
