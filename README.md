# **Проект "Kittygram"**

## ****Описание****
Kittygram - cоциальная сеть для обмена фотографиями любимых питомцев. Пользователи смогут заходить на cайт, смотреть фотографии других питомцев, делиться своими любимцами и присваивать их достижения. API для Kittygram обеспеченивает возможности взаимодействия между различными компонентами проекта, а также с другими приложениями и сервисами.

## ****Использованные технологии****
- [Python](https://www.python.org/) - язык программирования;
- [Django](https://django.fun/ru/docs/django/4.1/)- cвободный фреймворк для веб-приложений на языке Python, использующий шаблон проектирования MVC;
- [Django REST framework](https://www.django-rest-framework.org/) - это мощный и гибкий набор инструментов для создания Web API;
- [Django Rest framework-Simple JWT(JSON WEB Token)](https://django-rest-framework-simplejwt.readthedocs.io/en/latest/) - это открытый стандарт для создания токенов доступа, основанный на формате JSON;
- [Requests](https://requests.readthedocs.io/en/latest/index.html) - данная библиотека упрощает генерацию HTTP-запросов к другим сервисам, помогает писать их очень просто и быстро;
- [React](https://ru.react.js.org/) - это библиотека JavaScript с открытым кодом для создания внешних пользовательских интерфейсов;
- [WSGI-сервер Gunicorn](https://gunicorn.org/) - веб-сервер исполняет код и отправляет связанную с http-запросом информацию и callback-функцию в веб-приложение. Затем запрос на стороне приложения обрабатывается и высылается ответ на веб-сервер.
- [Веб- и прокси-сервер Nginx](https://nginx.org/ru/docs/beginners_guide.html) - основная задача состоит в обслуживании запросов к статитическим файлам, страницы приложения прогружались с использованием указанных в коде стилей.
- [Certbot](https://certbot.eff.org/) - для шифрования HTTPS используются SSL-сертификаты, с помощью официального клиента Certbot можно получить SSL-сертификат; 

## ****Установка проекта****
#### Подключение проекта и установка на сервер:
 - [ ] Подключитесь к удалённому серверу по SSH-ключу:
```
ssh -i путь_до_SSH_ключа/название_файла_с_SSH_ключом_без_расширения login@ip
```
 - [ ] Клонируйте проект с GitHub на сервер:
```
git clone git@github.com:ваш_аккаунт/infra_sprint1.git
```
#### Настройка бэкенд-приложения:
 - [ ] Создайте и активируйте виртуальное окружение:
```
source venv/bin/activate
```
 - [ ] Установите зависимости из файла requirements.txt:
```
pip install -r requirements.txt
```
 - [ ] В папке с файлом manage.py выполните миграции:
```
python manage.py migrate
```
 - [ ] Создайте суперпользователя:
```
python3 manage.py createsuperuser
```
 - [ ] Соберите статику бэкенд-приложения:
```
python3 manage.py collectstatic
```
```
sudo cp -r путь_к_директории_с_бэкендом/static_backend /var/www/название_проекта
```
#### Создания пространства переменных окружения в файле .env:
 - [ ] Создайте файл .env в той же директории, что и исполняемый файл:
```
touch .env
```
 - [ ] Добавьте в файл .env переменные:
```
nano .env
```
```
SECRET_KEY = 'ВАШ_ТОКЕН'
````
```
DEBUG = 'False'
```
```
ALLOWED_HOSTS = 'IP сервера' 'ваш_домен'
```

#### Настройка фронтенд-приложения:
 - [ ] Находясь в директории с фронтенд-приложением, установите зависимости для него:
```
npm i
```
 - [ ] Из директории с фронтенд-приложением выполните команду:
```
npm run build
```
```
sudo cp -r путь_к_директории_с_фронтенд-приложением/build/. /var/www/имя_проекта/
```
#### Установка и настройка WSGI-сервера Gunicorn:
 - [ ] Подключитесь к удалённому серверу, активируйте виртуальное окружение
бэкенд-приложения и установите пакет gunicorn:
```
pip install gunicorn==20.1.0
```
```
gunicorn --bind 0.0.0.0:8080 kittygram_backend.wsgi
```
 - [ ] Создайте файл конфигурации юнита systemd для Gunicorn в директории (скопируйте файл gunicorn_kittygram.service с папки infra)
/etc/systemd/system/gunicorn_kittygram.service:
```
sudo nano /etc/systemd/system/gunicorn_kittygram.service
```
 - [ ] Команда sudo systemctl с параметрами start, stop или restart запустит, остановит
или перезапустит Gunicorn:
```
sudo systemctl start gunicorn_kittygram.service
```
#### Установка и настройка WSGI-сервера Gunicorn:
 - [ ] Установите Nginx на удалённый сервер:
```
sudo apt install nginx -y
```
```
sudo systemctl start nginx
```
 - [ ] Обновите настройки Nginx в файле конфигурации веб-сервера default(скопируйте файл default с папки infra):
```
sudo nano /etc/nginx/sites-enabled/default
```
 - [ ] Перезагрузите конфигурацию Nginx::
```
sudo systemctl reload nginx
```
#### Настройка файрвола ufw:
 - [ ] Файрвол установит правило, по которому будут закрыты все порты, кроме тех, которые
вы явно укажете:
```
sudo ufw allow 'Nginx Full'
```
```
sudo ufw allow OpenSSH
```
```
sudo ufw enable
```
```
sudo ufw status
```
#### Автоматизация тестирования и деплой проекта с помощью GitHub Actions:
 - [ ]  Файл .github/workflows/main.yml workflow будет:
```
Проверять код бэкенда в репозитории на соответствие PEP8;
```
```
Запускать тесты для фронтенда и бэкенда (тесты уже написаны);
```
```
Собирать образы проекта и отправлять их на Docker Hub (замените username на ваш логин на Docker Hub):
username/kittygram_backend,
username/kittygram_frontend,
username/kittygram_gateway.
```
```
Обновлять образы на сервере и перезапускать приложение при помощи Docker Compose;
```
```
Выполнять команды для сборки статики в приложении бэкенда, переносить статику в volume; выполнять миграции;
```
```
Извещать вас в Telegram об успешном завершении деплоя.
```

## **Автор**
- Алексей Коренков
