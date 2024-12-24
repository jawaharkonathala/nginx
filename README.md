# nginx
This is a course material for learning nginx (reverse proxy).

The course is divided into the below modules.
- What is NGINX?
- Installation of NGINX
- Terminology
- Serving Static Content
- Mime types
- Location Context
- Rewrites and Redirect
- NGINX as Load Balancer

## What is NGINX?

NGINX is a reverse proxy which is used for encryption of data as well as use to distribute traffic among different servers.

## Installation of NGINX

These steps are for MacOS
1. Go to terminal and type "brew install nginx" and press Enter
2. Now, type "cd /ur/local/etc/nginx" and press Enter
3. You can see the files in the folder using"ls" command
4. Our interest is in the nginx.conf folder
5. For starting nginx, type "nginx" in the terminal and press Enter.

## Terminology

The key value pairs in the nginx.conf file are called "directives"
The blocks present (containing curly braces) are called "contexts".
Each context contains directives specific to that context for configuring nginx.

## Serving static content

1. Create a folder for an application, ex: mysite
2. Inside mysite, create an index.html file and write some HTML code in it.
3. Now, for serving this content, we need to configure nginx.conf
4. So, clear the nginx.conf file and add below code.
```nginx.conf
http {}

events {}
```
5. Now within this file we need to set the port where the content is served, the directory where the html is present and other settings.
6. Add code below to the file.
```nginx.conf
http {
    server {
        listen 8080;
        root /Users/hellopc/mysite;
    }
}

events {}
```
7. Now we can goto terminal and enter command "nginx -s reload" to refresh nginx.
8. Now, we can see the HTML rendered in a browser at http://localhost:8080/

## Mime types

- MIME is a short form for "Multipurpose Internet Mail Extensions"
- It is used to associate particular type of files to render in the website accordingly.
- For example, let's create a styles.css file and associate it with the index.html file.
- Now on refreshing nginx, we still do not see the styles applied to the webpage, ass the css file is associated with "text/plain" instead of "text/css" in DevTools.
- 1. We need to associate the IME type accordingly to sort this issue.
  2. We can take the required types from mime.types file and add them in nginx.conf file within the http context.
  3. ```nginx.conf
     http {
         include mime.types;
         . . .
     }
     ```
  4. Now, upon nginx restart we can see css styles applied.
 
## Location Context

- We want to associate endpoints for serving users from our server. We use location context for this.
- This location context is placed within server context.
- Below is the code for the same.
```nginx.conf
http {
    server {
        . . .
        location /fruits {
            root /Users/hellopc/mysite;
        } # If the file at /fruits endpoint is index.html
        # In address bar, localhost:8080/fruits/index.html will be seen

        location /carbs {
            alias /Users/hellopc/mysite/fruits;
        } # In address bar, localhost:8080/fruits/index.html will be seen

        location /vegetables {
            root /Users/hellopc/mysite;
        }
        # In the vegetables folder, veggies.html present, but not index.html, so above context will give error.

        location /vegetables {
            root /Users/hellopc/mysite;
            try_files /vegetables/veggies.html /index.html
        }
        # Now first veggies.html will be tried to render, if it does not exist, mysite/index.html renders.

        # Using regular expresions
        location ~* /count/[0-9] {
            root /Users/hellopc/mysite;
            try_files /index.html
        }
```

## Rewrites and Redirect

- For redirecting from one endpoint to another
```nginx.conf
location /crops {
    return 307 /fruits
} # In address bar, we see /fruits/index.html
```

- For rewrites the URL does not change upon redirection, we can write below code within server context.
```nginx.conf
rewrite ^/hello/(w+) /count/$1
# Here the variable passed as (w+) will be replaced in place of $1
```

## NGINX as Load Balancer

- For using nginx as a load balancer, we need 3 docker containers.
- Assuming we have 3 docker containers already setup, let's create different ports of nginx to direct to each container.
- Now withing the http context, we can write below code which is used to load balance.
```nginx.conf
http {
    upstream backendserver {
        server 127.0.0.1:<port1>;
        server 127.0.0.1:<port2>;
        server 127.0.0.1:<port3>;
        server 127.0.0.1:<port4>;
    }
    . . .
    server {
        location / {
            proxy_server http:/backendserver/
        }
}
```


