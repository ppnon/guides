# Install & Configuration

```bash
	
	sudo apt-get install nginx

	sudo systemctl status nginx
	sudo systemctl start/stop/restart nginx
	sudo systemctl reload nginx

	# test configuraion
	sudo nginx -t

```

default logs directory:

	/var/log/nginx/

default static resource directory:

	/var/www/html/

if you want to add a new static page use this stucture:

	/var/www/***my-sit.com***/html/

default configurations directory:

	/etc/nginx/

add a new site

	sudo vi /etc/nginx/sites/availables/my-site.com

```javascript
	
	server {
        listen 80;
        listen [::]:80;

        root /var/www/my-site.com/html;
        index index.html index.htm index.nginx-debian.html;

        server_name my-site.com www.my-site.com;

        location / {
                try_files $uri $uri/ =404;
        }
	}

```

```bash

sudo ln -s /etc/nginx/sites-availables/my-site.com /etc/nginx/sites-enabled/my-site.com

```

To avoid a possible hash bucket memory problem edit the file */etc/nginx/nginx.conf*
Uncomment the line of *server_names_hash_bucket_size*.

Configuration example:

```javascript

upstream docker.registry {
	server 127.0.0.1:5080;
}

server {
        listen 80;
        listen [::]:80;

        server_name registry.opendevj.com;
	return 301 https://registry.opendevj.com$request_uri;
}

server {

	listen 443 ssl http2;
	listen [::]:443 ssl http2;

	server_name registry.opendevj.com;

	ssl_protocols TLSv1.2;
        ssl_certificate /etc/nginx/ssl/nginx.crt;
        ssl_certificate_key /etc/nginx/ssl/nginx.key;

        add_header X-Frame-Options "SAMEORIGIN";
        server_tokens off;

        access_log /var/log/nginx/opendevj/registry.access.log;
        error_log /var/log/nginx/opendevj/registry.error.log;

        location / {
                proxy_pass http://docker.registry/;
		proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header  X-Forwarded-For   $proxy_add_x_forwarded_for;
		proxy_set_header  X-Forwarded-Proto $scheme;
		proxy_read_timeout                  900;
        }
}


```

# SSl

Create a self-signed certificate

```bash

sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/ssl/nginx.key -out /etc/nginx/ssl/nginx.crt

```
Complete the certificate information as follow:

- Country Name (2 letter code) [AU]:US
- State or Province Name (full name) [Some-State]:New York
- Locality Name (eg, city) []:New York City
- Organization Name (eg, company) [Internet Widgits Pty Ltd]:Bouncy Castles, Inc.
- Organizational Unit Name (eg, section) []:Ministry of Water Slides
- Common Name (e.g. server FQDN or YOUR name) []:your_domain.com
- Email Address []:admin@your_domain.com

# Referecias
- https://www.digitalocean.com/community/tutorials/how-to-create-an-ssl-certificate-on-nginx-for-ubuntu-14-04
- https://www.digitalocean.com/community/questions/configure-nginx-ssl-force-http-to-redirect-to-https-force-www-to-non-www-on-serverpilot-free-plan-using-nginx-configuration-file-only
