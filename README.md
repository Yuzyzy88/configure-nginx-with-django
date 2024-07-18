# Configure Nginx with Django

## 1. Install Nginx
First udpate our local package and install nginx:
```
sudo apt update
sudo apt install nginx
```

## 2. Adjusting the Firewall
List the application config that uufw knows how to work by run:
```
sudo ufw app list
```

If your firewall enabled. You may need to allow traffic on port 80:
```
sudo ufw allow 'Nginx HTTP'
```

You can verify by run:
```
sudo ufw status
```

Output:
> OpenSSH                    ALLOW       Anywhere                  
Nginx HTTP                 ALLOW       Anywhere                  
OpenSSH (v6)               ALLOW       Anywhere (v6)             
Nginx HTTP (v6)            ALLOW       Anywhere (v6)

## 3 Install Gunicorn:
Gunicorn: A WSGI server to serve your Django application
Go to your environment django and run:
```
pipenv install django gunicorn
or
pip install django gunicorn
```

Run your Django application using Gunicorn. Navigate to your Django project directory 
```
cd /path/to/your/django/project
pipenv shell
gunicorn --bind 127.0.0.1:8000 your_project.wsgi:application

```
and verivy gunicorn can serve your django application by accessing `http://127.0.0.1:8000` in your browser


## 4. Configure Nginx
Create a new configuration file for Node.js application in Nginx:
```
sudo nano /etc/nginx/sites-available/myapp
```

Example nginx configuration in myapp:
```
server {
  listen 80;
  server_name example.com;

  location / {
    proxy_pass http://localhost:8000;
  }
}
```
> listen 80: Nginx listen on port 80 (default HTTP port)

> server_name example.com: Replace `example.com` with your domain name or server IP

> location / : Defines how nginx handle request

> proxy_pass http://localhost:8000: Forwards requests to your Node.js application running on port 3000.

Save with press `etc` run `:wq`

## 5. Linked File:
```
sudo ln -srvf /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled/myapp
```

> ln: to create links between file

> -srvf: you can check by run `man ln`

## 6. Test Nginx Configuration for Syntax Errors:
```
sudo nginx -t
```

if no errors, reload nginx to applay configuration:
```
sudo systemctl reload nginx
```

## Another Command
Checking web server
```
sudo systemctl status nginx
```

To stop web server:
```
sudo systemctl stop nginx
```

To start web server:
```
sudo systemctl start nginx
```

To stop and then start service:
```
sudo systemctl restart nginx
```

Nginx is configured to start automatically when the server boots. If this is what you want, you can disable:
```
sudo systemctl disable nginx
```

To re-enable the service to start up at boot:
```
sudo systemctl enable nginx
```

## To Check Application is ussing Nginx 
You can inspect the HTTP headers like `server` may indicate nginx as the server handling the request
```
curl -I http://your-application-url
```
