СПРИНТ 16. yamdb_final

Проект доступен по ссылке - http://51.250.110.56/api/v1/

Бейдж о статусе работы workflow: [![yamdb](https://github.com/roman7373/yamdb_final/workflows/yamdb_workflow/badge.svg)]

Описание
Проект YaMDb собирает отзывы пользователей на произведения. Произведения (Titles) делятся на категории (Cathegories): «Книги», «Фильмы», «Музыка»; и могут относиться к различным жанрам (Genres). Аутентифицированные пользователи могут оставлять оценки (Reviews) с комментариями (Comments) к выбранным произведениям.

В проекте задействованы:

Python
Django
Docker
Nginx
DevOps

Запуск проекта:

Установите Docker по инструкции с официального сайта

Склонируйте репозиторий:

git clone https://github.com/roman7373/infra_sp2

Cоздать и активировать виртуальное окружение:

python3 -m venv venv source venv/bin/activate python3 -m pip install --upgrade pip

Установить зависимости из файла requirements.txt:

pip install -r api_yamdb/requirements.txt

Запуск контейнеров с приложением:

#Переходим в папку с docker-compose cd infra/

Создать файл .env и заполнить по шаблону ниже:

docker-compose up -d --build docker-compose exec web python manage.py migrate docker-compose exec web python manage.py createsuperuser docker-compose exec web python manage.py collectstatic --no-input

Загрузить данные с примера в базу данных:

#С корневой папки проекта api_yamdb/python manage.py loaddata infra/fixtures.json

Остановить приложение:

docker-compose down -v

Описание файла .env

SECRET_KEY=<...> # ключ из settings.py DB_ENGINE=<...> # указываем, что работаем с postgresql DB_NAME=<...> # имя базы данных POSTGRES_USER=<...> # логин для подключения к базе данных POSTGRES_PASSWORD=<...> # пароль для подключения к БД (установите свой) DB_HOST=<...> # название сервиса (контейнера) DB_PORT=<...> # порт для подключения к БД

Алгоритм регистрации пользователей
Пользователь отправляет запрос с параметром email на /auth/email/.
YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на адрес email .
Пользователь отправляет запрос с параметрами email и confirmation_code на /auth/token/, в ответе на запрос ему приходит token (JWT-токен).
При желании пользователь отправляет PATCH-запрос на /users/me/ и заполняет поля в своём профайле (описание полей — в документации).
Пользовательские роли
Аноним — может просматривать описания произведений, читать отзывы и комментарии.
Аутентифицированный пользователь — может, как и Аноним, читать всё, дополнительно он может публиковать отзывы и ставить рейтинг произведениям (фильмам/книгам/песенкам), может комментировать чужие отзывы и ставить им оценки; может редактировать и удалять свои отзывы и комментарии.
Модератор — те же права, что и у Аутентифицированного пользователя плюс право удалять любые отзывы и комментарии.
Администратор — полные права на управление проектом и всем его содержимым. Может создавать и удалять категории и произведения. Может назначать роли пользователям.
Администратор Django — те же права, что и у роли Администратор.
Пути для работы с API
Запросы к API начинаются с /api/v1/

Произведения - /titles/, /titles/<title_id>/ Оценка - /titles/<title_id>/review/, /titles/<title_id>/review/<review_id>/ Комментарии - /titles/<title_id>/review/<review_id>/comment/, /titles/<title_id>/review/<review_id>/comment/<comment_id>/ Категории - /cathegories/ Произведениям - /genres/

Подробное описание API приводится по адресу: /static/redoc.yaml


Автор:

Романенко Роман