# My Nginx Configuration

```
server{
    listen 80;
    server_name joomla.local.io;
    root    /Users/workspace/joomla/; 
    index index.php index.html;

# important part, 重要！配置这就可以隐藏index.php
    location / {
        try_files $uri $uri/ /index.php?$args;
    }

# deny running scripts inside writable directories
    location ~* /(images|cache|media|logs|tmp)/.*\.(php|pl|py|jsp|asp|sh|cgi)$ {
        return 403;
        error_page 403 /403_error.html;
    }

    location ~ \.php$ {
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param SCRIPT_NAME $fastcgi_script_name;
    }

# caching of files 
    location ~* \.(ico|pdf|flv)$ {
            expires 1y;
    }

    location ~* \.(js|css|png|jpg|jpeg|gif|swf|xml|txt)$ {
            expires 14d;
    }
}
```
