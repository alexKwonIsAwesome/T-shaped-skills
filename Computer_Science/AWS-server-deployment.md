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
- `web: gunicorn --pythonpath snailshell/ --bind :5494 --workers=3 snailshell.wsgi`

