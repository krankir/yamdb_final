# YaMDb
![example workflow](https://github.com/krankir/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)
## Описание
Проект YaMDb собирает отзывы пользователей на произведения. 
Произведения делятся на категории: "Книги", "Фильмы", "Музыка". Читатели оставляют к произведениям отзывы и выставляют произведению рейтинг.

Проект развернут по адресу: <http://51.250.85.113/redoc/>

### Технологии
Python 3.8

Django 2.2.20

Docker

### Шаблон наполнения .env файла
- работаем с postgresql
```
DB_ENGINE=django.db.backends.postgresql 
```
- имя базы данных
```
DB_NAME=postgres
```
- пользователь базы данных
```
POSTGRES_USER=postgres
```
- пароль пользователя базы данных
```
POSTGRES_PASSWORD=postgres
```
- название контейнера
```
DB_HOST=db
```
- порт для работы с базой данных
```
DB_PORT=5432
```

### Запуск проекта в контейнерах Docker
- Перейдите в раздел infra для сборки docker-compose
```
docker-compose up
```
- Выполнить migrate
```
docker-compose exec web python manage.py migrate
```
- Для загрузки данных (опционально)
```
docker-compose exec web python manage.py loaddata db.json
```
- Создайте пользователя
```
docker-compose exec web python manage.py createsuperuser
```
- (или) Сменить пароль для пользователя admin
```
docker-compose exec web python manage.py changepassword admin
```
- Сформируйте STATIC файлы:
```
docker-compose exec web python manage.py collectstatic --no-input
```

# API ресурсы:
- **AUTH**: Аутентификация.
- **USERS**: Пользователи.
- **TITLES**: Произведения, к которым пишут отзывы.
- **CATEGORIES**: Категория произведений ("Фильмы", "Книги", "Музыка").
- **GENRES**: Жанры, одно из произведений может быть присвоено к нескольким жанрам.
- **REVIEWS**: Отзывы на произведения.
- **COMMENTS**: Комментарии к отзывам.

# Алгоритм регистрации пользователей
Пользователь отправляет POST-запрос с параметром email и username на `/api/v1/auth/signup`.
YaMDB отправляет письмо с кодом подтверждения (confirm_code) на адрес email (эмуляция почтовго сервера).
Пользователь отправляет POST-запрос с параметрами email и confirmation_code на `/api/v1/auth/token/`, 
в ответе на запрос ему приходит token.
Эти операции выполняются один раз, при регистрации пользователя. 
В результате пользователь получает токен и каждый раз отправяет его при запросе.

# Пользовательские роли
- **Аноним** — может просматривать описания произведений, читать отзывы и комментарии.
- **Аутентифицированный пользователь (user)** — может читать всё, как и Аноним, дополнительно может публиковать отзывы и ставить рейтинг произведениям (фильмам/книгам/песенкам), может комментировать чужие отзывы и ставить им оценки; может редактировать и удалять свои отзывы и комментарии.
- **Модератор (moderator)** — те же права, что и у Аутентифицированного пользователя плюс право удалять и редактировать любые отзывы и комментарии.
- **Администратор (admin)** — полные права на управление проектом и всем его содержимым. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.
- **Супер юзер** — те же права, что и у роли Администратор.

### Автор
Анатолий Редько

### License
MIT

