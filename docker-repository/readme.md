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
