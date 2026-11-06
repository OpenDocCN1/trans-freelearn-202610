```
rewrite ^/owncloud/.well-known/host-meta /public.php?service=host-meta last;
rewrite ^/owncloud/.well-known/host-meta.json /public.php?service=host-meta-json last;
rewrite ^/owncloud/.well-known/carddav /remote.php/carddav/ redirect;
rewrite ^/owncloud/.well-known/caldav /remote.php/caldav/ redirect;
rewrite ^/owncloud/apps/calendar/caldav.php /remote.php/caldav/ last;
rewrite ^/owncloud/apps/contacts/carddav.php /remote.php/carddav/ last;
rewrite ^/owncloud/apps/([^\*/])(/.*\.(css|php))$ /index.php?app=$1&getfile=$2 last;
rewrite ^(/owncloud/core/doc[^\*/]+)$ $1/index.html;
try_files $uri $uri/ index.php;
}
location ~ \.php(?:$|/) {
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
    include fastcgi_params;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_param PATH_INFO $fastcgi_path_info;
    fastcgi_param HTTPS on;
    fastcgi_pass unix:/var/run/php5-fpm.sock;
}
# Optional: set long EXPIRES header on static assets
location ~* \.(?:jpg|jpeg|gif|bmp|ico|png|css|js|swf)$ {
    expires 30d;
    # Optional: Don't log access to assets
    access_log off;
}
}
```

Now you have enable ownCloud and disabled the default configuration if you just installed nginx:

```
sudo ln -s /etc/nginx/sites-available/owncloud /etc/nginx/sites-enabled/owncloud
```

```
sudo unlink /etc/nginx/sites-enabled/default
```

Finally, we need to restart everything for the new configuration to take effect:

```
sudo service nginx restart
```

```
sudo service php5-fpm restart
```