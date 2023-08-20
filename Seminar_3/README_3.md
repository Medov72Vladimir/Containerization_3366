# Контейнеризация (семинары)

## Урок 3. Введение в Docker

**<u>1. Установим Докер</u>**<br>

Обновим списки пакетов:
> apt update

![скриншот](image_1.1.PNG)

Установим пакеты, которые позволят использовать репозиторий по HTTPS:
> apt install apt-transport-https ca-certificates curl software-properties-common

![скриншот](image_1.2.PNG)

Добавим официальный GPG-ключ Docker:
> curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

![скриншот](image_1.3.PNG)

Добавим репозиторий Docker к списку источников пакетов:
> echo "deb [signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

![скриншот](image_1.4.PNG)

Обновим список пакетов, чтобы включить информацию о пакетах Docker из добавленного репозитория:
> apt update

![скриншот](image_1.5.PNG)

Установим Docker:
> apt install docker-ce

Добавим текущего пользователя в группу docker, чтобы исключить использование sudo при запуске команд Docker:
> sudo usermod -aG docker $USER

Перезагрузим группы пользователя, чтобы изменения в группе docker вступили в силу:
> newgrp docker

Теперь мы готовы использовать Docker через терминал. Для этого проверим его работу, выполнив команду:
> docker --version

![скриншот](image_1.6.PNG)

**<u>2. Протестируем работу Докер</u>**<br>

Запустим Docker:
> sudo systemctl start docker


Запустим контейнер, используя образ "cowsay":
> docker run docker/whalesay cowsay Hello, Docker!

![скриншот](image_2.1.PNG)

<u>Теперь запустим контейнеры с различными рисунками животных с использованием cowsay. Мы можем заменить текст "Hello, Docker!" на любой другой текст, который захотим, чтобы животное "сказало". Таким образом, мы сможем не только визуально проверить работу Docker, но и весело провести время!</u>

Запустим контейнер с рисунком слона:
> docker run docker/whalesay cowsay -f elephant "Hello, Vladimir Medov!"

![скриншот](image_2.2.PNG)

Запустим контейнер с рисунком пингвина:
>  docker run docker/whalesay cowsay -f tux "Hello, Vladimir Medov!"

![скриншот](image_2.3.PNG)

Запустим контейнер с рисунком дракона:
> docker run docker/whalesay cowsay -f dragon "Hello, Vladimir Medov!"

![скриншот](image_2.4.PNG)

Запустим контейнер с рисунком кенгуру:
> docker run docker/whalesay cowsay -f kangaroo "Hello, Vladimir Medov!"

Запустим контейнер с рисунком утки:
> docker run docker/whalesay cowsay -f duck "Hello, Vladimir Medov!"

Запустим контейнер с рисунком панды:
> docker run docker/whalesay cowsay -f panda "Hello, Vladimir Medov!"

Запустим контейнер с рисунком совы:
> docker run docker/whalesay cowsay -f owl "Hello, Vladimir Medov!"

Запустим контейнер с рисунком кота:
> docker run docker/whalesay cowsay -f kitty "Hello, Vladimir Medov!"

![скриншот](image_2.5.PNG)

**<u>3. Команды Докер</u>**<br>

Создание и запуск контейнеров:

**docker run** - запускает контейнер из образа.\
**docker start** - запускает остановленный контейнер.\
**docker stop** -  останавливает работающий контейнер.\
**docker restart** - перезапускает контейнер.\
**docker exec** -  выполняет команду внутри запущенного контейнера.

Управление контейнерами:

**docker rm $(docker ps -aq)** - удалит все остановленные контейнеры.\
**docker ps** - просмотр списка запущенных контейнеров.\
**docker ps -a** - просмотр списка всех контейнеров (включая остановленные).<br>
**docker rm** - удаляет контейнер.<br>
**docker logs** - просмотр логов контейнера.

Работа с образами:

**docker images** - просмотр списка образов.\
**docker pull** - загрузка образа с Docker Hub.<br>
**docker build** - сборка образа из Dockerfile.\
**docker rmi** - удаляет образ.

**<u>4. Хранение данных в контейнерах Docker: Руководство с пояснениями</u>**<br>
Часть -1

**<u>Цель:</u>**<br>разберемся с тем, как можно хранить данные в контейнерах. Это критически важно для инженеров, работающих с Docker, так как хранение данных - ключевой аспект работы.

Запустим контейнер из образа Ubuntu и войдем в него:
> docker run -it -h GB --name gb-test ubuntu:22.10

![скриншот](image_4.1.PNG)

Посмотрим содержимое корневой директории:
> ls -l /

![скриншот](image_4.2.PNG)

Создадим новую директорию в корне:
> mkdir /example

![скриншот](image_4.3.PNG)

В каталоге example создадим файл "passwords.txt":
> touch /example/passwords.txt

![скриншот](image_4.4.PNG)

Добавим в файл "passwords.txt" какие-либо данные:
> echo "123test" >> /example/passwords.txt

Просмотрим содержимое файла passwords.txt:
> cat passwords.txt

![скриншот](image_4.5.PNG)

Попробуем остановить контейнер и затем запустить его снова. Сохранятся ли наши данные?:
> exit\
> docker stop gb-test\
> docker start gb-test\
> docker exec -it gb-test bash\
> cat /example/passwords.txt

![скриншот](image_4.6.PNG)

<u>Результат:</u><br> данные сохранились, так как мы не пересоздавали контейнер.

Удалим контейнер и создадим его заново, используя те же команды. Сохранятся ли теперь наши данные?:
> exit\
> docker stop gb-test\
> docker rm gb-test\
> docker run -it -h GB --name gb-test ubuntu:22.10

![скриншот](image_4.7.PNG)

<u>Результат:</u><br> данные не сохранились, так как контейнер был удален и создан заново.

Рассмотрим наиболее интересный вариант для сохранения данных - использование внешнего хранилища. Создадим директорию и подмонтируем ее к контейнеру:
> mkdir testseminar3\
> docker run -it -h GB --name gb-test1 -v /home/vmedov/testseminar3:/otherway ubuntu:22.10

![скриншот](image_4.8.PNG)

Добавим данные в подмонтированную директорию:
> echo "$HOSTNAME" >> /otherway/test.txt

Просмотрим содержимое файла test.txt в контейнере:
> cat otherway/test.txt

![скриншот](image_4.9.PNG)

Проверим доступность данных с локальной системы:
> exit\
> cat testseminar3/test.txt

![скриншот](image_4.10.PNG)

Удалим контейнер и создадим его снова, подмонтировав директорию:
> docker rm gb-test1\
>  docker run -it -h GB --name gb-test1 -v /home/vmedov/testseminar3:/otherway ubuntu:22.10

Просмотрим содержимое файла test.txt в контейнере:
> cat otherway/test.txt

![скриншот](image_4.11.PNG)

<u>Результат:</u><br>мы видим, что данные по-прежнему доступны.

**Заключение:**\
<u>Самый надежный способ хранения данных в контейнерах - использование внешних хранилищ. Важно избегать хранения важных данных внутри контейнеров, чтобы предотвратить потерю информации.</u>

**<u>4. Хранение данных в контейнерах Docker: Практическое руководство.</u>**\
Часть -2

Создадим папку, которую мы будем готовы смонтировать в контейнер:
> mkdir ~/docker-mount-example

В этой папке создадим файл test.txt и наполним его данными:
> echo "This is the host test.txt file" > ~/docker-mount-example/test.txt

![скриншот](image_4.12.PNG)

В домашней директории создаем файл test.txt, который также понадобится для монтирования в контейнер, но с другим содержимым:
> echo "This is the root test.txt file" > ~/test.txt

Создаем контейнер из образа ubuntu:22.10 и задаем ему имя и hostname:
> docker run -it -h GB --name gb-test2 ubuntu:22.10

![скриншот](image_4.13.PNG)

Выходим из контейнера, останавливаем и удаляем контейнер:
> exit\
> docker stop gb-test && docker rm gb-test

![скриншот](image_4.14.PNG)

Создаем контейнер из образа ubuntu:22.10, задаем ему имя и hostname и монтируем ранее созданную папку с хоста в контейнер:
> docker run -it -h GB --name gb-test2 -v ~/docker-mount-example:/container-mount -v ~/test.txt:/container-mount/test.txt ubuntu:22.10

Смотрим содержимое текстового файла в контейнере:
> cat /container-mount/test.txt

Останавливаем и удаляем контейнер:
> docker stop gb-test2 && docker rm gb-test2

![скриншот](image_4.15.PNG)

Создаем контейнер из образа ubuntu:22.10, задаем ему имя и hostname и монтируем созданный ранее текстовый файл из домашней директории внутрь смонтированной папки в контейнере:
> docker run -it -h GB --name gb-test2 -v ~/docker-mount-example:/container-mount -v ~/test.txt:/container-mount/test.txt ubuntu:22.10

![скриншот](image_4.16.PNG)

Смотрим содержимое текстового файла в контейнере:
> cat /container-mount/test.txt

![скриншот](image_4.17.PNG)

Мы создали контейнер и монтировали папку docker-mount-example внутрь контейнера. Затем мы монтировали файл test.txt из домашней директории внутрь этой папки в контейнере. При просмотре содержимого файла в контейнере, вы увидите данные из файла в домашней директории.