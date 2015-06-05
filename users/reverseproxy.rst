.. _reverse-proxy-setup:

Reverse Proxy Setup
===================

Server Configuration
--------------------

Use the following examples to link /syncthing of your web server to Syncthing's WebUI hosted on localhost:8384

Apache
~~~~~~

    ProxyPass /syncthing/ http://localhost:8080/
    <Location /syncthing/>
    \  ProxyPassReverse http://localhost:8080/
    \  Order Allow,Deny
    \  Allow from All
    </Location>

Nginx
~~~~~

    location /syncthing/ {
      proxy_set_header        Host $host;
      proxy_set_header        X-Real-IP $remote_addr;
      proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header        X-Forwarded-Proto $scheme;
	  
      proxy_pass              http://localhost:8384/;
    }


Folder Configuration
--------------------

If you don't have access to your web server configuration files you might try the following technique.

Apache
~~~~~~

Place .htaccess file in the folder of your webroot which should redirect to the WebUI, "/syncthing" to produce the same behaviour as above.

    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteCond %{ENV:HTTPS} !=on
    RewriteRule .* https://%{SERVER_NAME}%{REQUEST_URI} [R=301,L]
    RewriteRule ^(.*) http://localhost:8080/$1 [P]

	
