# Nginx

## Installation

    sudo apt install nginx

## Configuration

config file path : `/etc/nginx/~`

    nginx.conf
    sites-available/default

## Maximum file transfer size

nginx.conf

    http {
        ...
        client_max_body_size 1M
    }

    or

    location / {
        client_max_body_size 250M;
        proxy_pass http://127.0.0.1:8080;
    }

after saving

    sudo service nginx reload

## Check ngx modules

    nginx -V 2>&1 | grep -- 'http_auth_request_module'

## server block

    server {
        listen <port>; # Connection port
        server_name <host>; # IP and Domain, _ Available
        root /var/www/html; # File Path
        index index.html index.nginx-debian.html; # NGINX searches for files in the specified order and returns the first one it finds.

        # When connecting to "<PUBLIC_IP>/"
        location / {
            # Also available here
            root ~
            index ~

            ​# First attempt to serve request as file, then
            ​# as directory, then fall back to displaying a 404.
            try_files $uri $uri/ =404;

            # Proxy (URL not changed)
            proxy_pass http://127.0.0.1:PORT;

            # Proxy header
            proxy_set_header Host $host; # See description below
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;

            # Redirect (URL changed)
            return 302 http://127.0.0.1:PORT;
        }

        # different location
        location /someRoute {
            ...
        }
    }

**proxy_set_header Host**

- `$http_host` : **host** of `request header`.

- `$host` : return **server_name** if `host` of `request header` is empty.

## Variable

    server {
        set $value cookie_value # add variable

        if ($cookie_id != $value) {
            return 204;
        }
        ...
    }

## websocket

If you need to use Websocket, write as follows:

    location / {
        proxy_http_version 1.1; # For websockets and keepalive connections (default 1.0)
        proxy_set_header Upgrade $http_upgrade; # Switch protocol to `web socket`
        proxy_set_header Connection "upgrade"; # Switch protocol to `web socket`

        proxy_pass http://127.0.0.1:8443;
    }

## Priority

3000 port reached when connecting to `/a`

    location / {
        proxy_pass http://127.0.0.1:3000;
    }
    location /a {
        proxy_pass http://127.0.0.1:4000;
    }

4000 port reached when connecting to `/a`

    location / {
        proxy_pass http://127.0.0.1:3000;
    }
    location = /a {
        proxy_pass http://127.0.0.1:4000;
    }

File download when connecting to `/[filename].tar`

    location / {
        proxy_pass http://127.0.0.1:3000;
    }
    location ~ \.tar$ {
        root /home/ubuntu/files;
        try_files $uri $uri/ =404;
    }

## Regex

    # Detect only "/"
    location = / {
        return 302 http://<host>/a
    }

    # Regular expression detection
    location ~ ^/(a|b|c|...) { ... } # Case sensitive
    location ~* ^/(a|b|c|...) { ... } # Not case sensitive
    location ^~ ^/(a|b|c|...) { ... } # Best non-regular expression match

    # When a regular expression fails
    if ($http_user_agent !~ (Chrome|Firefox)) {
        return 444;
    }
    if ($request_method !~ ^(GET|POST|PUT|PATCH|DELETE)$) {
        return 444;
    }

## error_page

Understanding error_page

    # proxy_intercept_errors OFF
    # error_page             ON
    Client >>>> Nginx >>>> proxy_pass >>>> 127.0.0.1:4000
      502 <<<<<<<<<<<<<<<<<<< 502
      444 <<<<< CATCH <<<<<<< 502
     file <<<<< CATCH <<<<<<< 502
      403 <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< 403

    # proxy_intercept_errors ON
    # error_page             ON
    Client >>>> Nginx >>>> proxy_pass >>>> 127.0.0.1:4000
      502 <<<<<<<<<<<<<<<<<<< 502
      444 <<<<< CATCH <<<<<<< 502
     file <<<<< CATCH <<<<<<< 502
      444  <<<< CATCH <<<<<<<<<<<<<<<<<<<<<<<<  403
      403  <<<< CATCH <<<<<<<<<<<<<<<<<<<<<<<<  403
     file  <<<< CATCH <<<<<<<<<<<<<<<<<<<<<<<<  403

Code

    root /var/www/html;

    sudo nano /var/www/html/error.txt # Create file
    > Invalid approach detected. your IP has been recorded.

    sudo nano /etc/nginx/sites-available/default
    > server {
    >   ...
    >   location / {
    >       ...
    >       proxy_intercept_errors on;
    >       proxy_pass http://127.0.0.1:51425;
    >   }
    >   error_page 400 401 402 403 404 /error.txt;
    >   location = /error.txt {
    >       internal; # Specifies that a given location can only be used for internal requests.
    >   }
    > }

Different usage

    error_page [status_code] =444 /error.txt; // Entered states codes are converted to 444

    error_page [status_code] @error;
    location @error { ... }

## prevent processing requests with undefined server names

If requests without the “Host” header field should not be allowed, a server that just drops the requests can be defined

> **HTTP1.0** : return 444
>
> **HTTP1.1** : return 400

    server {
        listen      80;
        server_name "";
        return      444;
    }

test

    curl -A "Firefox" -H "Host:" [host] # User-Agent : "Firefox", Host : empty
    curl -X "" [host] # Method : empty
    curl -0 -H "Host:" [host] # Host : empty, HTTP1.0

## IP

    http, server, location, limit_except {
        allow IP | CIDR | all;
        deny IP | CIDR | all;
    }

## Cookie

`add_header Set-Cookie ~` : Save cookies to browser.

    set $value eiFNzXSJRYLQuleaD8J2ESVqMF5aWQ0R;

    location = /key {
        if ($query_string = id) {
            add_header Set-Cookie "id=$value;Domain=$host;Path=/;Max-Age=14400";
            return 301 http://domain
        }
        return 204;
    }
    location / {
        if ($cookie_id != $value) {
            return 204;
        }
        ...
    }

## Limit methods

    # 1
    if ($request_method !~ ^(GET|POST|PUT)$) {
        return 444;
    }

    # 2
    location / {
        limit_except GET POST { # includes HEAD
            allow IP | CIDR | all; (optional)
            deny all;
        }
    }

1 : `status` can be changed.

2 : `status` is fixed to 403.

## Apply configs

Must be reloaded after setting.

    sudo nginx -t
    sudo service nginx reload

## Log

    /etc/nginx/nginx.conf

    # custom log
    log_format custom '$remote_addr - $remote_user [$time_local] "$request_method $request_uri $server_protocol" $status $body_bytes_sent "$http_referer" "$http_user_agent" ($server_name)';

    # Apply log
    access_log /var/log/nginx/access.log; # Default : combined
    access_log /var/log/nginx/access.log custom;

## Clear logs

    sudo truncate -s 0 /var/log/nginx/access.log

    or

    sudo mv /var/log/nginx/access.log /var/log/nginx/access.log.old
    sudo service nginx reload

Clean every sunday

    nano script/clean-nginx-log.sh
    > sudo mv /var/log/nginx/access.log /var/log/nginx/access.log.old
    > sudo service nginx reload

    chmod +x script/clean-nginx-log.sh

    crontab -e
    > sudo truncate -s 0 /var/log/nginx/access.log

    or

    > 50 19 * * 7 bash script/clean-nginx-log.sh

## Default comments

        ##
        # You should look at the following URL's in order to grasp a solid understanding
        # of Nginx configuration files in order to fully unleash the power of Nginx.
        # https://www.nginx.com/resources/wiki/start/
        # https://www.nginx.com/resources/wiki/start/topics/tutorials/config_pitfalls/
        # https://wiki.debian.org/Nginx/DirectoryStructure
        #
        # In most cases, administrators will remove this file from sites-enabled/ and
        # leave it as reference inside of sites-available where it will continue to be
        # updated by the nginx packaging team.
        #
        # This file will automatically load configuration files provided by other
        # applications, such as Drupal or Wordpress. These applications will be made
        # available underneath a path with that package name, such as /drupal8.
        #
        # Please see /usr/share/doc/nginx-doc/examples/ for more detailed examples.
        ##

        # Virtual Host configuration for example.com

        # You can move that to a different file under sites-available/ and symlink that
        # to sites-enabled/ to enable it.

        server {
            # SSL configuration

            # listen 443 ssl default_server;
            # listen [::]:443 ssl default_server;
            #
            # Note: You should disable gzip for SSL traffic.
            # See: https://bugs.debian.org/773332
            #
            # Read up on ssl_ciphers to ensure a secure configuration.
            # See: https://bugs.debian.org/765782
            #
            # Self signed certs generated by the ssl-cert package
            # Don't use them in a production server!
            #
            # include snippets/snakeoil.conf;

            listen 80;
            listen [::]:80;

            server_name example.com;

            # Add index.php to the list if you are using PHP
            root /var/www/example.com;
            index index.html;

            location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
            }

            # pass PHP scripts to FastCGI server

            location ~ \.php$ {
                include snippets/fastcgi-php.conf;

                # With php-fpm (or other unix sockets):
                fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
                # With php-cgi (or other tcp sockets):
                fastcgi_pass 127.0.0.1:9000;
            }

            # deny access to .htaccess files, if Apache's document root
            # concurs with nginx's one

            location ~ /\.ht {
                deny all;
            }
        }

---

# Security

Remove `nginx version`

    /etc/nginx/nginx.conf

    http {
        server_tokens off;
    }

Disable symlinks

    /etc/nginx/nginx.conf

    http {
        disable_symlinks on;
    }
