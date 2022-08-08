# YaMDb API

Проект YaMDb собирает отзывы пользователей на различные произведения

![example workflow](https://github.com/aVeter77/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

Пример работы приложения [http://yamdb.aveter77.site/redoc/](http://yamdb.aveter77.site/redoc/)

Образ доступен на [Dockerhub](https://hub.docker.com/r/aveter77/api_yamdb/tags).
## Алгоритм регистрации пользователей
1. Пользователь отправляет POST-запрос на добавление нового пользователя с параметрами `email` и `username` на эндпоинт `/api/v1/auth/signup/`.
2. YaMDB отправляет письмо с кодом подтверждения (`confirmation_code`) на адрес `email`.
3. Пользователь отправляет POST-запрос с параметрами `username` и `confirmation_code` на эндпоинт `/api/v1/auth/token/`, в ответе на запрос ему приходит `token` (JWT-токен).
4. При желании пользователь отправляет PATCH-запрос на эндпоинт `/api/v1/users/me/` и заполняет поля в своём профайле (описание полей — в документации).

## Пользовательские роли

- **Аноним** — может просматривать описания произведений, читать отзывы и комментарии.
- **Аутентифицированный пользователь** (`user`) — может, как и **Аноним**, читать всё, дополнительно он может публиковать отзывы и ставить оценку произведениям (фильмам/книгам/песенкам), может комментировать чужие отзывы; может редактировать и удалять **свои** отзывы и комментарии. Эта роль присваивается по умолчанию каждому новому пользователю.
- **Модератор** (`moderator`) — те же права, что и у **Аутентифицированного пользователя** плюс право удалять **любые** отзывы и комментарии.
- **Администратор** (`admin`) — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.
- **Суперюзер Django** — обладет правами администратора (`admin`)

## Технологии
- [Python 3.7](https://www.python.org/)
- [Django 2.2.16](https://www.djangoproject.com/)
- [Django Rest Framework 3.12.4](https://www.django-rest-framework.org/)
- [PostgreSQL 13.0](https://www.postgresql.org/)
- [gunicorn 20.0.4](https://pypi.org/project/)
- [nginx 1.21.3](https://nginx.org/ru/)
- [Docker 20.10.17](https://www.docker.com/)
- [Docker Compose 1.29.2](https://docs.docker.com/compose/)

## Запуск

Установите переменные среды, как в `.env.example`.
### Docker
```
cd infra/
docker-compose up -d
```
После запуска выполните команды:
```
docker-compose exec web python manage.py migrate
docker-compose exec web python manage.py createsuperuser
docker-compose exec web python manage.py collectstatic --no-input 
```

## Заполнение базы начальными данными
```
cd infra/
cat fixtures.json | docker-compose exec -T web python manage.py loaddata --format=json -
```

## Примеры запросов

**Регистрация нового пользователя:**
```
POST /api/v1/auth/signup/
```
```
{
    "email": "string",
    "username": "string"
}
```
**Получение JWT-токена:**

```
POST /api/v1/auth/token/
```
```
{
  "username": "string",
  "confirmation_code": "string"
}
```

**Получение списка всех пользователей:**

```
GET /api/v1/users/
```

**Регистрация пользователя:**

```
POST /api/v1/auth/signup/
```
```
{
  "email": "string",
  "username": "string"
}
```
**Полная документация** по API доступна после запуска `http://localhost/redoc/`

## Автор
Александр Николаев

## Лицензия

MIT
