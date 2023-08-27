# Контейнеризация (семинары)

## Урок 5. Docker Compose и Docker Swarm

**<u>Задание:</u>**<br> создать сервис, состоящий из 2 различных контейнеров: 1 - веб, 2 - БД (compose).

1. Создадим файл docker-compose.yaml и пропишем в него сервисы:

    vim docker-compose.yaml

![скриншот](image_1.PNG)

2. Устанавим docker-compose, если его нет:

    apt install docker-compose

![скриншот](image_2.PNG)

3. Запустим наш проект в фоновом режиме:

    docker-compose up -d

![скриншот](image_3.PNG)

4. Выведемм на экран все доступные контейнеры:

    docker-compose ps

![скриншот](image_4.PNG)

5. Проверим работу проекта в браузере, перейдя по адресу:

    http://localhost:6080/

![скриншот](image_5.PNG)

![скриншот](image_6.PNG)

