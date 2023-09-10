# Локальный docker-репозиторий

Простейший Docker-репозиторий можно поднять одной командой
```bash
docker run -d -p 5000:5000 --restart=always --name registry registry:2
```
Для проверки репозитория зальем в него образ ubuntu.

Сначала скачаем образ с официального репозитория и добавим ему тег `localhost:5000/ubuntu`
Запушим образ в наш новый репозиторий: `docker push localhost:5000/ubuntu`

Хорошо, теперь мы умеем работать с локальным docker-репозиторием. Чтобы каждый раз не вспоминать команду запуска репозитория давайте создадим docker-compose.yml файл:

```
registry:
  restart: always
  image: registry:2
  ports:
    - 5000:5000  
  volumes:
    - /path/data:/var/lib/registry /* раздел который будет использоваться как хранилище для вашего репозитория*/
```

Чтобы запустить репозиторий достаточно ввести команду `docker-compose up -d` в директории с docker-compose файлом. 

Для тестирования нашего репозитория выполним следующие команды:

```
docker pull ubuntu
docker tag ubuntu myregistrydomain.com:443/ubuntu
docker push myregistrydomain.com:443/ubuntu
docker pull myregistrydomain.com:443/ubuntu
```

## Настраиваем аутентификацию

Защитим наш репозиторий паролем. Для этого необходимо создать файл с паролями и указать его docker-репозиторию. Следующая команда создаст пользователя testuser с паролем testpassword, поместит их в файл и сохранит его в path/auth/htpasswd.

`docker run --entrypoint htpasswd registry:2 -Bbn testuser testpassword > path/auth/htpasswd`

В docker-compose укажем путь к файлу с паролями и использование `basic аутентификации`

```
registry:
  restart: always
  image: registry:2
  ports:
    - 443:5000
  environment:
    REGISTRY_HTTP_TLS_LETSENCRYPT_CACHEFILE: /cache.letsencrypt
    REGISTRY_HTTP_TLS_LETSENCRYPT_EMAIL: hello@rdseventeen.com
    REGISTRY_AUTH: htpasswd
    REGISTRY_AUTH_HTPASSWD_PATH: /auth/htpasswd
    REGISTRY_AUTH_HTPASSWD_REALM: Registry Realm
  volumes:
    - /path/data:/var/lib/registry
    - /path/auth:/auth
```

Для того чтобы изменения вступили в силу необходимо перезапустить docker-репозиторий. Используем команду `docker-compose restart`. 
Логинимся в созданный репозиторий с помощью команды `docker login myregistrydomain.com:443`
После этого наш репозиторий будет доступен для скачивания хранящихся в нём образов.
