Чтобы развернуть приложение Docker, состоящее из компонентов NGINX и PHP, с дополнительным использованием режима swarm,с учетом запрошеной Вами конфигурацией ограничений и настройкой ведения журнала, я выполнил следующие шаги: 

1. Настроил структуру проекта 

project/

├── nginx/

│   └── nginx.conf

├── php/

│   └── Dockerfile

└── docker-compose.yml

2. Настроил NGINX, точнее создал в nginx каталоге файл nginx.conf. 

3. Создал Dockerfile для PHP в php каталоге для создания контерйнера PHP. Я использовал официальный образ PHP в качестве основы и добавил парочку зависимостей.

4. Настроил файл docker-compose.yml для определения и настройки служб.

5. Развернул приложение. Так как мы хотим использовать режим swarm в Docker'e, выполним следующую команду перед деплоем - docker swarm init

Итого получились такие команды:
mkdir project - Создал папку "Проект"
cd project
mkdir nginx php
cd nginx
nano nginx.config - сюда прописал настройка nginx'a
cd ../php
nano Dockerfile - сюда докерфайл для PHP
cd ..
nano docker-compose.yml - сюда компановку
docker swarm init - инициировал режим swarm
docker stack deploy -c docker-compose.yml <имя стека> - так как нам необходимо работать в режиме swarm команда "docker-compose up -d" нам не подходит, вместо нее используем stack deploy
 6. Также сразу укажу, что необходимо чтобы организовать связь между контейнерами в режиме swarm:

docker network create --driver overlay <имя-сети> - создать сеть
nano docker-compose.yml - дописать сеть в файл настройки 
docker stack deploy -c docker-compose.yml <имя стека> - перезапустить обновленный стек

С помощью этих добавленых шагов контейнеры будут развернуты в оверлейной сети, что позволит им взаимодействовать друг с другом, используя свои служебные имена в качестве DNS-разрешимых имен хостов. Например, nginx служба может связываться с php службой с использованием имени хоста php. 
По умолчанию Docker Swarm включает разрешение DNS в оверлейной сети, позволяя контейнерам обнаруживать друг друга и взаимодействовать друг с другом, используя имена служб, определенные в docker-compose.yml файле. 

