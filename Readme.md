# docker-compose

Чем удобен docker-compose?

docker-compose позволяет управлять всеми контейнерами из одного файла (файл - формата yml). Теперь не нужно будет писать кучу docker run и длинные команды для запуска контейнеров: достаточно будет лишь запустить одну команду docker-compose up, после чего все контейнеры будут подняты без нашего вмешательства, причем со всеми  настройками, которые будут прописаны в yml-файле. Также docker-compose позволяет взаимодействовать с системой управления репозиториями программного кода. Например: gitlab.