# Django_docker-compose startup template
![docker-compose](https://img.shields.io/badge/docker_compose-v3-blue?style=plastic&logo=docker)
![nginx](https://img.shields.io/badge/nginx-1.17-darkgreen?style=plastic&logo=nginx)
![django](https://img.shields.io/badge/django-3.1.12-green?style=plastic&logo=django)
![python](https://img.shields.io/badge/python-3.9-red?style=plastic&logo=python)

1. .env.sample -> .env
2. .env にそれぞれDBパスワードを設定。
3. `docker-compose run --rm app django-admin.py startproject app .` を実行。
4. `./docker/src/app/settings.py` に以下を記述。
```py
# 14 - 16 行目
import pymysql
import os
pymysql.install_as_MySQLdb()

# 31 行目
ALLOWED_HOSTS = ['localhost']

# 79 - 88 行目
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'app_db',
        'USER': 'appuser',
        'PASSWORD': os.environ.get("MYSQL_PASSWORD"),
        'HOST': 'db',
        'PORT': '3306',
    }
}

# 以下はお好みにより変更
# 113 行目
LANGUAGE_CODE = 'ja'
# 115 行目
TIME_ZONE = 'Asia/Tokyo'
```

- モデルに対して行った変更を元に、新しいマイグレーションを作成。
4. `docker-compose run --rm app ./manage.py makemigrations` を実行
- 変更内容をDBに反映。
5. `docker-compose run --rm app ./manage.py migrate` を実行。
- Djangoの管理者を設定。
6. `docker-compose run --rm app ./manage.py createsuperuser` を実行
```
Username (leave blank to use 'root'): （任意のユーザー名を入力。）
Email address: （メールアドレスを入力。）
Password: （パスワードを入力。）
Password (again): （上で設定したパスワードを再入力。）
```
- 起動。
7. `docker-compose up -d`
- WEBページで確認。
8. http://localhost
- 管理者ページを確認。
9. http://localhost/admin

- その他
  - WEBアプリ作成。
  - `docker-compose run --rm app python manage.py startapp example`
