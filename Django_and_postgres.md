## ユーザ作成
データベースを使用する際のDjango用ユーザを作成する。  
```
$ sudo useradd django
$ sudo passwd django
```

## PostgreSQLのインストール
```
$ yum list | grep "postgresql.*-server"
postgresql-server.x86_64                 9.2.23-1.el7_4                 updates
```
[PostgreSQLの公式サイト](https://www.postgresql.org/download/linux/redhat/#yum)を見ると、最新バージョンは10らしいので、今回はそれをインストールする。  

インストールをするには、リポジトリを追加する必要がある。
```
$ sudo yum install -y https://download.postgresql.org/pub/repos/yum/10/redhat/rhel-7-x86_64/pgdg-redhat10-10-1.noarch.rpm

$ yum list | grep "postgresql.*-server"
postgresql-server.x86_64                 9.2.23-1.el7_4                 updates
postgresql10-server.x86_64               10.0-1PGDG.rhel7               pgdg10-updates-testing

$ sudo yum install -y postgresql10-server
$ sudo /usr/pgsql-10/bin/postgresql-10-setup initdb
$ sudo systemctl start postgresql-10
$ sudo systemctl enable postgresql-10
```


## PostgreSQLの設定
```
$ psql -l
psql: FATAL:  role "user" does not exist
```

```
$ id postgres
uid=26(postgres) gid=26(postgres) groups=26(postgres)
$ sudo passwd postgres
$ su postgres
$ cd
$ psql -q


```



```
$ vim mysite/mysite/settings.py

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'django',
				'USER': 'django',
				'PASSWORD': 'pass',
				'HOST': '127.0.0.1',
				'PORT': 5432,
    }
}

```
