### Сборка докер образов для пустого PHP приложения
<hr>

При запуске `docker-compose up -d --build`, будут запущенны следующие сервисы:
 - php8.0 - port: 8000
 - postgres:13.3 - port: 5432
 - adminer - port: 8181
 - redis - port: 6379

Структура докер файлов:
```
app
└── docker
    ├── adminer
    │    └── Dockerfile
    ├── nginx
    │    ├── default.conf
    │    └── nginx.conf
    ├── php
    │    ├── php.ini
    │    └── php-xdebug.ini
    └── Dockerfile
