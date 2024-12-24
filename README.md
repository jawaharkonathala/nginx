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

