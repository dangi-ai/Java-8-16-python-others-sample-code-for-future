## Option 1
```sh

# Redirect www to non-www
server {
    server_name www.dynamicpartnersusa.com;
    return 301 https://dynamicpartnersusa.com$request_uri;
}

# Main server block for non-www
server {
    listen 443 ssl;
    server_name dynamicpartnersusa.com;
    root /var/www/html;
    index index.html;

    ssl_certificate /etc/letsencrypt/live/dynamicpartnersusa.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/dynamicpartnersusa.com/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

    location / {
        try_files $uri $uri/ /index.html;
    }
}


```

## Option 2

```sh
# Redirect non-www to www
server {
    server_name dynamicpartnersusa.com;
    return 301 https://www.dynamicpartnersusa.com$request_uri;
}

# Main server block for www
server {
    listen 443 ssl;
    server_name www.dynamicpartnersusa.com;
    root /var/www/html;
    index index.html;

    ssl_certificate /etc/letsencrypt/live/dynamicpartnersusa.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/dynamicpartnersusa.com/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

    location / {
        try_files $uri $uri/ /index.html;
    }
}


```
