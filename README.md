# shvirtd-example-python
# Задача 1

Сделайте в своем github пространстве fork репозитория. Примечание: В связи с доработкой кода python приложения. Если вы уверены что задание выполнено вами верно, а код python приложения работает с ошибкой то используйте вместо main.py файл old_main.py(просто измените CMD)
Создайте файл с именем Dockerfile.python для сборки данного проекта(для 3 задания изучите https://docs.docker.com/compose/compose-file/build/ ). Используйте базовый образ python:3.9-slim. Обязательно используйте конструкцию COPY . . в Dockerfile. Не забудьте исключить ненужные в имадже файлы с помощью dockerignore. Протестируйте корректность сборки.
(Необязательная часть, *) Изучите инструкцию в проекте и запустите web-приложение без использования docker в venv. (Mysql БД можно запустить в docker run).
(Необязательная часть, *) По образцу предоставленного python кода внесите в него исправление для управления названием используемой таблицы через ENV переменную.

# Ответ 1

```
sudo docker build -t python -f Dockerfile.python . 
```

# Задача 3

Изучите файл "proxy.yaml"
Создайте в репозитории с проектом файл compose.yaml. С помощью директивы "include" подключите к нему файл "proxy.yaml".
Опишите в файле compose.yaml следующие сервисы:
web. Образ приложения должен ИЛИ собираться при запуске compose из файла Dockerfile.python ИЛИ скачиваться из yandex cloud container registry(из задание №2 со *). Контейнер должен работать в bridge-сети с названием backend и иметь фиксированный ipv4-адрес 172.20.0.5. Сервис должен всегда перезапускаться в случае ошибок. Передайте необходимые ENV-переменные для подключения к Mysql базе данных по сетевому имени сервиса web

db. image=mysql:8. Контейнер должен работать в bridge-сети с названием backend и иметь фиксированный ipv4-адрес 172.20.0.10. Явно перезапуск сервиса в случае ошибок. Передайте необходимые ENV-переменные для создания: пароля root пользователя, создания базы данных, пользователя и пароля для web-приложения.Обязательно используйте уже существующий .env file для назначения секретных ENV-переменных!

Запустите проект локально с помощью docker compose , добейтесь его стабильной работы: команда curl -L http://127.0.0.1:8090 должна возвращать в качестве ответа время и локальный IP-адрес. Если сервисы не стартуют воспользуйтесь командами: docker ps -a  и docker logs <container_name> . Если вместо IP-адреса вы получаете NULL --убедитесь, что вы шлете запрос на порт 8090, а не 5000.

Подключитесь к БД mysql с помощью команды docker exec -ti <имя_контейнера> mysql -uroot -p<пароль root-пользователя>(обратите внимание что между ключем -u и логином root нет пробела. это важно!!! тоже самое с паролем) . Введите последовательно команды (не забываем в конце символ ; ): show databases; use <имя вашей базы данных(по-умолчанию example)>; show tables; SELECT * from requests LIMIT 10;.

Остановите проект. В качестве ответа приложите скриншот sql-запроса.

# Ответ 3
127.0.0.1:8090/

![alt text](https://github.com/StepanovSA/shvirtd-example-python/blob/main/victory.PNG)

SQL

![alt text](https://github.com/StepanovSA/shvirtd-example-python/blob/main/victory2.PNG)


# Задача 4

Запустите в Yandex Cloud ВМ (вам хватит 2 Гб Ram).
Подключитесь к Вм по ssh и установите docker.
Напишите bash-скрипт, который скачает ваш fork-репозиторий в каталог /opt и запустит проект целиком.
Зайдите на сайт проверки http подключений, например(или аналогичный): https://check-host.net/check-http и запустите проверку вашего сервиса http://<внешний_IP-адрес_вашей_ВМ>:8090. Таким образом трафик будет направлен в ingress-proxy. ПРИМЕЧАНИЕ: приложение(old_main.py) весьма вероятно упадет под нагрузкой, но успеет обработать часть запросов - этого достаточно. Обновленная версия (main.py) не прошла достаточного тестирования временем, но должна справиться с нагрузкой.
(Необязательная часть) Дополнительно настройте remote ssh context к вашему серверу. Отобразите список контекстов и результат удаленного выполнения docker ps -a
В качестве ответа повторите sql-запрос и приложите скриншот с данного сервера, bash-скрипт и ссылку на fork-репозиторий.

# Ответ 4
SCRIPT

```
#!/bin/bash

docker rm -f $(docker ps -a -q)
git clone https://github.com/StepanovSA/shvirtd-example-python.git
cd ./shvirtd-example-python

docker compose up -d
```

VM
![alt text](https://github.com/StepanovSA/shvirtd-example-python/blob/main/Ex%204.PNG)

![alt text](https://github.com/StepanovSA/shvirtd-example-python/blob/main/docker%20vm.PNG)

SQL
![alt text](https://github.com/StepanovSA/shvirtd-example-python/blob/main/SQL%20VM.PNG)

![alt text](https://github.com/StepanovSA/shvirtd-example-python/blob/main/проверка.PNG)

```
https://github.com/StepanovSA/shvirtd-example-python
```

# Задача 6

Скачайте docker образ hashicorp/terraform:latest и скопируйте бинарный файл /bin/terraform на свою локальную машину, используя dive и docker save. Предоставьте скриншоты действий .

# Ответ 6

![alt text](https://github.com/StepanovSA/shvirtd-example-python/blob/main/Ex%206.1.PNG)

![alt text](https://github.com/StepanovSA/shvirtd-example-python/blob/main/Ex%206.2.PNG)


# Задача 6.1

Добейтесь аналогичного результата, используя docker cp.
Предоставьте скриншоты действий .

# Ответ 6.1

![alt text](https://github.com/StepanovSA/shvirtd-example-python/blob/main/Ex%206.3.PNG)
