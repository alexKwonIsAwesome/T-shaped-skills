# Deploying Django Application on AWS

## Create AWS EC2
On the AWS console, create your own EC2 instance.

## SSH access to the server computer

## Install pyenv, virtualenv, autoenv on the EC2 instance

1. Install git on the computer
```sudo apt-get install git```
2. Install pyenv by following the steps on the link
  - [https://github.com/yyuu/pyenv-installer](https://github.com/yyuu/pyenv-installer)
  - [https://github.com/yyuu/pyenv](https://github.com/yyuu/pyenv)
  - You can install specific python versions with the command: ```pyenv install 3.5.1```
3. Install pyenv-virtualenv on the links below
  - [https://github.com/yyuu/pyenv-virtualenv](https://github.com/yyuu/pyenv-virtualenv)
  - Now we can use virtualenv with the command:
  ```pyenv virtualenv 3.5.1 my-virtual-env```
4. Install Autoenv to enable auto activation of scripts
  - [https://github.com/kennethreitz/autoenv](https://github.com/kennethreitz/autoenv)

## Installing Database(PostgreSQL) and connect it with Django

### Setting database on local machine
reference doc:[https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-14-04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-14-04)
- Follow the guide on the reference document and install the PostgreSQL
$ sudo apt-get install postgresql postgresql-contrib

최고 관리자 계정인 postgres으로 데이터베이스 접속

$ sudo -i -u postgres

새로운 계정의 이름과 비밀번호 생성

CREATE USER [USERNAME] WITH PASSWORD '[PASSWORD]';

새로운 데이터베이스 생성

CREATE DATABASE [DATABASE NAME];

데이터베이스의 권한을 유저에게 부여

GRANT ALL PRIVILEGES ON DATABASE [DATABASE NAME] to [USERNAME];

$ psql -d [DATABASE NAME]

데이터베이스에서 나온 후,

/q

root 계정으로 로그인

$ sudo -s

아까 만든 계정 추가

$ adduser [USERNAME]

비밀번호를 통해서 데이터베이스에 들어올 수 있게 하기 위해 설정 변경

$ sudo vi /etc/postgresql/9.3/main/pg_hba.conf

90번째 줄에서 peer를 md5로 변경 후 데이터베이스 재시작

$ sudo service postgresql restart

아까 만든 계정으로 로그인 후 데이터베이스 접속되는지 확인

$ psql -U [USERNAME] -d [DATABASE NAME] -W

### Connecting database with Django with dj-databse-url
reference doc: [https://github.com/kennethreitz/dj-database-url](https://github.com/kennethreitz/dj-database-url)

## Procfile-based application
references: [https://devcenter.heroku.com/articles/procfile](https://devcenter.heroku.com/articles/procfile), [https://honcho.readthedocs.io/en/latest/](https://honcho.readthedocs.io/en/latest/), [http://gunicorn.org/](http://gunicorn.org/).
1. Install Gunicorn.
2. Create 'Procfile' in the base root folder and add the following script:`web: gunicorn --pythonpath snailshell/ --bind :5494 --workers=3 snailshell.wsgi`

## Install Nginx on EC2 server(ubuntu)
reference docs: [https://developers.google.com/speed/pagespeed/module/build_ngx_pagespeed_from_source](https://developers.google.com/speed/pagespeed/module/build_ngx_pagespeed_from_source)

1. Follow the installation process guided in the reference document:
  ``````
  $ cd
  $ NGINX_VERSION=1.8.1
  $ wget http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz
  $ tar -xvzf nginx-${NGINX_VERSION}.tar.gz
  $ cd nginx-${NGINX_VERSION}/
  $ ./configure --with-http_ssl_module
  $ make 
  $ sudo make install
  ``````
2. Next, install following library: [https://github.com/JasonGiedymin/nginx-init-ubuntu](https://github.com/JasonGiedymin/nginx-init-ubuntu). This enables to manage nginx server more easily.

## Setting Nginx configuration

Access the file: ```$ sudo vi /usr/local/nginx/conf/nginx.conf```. Then replace the content as:
```
worker_processes  1;

events {
    worker_connections  1024;
    accept_mutex off;
}


http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile on;

    server {
        listen       80;
        server_name  [DOMAIN ADDRESS];

        client_max_body_size 4G;
        keepalive_timeout 5;

        return 301 https://$server_name$request_uri;
    }

    server {
        listen  443 default_server ssl;
        server_name  [DOMAIN ADDRESS];

        client_max_body_size 4G;
        keepalive_timeout 5;

        ssl_certificate                 /etc/letsencrypt/live/[DOMAIN ADDRESS]/fullchain.pem;
        ssl_certificate_key             /etc/letsencrypt/live/[DOMAIN ADDRESS]/privkey.pem;

        location / {
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto https;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header HOST $http_host;
            proxy_set_header X-NginX-Proxy true;

            proxy_pass http://127.0.0.1:5736;
            proxy_redirect off;
        }
    }
}
```
Before domain is ready, you can replace the [DOMAIN ADDRESS] into the EC2 instance's ip address. When the request is made to the server, it knows the request method and if it is http request it activates the one with 80 server dictionary settings. On the other hand, when it is https request, it runs 443 settings. In this case, we assume that the domain has ssl_certificate. If that is not the case, we have to change the setting accordingly.

## Configuring SSL certification on domain
reference: [https://letsencrypt.org/](https://letsencrypt.org/), [https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-14-04](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-14-04).

Perform commands under below:
```
git clone https://github.com/letsencrypt/letsencrypt
cd letsencrypt
./letsencrypt-auto --help
```

### Domain namse server configuration

After creating hosted zones in route53 in aws, you should configure the dns setting on the domain. The NS type record set is the one should be input to the domain's dns server list.

Next, create an A record set to connect the domain with the EC2's IP address.

Then, configure the Nginx settings:
```
worker_processes  1;

events {
    worker_connections  1024;
    accept_mutex off;
}


http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile on;

    server {
        listen       80;
        server_name  xx.xx.xxx.xxx http://blabla.co;

        client_max_body_size 4G;
        keepalive_timeout 5;

        # return 301 https://$server_name$request_uri;

        location / {
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto https;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header HOST $http_host;
            proxy_set_header X-NginX-Proxy true;

            proxy_pass http://127.0.0.1:5494;
            proxy_redirect off;
        }
    }

    server {
        listen  443 default_server ssl;
        server_name  xx.xx.xxx.xxx http://blabla.co;

        client_max_body_size 4G;
        keepalive_timeout 5;

        # ssl_certificate                 /etc/letsencrypt/live/[DOMAIN ADDRESS]/fullchain.pem;
        # ssl_certificate_key             /etc/letsencrypt/live/[DOMAIN ADDRESS]/privkey.pem;

        location / {
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto https;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header HOST $http_host;
            proxy_set_header X-NginX-Proxy true;

            proxy_pass http://127.0.0.1:5494;
            proxy_redirect off;
        }
    }
}
```
