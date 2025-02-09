
# Домашнее задание к занятию 4 «Оркестрация группой Docker контейнеров на примере Docker Compose»

### Инструкция к выполению

1. Для выполнения заданий обязательно ознакомьтесь с [инструкцией](https://github.com/netology-code/devops-materials/blob/master/cloudwork.MD) по экономии облачных ресурсов. Это нужно, чтобы не расходовать средства, полученные в результате использования промокода.
2. Практические задачи выполняйте на личной рабочей станции или созданной вами ранее ВМ в облаке.
3. Своё решение к задачам оформите в вашем GitHub репозитории.
4. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.

## Задача 1

Сценарий выполнения задачи:
- Установите docker и docker compose plugin на свою linux рабочую станцию или ВМ.
- Зарегистрируйтесь и создайте публичный репозиторий  с именем "custom-nginx" на https://hub.docker.com;
- скачайте образ nginx:1.21.1;
- Создайте Dockerfile и реализуйте в нем замену дефолтной индекс-страницы(/usr/share/nginx/html/index.html), на файл index.html с содержимым:
```
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
```
- Соберите и отправьте созданный образ в свой dockerhub-репозитории c tag 1.0.0 . 
- Предоставьте ответ в виде ссылки на https://hub.docker.com/<username_repo>/custom-nginx/general .

## Ответ 1
https://hub.docker.com/r/alexandreevich/custom-nginx/tags


## Задача 2
1. Запустите ваш образ custom-nginx:1.0.0 командой docker run в соответвии с требованиями:
- имя контейнера "ФИО-custom-nginx-t2"
- контейнер работает в фоне
- контейнер опубликован на порту хост системы 127.0.0.1:8080
2. Переименуйте контейнер в "custom-nginx-t2"
3. Выполните команду ```date +"%d-%m-%Y %T.%N %Z" && sleep 0.150 && docker ps && ss -tlpn | grep 127.0.0.1:8080  && docker logs custom-nginx-t2 -n1 && docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html```
4. Убедитесь с помощью curl или веб браузера, что индекс-страница доступна.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.


## Ответ 2

 ![Снимок экрана 2024-02-01 в 19 05 56](https://github.com/alexandreevich/virtd-homeworks/assets/109306886/0b0b2ef5-42f8-4b0b-a2dc-9846480ee467)

 ![Снимок экрана 2024-02-01 в 19 19 10](https://github.com/alexandreevich/virtd-homeworks/assets/109306886/c7c87b5b-1676-4861-95a1-e4ee799c835c)




## Задача 3
1. Воспользуйтесь docker help или google, чтобы узнать как подключиться к стандартному потоку ввода/вывода/ошибок контейнера "custom-nginx-t2".
2. Подключитесь к контейнеру и нажмите комбинацию Ctrl-C.
3. Выполните ```docker ps -a``` и объясните своими словами почему контейнер остановился.
4. Перезапустите контейнер
5. Зайдите в интерактивный терминал контейнера "custom-nginx-t2" с оболочкой bash.
6. Установите любимый текстовый редактор(vim, nano итд) с помощью apt-get.
7. Отредактируйте файл "/etc/nginx/conf.d/default.conf", заменив порт "listen 80" на "listen 81".
8. Запомните(!) и выполните команду ```nginx -s reload```, а затем внутри контейнера ```curl http://127.0.0.1:80 ; curl http://127.0.0.1:81```.
9. Выйдите из контейнера, набрав в консоли  ```exit``` или Ctrl-D.
10. Проверьте вывод команд: ```ss -tlpn | grep 127.0.0.1:8080``` , ```docker port custom-nginx-t2```, ```curl http://127.0.0.1:8080```. Кратко объясните суть возникшей проблемы.
11. * Это дополнительное, необязательное задание. Попробуйте самостоятельно исправить конфигурацию контейнера, используя доступные источники в интернете. Не изменяйте конфигурацию nginx и не удаляйте контейнер. Останавливать контейнер можно. [пример источника](https://www.baeldung.com/linux/assign-port-docker-container)
12. Удалите запущенный контейнер "custom-nginx-t2", не останавливая его.(воспользуйтесь --help или google)

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.


## Ответ 3 
Контейнер остановился, так как мы нажали комбинацию Ctrl-C находясь в нем.
Скриншот с перезапуском nginx и курлом портов.

![Снимок экрана 2024-02-01 в 19 40 57](https://github.com/alexandreevich/virtd-homeworks/assets/109306886/5256915c-5ffa-446a-bc3d-be895852c3d0)

Скриншот с курлом с хостовой машины до контейнеров. Не находит nginx, потому что мы поменяли внутренний порт, но проброс портов в docker не изменили. Там же команда удаления контейнера без остановки. 

 ![Снимок экрана 2024-02-01 в 19 43 09](https://github.com/alexandreevich/virtd-homeworks/assets/109306886/9ac56357-98cd-4cee-89f8-1d60faabee45)


## Задача 4


- Запустите первый контейнер из образа ***centos*** c любым тегом в фоновом режиме, подключив папку  текущий рабочий каталог ```$(pwd)``` на хостовой машине в ```/data``` контейнера, используя ключ -v.
- Запустите второй контейнер из образа ***debian*** в фоновом режиме, подключив текущий рабочий каталог ```$(pwd)``` в ```/data``` контейнера. 
- Подключитесь к первому контейнеру с помощью ```docker exec``` и создайте текстовый файл любого содержания в ```/data```.
- Добавьте ещё один файл в текущий каталог ```$(pwd)``` на хостовой машине.
- Подключитесь во второй контейнер и отобразите листинг и содержание файлов в ```/data``` контейнера.


В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

## Ответ 4
Контейнеры запущены. Подключаемя к первому. 
<img width="901" alt="Снимок экрана 2024-02-02 в 23 15 47" src="https://github.com/alexandreevich/virtd-homeworks/assets/109306886/97de6e4a-524a-4c42-af91-f3d6f6da5880">

Создали два файла. Они оба видны с другого контейнера, так как у нас идет проброс volumes. 
<img width="311" alt="Снимок экрана 2024-02-02 в 23 17 03" src="https://github.com/alexandreevich/virtd-homeworks/assets/109306886/abaf4575-6df6-44bf-a9b2-5637856c1623">


## Задача 5

1. Создайте отдельную директорию(например /tmp/netology/docker/task5) и 2 файла внутри него.
"compose.yaml" с содержимым:
```
version: "3"
services:
  portainer:
    image: portainer/portainer-ce:latest
    network_mode: host
    ports:
      - "9000:9000"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```
"docker-compose.yaml" с содержимым:
```
version: "3"
services:
  registry:
    image: registry:2
    network_mode: host
    ports:
    - "5000:5000"
```

И выполните команду "docker compose up -d". Какой из файлов был запущен и почему? (подсказка: https://docs.docker.com/compose/compose-application-model/#the-compose-file )

2. Отредактируйте файл compose.yaml так, чтобы были запущенны оба файла. (подсказка: https://docs.docker.com/compose/compose-file/14-include/)

3. Выполните в консоли вашей хостовой ОС необходимые команды чтобы залить образ custom-nginx как custom-nginx:latest в запущенное вами, локальное registry. Дополнительная документация: https://distribution.github.io/distribution/about/deploying/
4. Откройте страницу "https://127.0.0.1:9000" и произведите начальную настройку portainer.(логин и пароль адмнистратора)
5. Откройте страницу "http://127.0.0.1:9000/#!/home", выберите ваше local  окружение. Перейдите на вкладку "stacks" и в "web editor" задеплойте следующий компоуз:

```
version: '3'

services:
  nginx:
    image: 127.0.0.1:5000/custom-nginx
    ports:
      - "9090:80"
```
6. Перейдите на страницу "http://127.0.0.1:9000/#!/2/docker/containers", выберите контейнер с nginx и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".

7. Удалите любой из манифестов компоуза(например compose.yaml).  Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите compose-проект ОДНОЙ(обязательно!!) командой.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод, файл compose.yaml , скриншот portainer c задеплоенным компоузом.


## Ответ 5 
Загрузился только первый файл,  compose.yml, так как он считается файлом по умолчанию.

<img width="1141" alt="Снимок экрана 2024-02-04 в 11 35 23" src="https://github.com/alexandreevich/virtd-homeworks/assets/109306886/e60e844e-3239-4815-b607-ae7c627004d7">

После включили еще один добавив в первый:
```
include:
  - docker-compose.yaml 
```
<img width="1127" alt="Снимок экрана 2024-02-04 в 11 38 52" src="https://github.com/alexandreevich/virtd-homeworks/assets/109306886/7e26ac64-6abe-4937-a12a-5d99b955ac14">

Отработал и второй. 

Далее скрин с portainer. 
![Снимок экрана 2024-02-04 в 12 45 47](https://github.com/alexandreevich/virtd-homeworks/assets/109306886/af4b6bc8-9d8a-461c-ae5e-ece73b42ec4e)

И удаление одного из compose.yaml, предупреждение что второй останется сиротой. И при выполнении флага --remove-orphans он очистит. Проделываем, получаем результат и далее в одну команду роняем все compose.yaml 

![Снимок экрана 2024-02-04 в 12 59 12](https://github.com/alexandreevich/virtd-homeworks/assets/109306886/b43956d5-f368-45a0-9c2f-1dc7e1a403ed)

---

### Правила приема

Домашнее задание выполните в файле readme.md в GitHub-репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.


