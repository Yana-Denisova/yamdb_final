# RESTful API для проекта YaMDb
YaMDb это интернет-сервис, где каждый может оставить свой отзыв на книгу,
кино, музыку. Пользователи могут поделиться впечатлениями и обсудить отзыв
в комментариях.  
Проект YaMDb развивается, что стало результатом создания YaMDb_api.
Через этот интерфейс смогут работать мобильное приложение или чат-бот;
через него же можно будет передавать данные в любое приложение или на фронтенд.

## Содержание
- [Технологии](#технологии)
- [Использование](#использование)
- [Deploy и CI/CD](#deploy-и-cicd)
- [Над проектом работали](#над-проектом-работали)

## Технологии
- [Python](https://www.python.org/)
- [Django](https://www.djangoproject.com/)
- [Django REST Framework](https://www.django-rest-framework.org/)
- [PostgreSQL](https://www.postgresql.org/)
- [Nginx](https://nginx.org/)
- [Gunicorn](https://gunicorn.org/)
- [Docker](https://www.docker.com/)
- [GitHub Actions](https://docs.github.com/en/actions)
- [Yandex.Cloud](https://cloud.yandex.ru/)


## Использование
Получение списка всех произведений.


``` 
GET /api/v1/titles/
```

RESPONSE:

```
[
  {
    "count": 0,
    "next": "string",
    "previous": "string",
    "results": [
      {
        "id": 0,
        "name": "string",
        "year": 0,
        "rating": 0,
        "description": "string",
        "genre": [
          {
            "name": "string",
            "slug": "string"
          }
        ],
        "category": {
          "name": "string",
          "slug": "string"
        }
      }
    ]
  }
]
```

Добавление новой публикации в коллекцию публикаций.Права доступа: Администратор.
Нельзя добавлять произведения, которые еще не вышли (год выпуска не может быть больше текущего).
При добавлении нового произведения требуется указать уже существующие категорию и жанр.

```
POST /api/v1/titles/
```

REQUEST:

```
{
  "name": "string",
  "year": 0,
  "description": "string",
  "genre": [
    "string"
  ],
  "category": "string"
}
```

Добавление нового отзыва. Пользователь может оставить только один отзыв на произведение.
Права доступа: Аутентифицированные пользователи.


```
POST /api/v1/titles/{title_id}/reviews/
```

REQUEST:

```
{
  "text": "string",
  "score": 1
}
```

Добавление комментария к отзыву.
Права доступа: Аутентифицированные пользователи.

```
POST /api/v1/titles/{title_id}/reviews/{review_id}/comments/
```

REQUEST:

```
{
  "text": "string"
}
```

## Deploy и CI/CD
Это учебный проект "CI и CD проекта api_yamdb".
При push в ветку `master` собирается, пушится на dockerhub и деплоится на боевой сервер новая версия приложения
### Шаблон наполнения env-файла
```
DB_ENGINE=django.db.backends.postgresql # указываем, что работаем с postgresql
DB_NAME=postgres # имя базы данных
POSTGRES_USER=postgres # логин для подключения к базе данных
POSTGRES_PASSWORD=postgrespass # пароль для подключения к БД (установите свой)
DB_HOST=db # название сервиса (контейнера)
DB_PORT=5432 # порт для подключения к БД

SECRET_KEY='secretkey' # Django Secret Key
```

## Над проектом работали
- [Yana-Denisova](https://t.me/DenisovaYana) - категории (Categories), жанры (Genres) и произведения (Titles): модели, представления и эндпойнты для них.
- [Алексей Туктанов](https://t.me/atuktanov) - управление пользователями (Auth и Users): система регистрации и аутентификации, права доступа, работа с токеном.
- [BuriloT](https://github.com/BuriloT) - отзывы (Review) и комментарии (Comments): модели и представления, эндпойнты.
