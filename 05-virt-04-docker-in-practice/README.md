# Домашнее задание к занятию 5. «Практическое применение Docker»

### Инструкция к выполнению

1. Для выполнения заданий обязательно ознакомьтесь с [инструкцией](https://github.com/netology-code/devops-materials/blob/master/cloudwork.MD) по экономии облачных ресурсов. Это нужно, чтобы не расходовать средства, полученные в результате использования промокода.
3. Своё решение к задачам оформите в вашем GitHub репозитории.
4. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.
5. Сопроводите ответ необходимыми скриншотами.

---
## Примечание: Ознакомьтесь со схемой виртуального стенда [по ссылке](https://github.com/netology-code/shvirtd-example-python/blob/main/schema.pdf)

---

## Задача 1
1. Сделайте в своем github пространстве fork репозитория ```https://github.com/netology-code/shvirtd-example-python/blob/main/README.md```.   
2. Создайте файл с именем ```Dockerfile.python``` для сборки данного проекта. Используйте базовый образ ```python:3.9-slim```. Протестируйте корректность сборки. Не забудьте dockerignore.
3. (Необязательная часть, *) Изучите инструкцию в проекте и запустите web-приложение без использования docker в venv. (Mysql БД можно запустить в docker run).
4. (Необязательная часть, *) По образцу предоставленного python кода внесите в него исправление для управления названием используемой таблицы через ENV переменную.


## Ответ 1 
1. Ссылка на [fork](https://github.com/alexandreevich/shvirtd-example-python/tree/main)
2. Ссылка на [Dockerfile.python](https://github.com/alexandreevich/shvirtd-example-python/blob/main/Dockerfile.python)
3. Сборка успешно осуществлена командой: docker build -t myimage -f ./Dockerfile.python .
4. ![Снимок экрана 2024-02-23 в 12 52 46](https://github.com/alexandreevich/virtd-homeworks/assets/109306886/81cbb635-1dcd-4f5d-bf71-dc4c6127b2e5)


## Задача 2 (*)
1. Создайте в yandex cloud container registry с именем "test" с помощью "yc tool" . [Инструкция](https://cloud.yandex.ru/ru/docs/container-registry/quickstart/?from=int-console-help)
2. Настройте аутентификацию вашего локального docker в yandex container registry.
3. Соберите и залейте в него образ с python приложением из задания №1.
4. Просканируйте образ на уязвимости.
5. В качестве ответа приложите отчет сканирования.

## Задача 3
1. Создайте файл ```compose.yaml```. Опишите в нем следующие сервисы: 

- ```web```. Образ приложения должен ИЛИ собираться при запуске compose из файла ```Dockerfile.python``` ИЛИ скачиваться из yandex cloud container registry(из задание №2 со *). Контейнер должен работать в bridge-сети с названием ```backend``` и иметь фиксированный ipv4-адрес ```172.20.0.5```. Сервис должен всегда перезапускаться в случае ошибок.
Передайте необходимые ENV-переменные для подключения к Mysql базе данных по сетевому имени сервиса ```web``` 

- ```db```. image=mysql:8. Контейнер должен работать в bridge-сети с названием ```backend``` и иметь фиксированный ipv4-адрес ```172.20.0.10```. Явно перезапуск сервиса в случае ошибок. Передайте необходимые ENV-переменные для создания: пароля root пользователя, создания базы данных, пользователя и пароля для web-приложения.Обязательно используйте .env file для назначения секретных ENV-переменных!

2. Запустите проект локально с помощью docker compose , добейтесь его стабильной работы.Протестируйте приложение с помощью команд ```curl -L http://127.0.0.1:8080``` и ```curl -L http://127.0.0.1:8090```.

5. Подключитесь к БД mysql с помощью команды ```docker exec <имя_контейнера> mysql -uroot -p<пароль root-пользователя>``` . Введите последовательно команды (не забываем в конце символ ; ): ```show databases; use <имя вашей базы данных(по-умолчанию example)>; show tables; SELECT * from requests LIMIT 10;```.

6. Остановите проект. В качестве ответа приложите скриншот sql-запроса.

## Ответ 3 
1. Ссылка на [compose.yaml](https://github.com/alexandreevich/shvirtd-example-python/blob/main/compose.yaml)
2. У меня не поднялся проект, возникла ошибка: User specified IP address is supported only when connecting to networks with user configured subnets
Как я понял, он не мог создать сеть. Поэтому я прописал командой сеть: docker network create --subnet 172.28.0.0/24 app_subnet
В compose.yaml & proxy.yaml прописал:
```
 external: true
```
И после этого заработало.

Curl работает, однако не возвращает IP.
![Снимок экрана 2024-02-24 в 14 48 00](https://github.com/alexandreevich/virtd-homeworks/assets/109306886/e637cf5f-c8f0-4318-be03-dc95c8df0842)
В БД подключался, там видны подключения, но IP так же не пишется: 
![Снимок экрана 2024-02-24 в 15 06 59](https://github.com/alexandreevich/virtd-homeworks/assets/109306886/4b78013f-13fa-4f20-926a-b43539e860b2)




## Задача 4
1. Запустите в Yandex Cloud ВМ (вам хватит 2 Гб Ram).
2. Подключитесь к Вм по ssh и установите docker.
3. Напишите bash-скрипт, который скачает ваш fork-репозиторий в каталог /opt и запустит проект целиком.
4. Зайдите на сайт проверки http подключений, например(или аналогичный): ```https://check-host.net/check-http``` и запустите проверку вашего сервиса ```http://<внешний_IP-адрес_вашей_ВМ>:8090```. Таким образом трафик будет направлен в ingress-proxy.
5. (Необязательная часть) Дополнительно настройте remote ssh context к вашему серверу. Отобразите список контекстов и результат удаленного выполнения ```docker ps -a```
6. В качестве ответа повторите  sql-запрос и приложите скриншот с данного сервера, bash-скрипт и ссылку на fork-репозиторий.



## Ответ 4
1. Использовал изначально terraform для разворачивания вм:
![Снимок экрана 2024-02-24 в 16 29 41](https://github.com/alexandreevich/virtd-homeworks/assets/109306886/d679af18-cea4-4bab-8a64-9f4944561e9f)
2. Cкрипт:
```
# Клонируем репозиторий, если он еще не клонирован
if [ ! -d "/opt/shvirtd-example-python" ] ; then
    echo "Клонирование репозитория..."
    sudo git clone https://github.com/alexandreevich/shvirtd-example-python.git /opt/shvirtd-example-python
else
    echo "Репозиторий уже клонирован."
    cd /opt/shvirtd-example-python
    sudo git pull
fi

# Переходим в директорию проекта
cd /opt/shvirtd-example-python

# Запускаем проект
echo "Запуск проекта..."
sudo docker-compose -f compose.yaml -f proxy.yaml up -d
```
3. Сайт отработал штатно, однако IP не отобразились в БД:
![Снимок экрана 2024-02-24 в 15 51 53](https://github.com/alexandreevich/virtd-homeworks/assets/109306886/93cc2a60-bf89-4637-897a-64633df7c294)


## Задача 5 (*)
1. Напишите и задеплойте на вашу облачную ВМ bash скрипт, который произведет резервное копирование БД mysql в директорию "/opt/backup" с помощью запуска в сети "backend" контейнера из образа ```schnitzler/mysqldump``` при помощи ```docker run ...``` команды. Подсказка: "документация образа."
2. Протестируйте ручной запуск
3. Настройте выполнение скрипта раз в 1 минуту через cron, crontab или systemctl timer. Придумайте способ не светить логин/пароль в git!!
4. Предоставьте скрипт, cron-task и скриншот с несколькими резервными копиями в "/opt/backup"

## Задача 6
Скачайте docker образ ```hashicorp/terraform:latest``` и скопируйте бинарный файл ```/bin/terraform``` на свою локальную машину, используя dive и docker save.
Предоставьте скриншоты  действий .
![Снимок экрана 2024-02-24 в 16 56 42](https://github.com/alexandreevich/virtd-homeworks/assets/109306886/7aa7afbb-ea5f-494d-a16b-f6c22db7dda0)

docker save hashicorp/terraform:latest > terraform-image.tar

## Задача 6.1
Добейтесь аналогичного результата, используя docker cp.  
Предоставьте скриншоты  действий .
![Снимок экрана 2024-02-24 в 17 00 42](https://github.com/alexandreevich/virtd-homeworks/assets/109306886/d70956ea-a9f1-4b78-8e85-1ee59a320009)


## Задача 6.2 (**)
Предложите способ извлечь файл из контейнера, используя только команду docker build и любой Dockerfile.  
Предоставьте скриншоты  действий .

## Задача 7 (***)
Запустите ваше python-приложение с помощью runC, не используя docker или containerd.  
Предоставьте скриншоты  действий .
