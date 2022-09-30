 # Проект YaMDb
 ![example workflow](https://github.com/Dimtiv/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg?event=push)
# Описание проекта
Проект собирает отзывы пользователей на произведения. Каждое произведение относится к определенной категории и ему может быть присвоен жанр. Пользователи могут оставлять отзыв и оценку произведению, комментировать отзывы других пользователей.

#### Создайте файл .env с переменными окружения для работы с базой данныхв папке /infra:

За основу возьмите файл .env.example, располоденный в папке /infra и отредактируйте его
```
nano .env.example
```
Переименуйте отредактированный файл
```
mv .env.example .env
```

#### Развернуть проект в котнейнере:
Запустите docker-compose в "фоновом" режиме командой
```
docker-compose up -d
```

Остановить контейнер можно командой:
```
docker-compose stop
```
Остановить и удалить контейнеры со всеми зависимостями можно по команде
```
docker-compose down -v
```

#### В контейнере web выполните миграции, создайте пользователя, соберите статику:
Выполните последовательно команды:
```
docker-compose exec web python manage.py migrate
```

```
docker-compose exec web python manage.py createsuperuser
```
```
docker-compose exec web python manage.py collectstatic --no-input
```
#### Заполнение базы данными:

```
docker-compose exec web python manage.py loaddata fixtures.json
```
### В проекте доступны Эндпойнты
| адрес                                         | метод             | описание                                                                                                                 |
|-----------------------------------------------|-------------------|--------------------------------------------------------------------------------------------------------------------------|
| api/v1/auth/signup/                            | POST              | регистрация нового пользователя                                                                                 |
| api/v1/auth/signup/                          | POST              | получение JWT-токена                                                                                                      |
| api/v1/categories/                          | GET, POST, DELETE | получение списка всех категорий, добавление, удаление категории                                                                                                        |
| api/v1/genres/                                | GET, POST, DELETE    | получение списка всех жанров, добавление, удаление жанров                                                                      |
| api/v1/titles/                                | GET, POST         | получение списка всех произведений,    добавление нового произведения                                                                                           |
| api/v1/titles/{title_id}/                    | GET, PATCH, DELETE   | получение информации о произведении по id, частичное обновление, удаление                                                                            |
| api/v1/titles/{title_id}/reviews/              | GET, POST         | получение списка всех отзывов,    добавление нового отзыва                                                                                              |
| api/v1/titles/{title_id}/reviews/{review_id}/   | GET, PATCH, DELETE   | получение информации о отзыве на произведение по id, частичное обновление, удаление                                                                                      |
| api/v1/titles/{title_id}/reviews/{review_id}/comments/ | GET, POST         | получение списка всех комментариев к отзыву, добавление нового комментария |
| api/v1/titles/{title_id}/reviews/{review_id}/comments/{comment_id}/ | GET, PATCH, DELETE   | получение информации о комментарии на отзыв по id, частичное обновление, удаление                                                |
| api/v1/users/                                | GET, POST         | получение списка всех пользователей, добавление нового пользователя                                                                     |
| api/v1/users/{username}/                               | GET, PATCH, DELETE   | получение информации о пользователе, частичное обновление, удаление                                                                          |
| api/v1/users/me/                               | GET, PATCH        | получение данных своей учетной записи, изменение своей учетной записи                                                                          |

# Примеры запросов и ответов
### Регистрация пользователя. 
>Адрес 
> >api/v1/auth/signup/ 
> 
>метод 
> >POST
> 
>запрос
> >{
"email": "string",
"username": "string"
}
> >
> ответ
> > {
"email": "string",
"username": "string"
}

### Получение JWT-токена. 
>Адрес 
> >/api/v1/auth/token/
> 
>метод 
> >POST
> 
>запрос
> >{
"username": "string",
"confirmation_code": "string"
}
> >
> ответ
> > {
"token": "string"
}

### Создание категории. 
>Адрес 
> >/api/v1/categories/
> 
>метод 
> >POST
> 
>запрос
> >{
"name": "string",
"slug": "string"
}
> >
> ответ
> > {
"name": "string",
"slug": "string"
}

### Получение списка жанров. 
>Адрес 
> >/api/v1/genres/
> 
>метод 
> >GET
> 
> ответ
> > {
"count": 0,
"next": "string",
"previous": "string",
"results": [
{
"name": "string",
"slug": "string"
}
]
}

### Добавление произведения. 
>Адрес 
> >/api/v1/titles/
> 
>метод 
> >POST
> 
>запрос
> >{
"name": "string",
"year": 0,
"description": "string",
"genre": [
"string"
],
"category": "string"
}
> >
> ответ
> > {
"id": 0,
"name": "string",
"year": 0,
"rating": 0,
"description": "string",
"genre": [
{}
],
"category": {
"name": "string",
"slug": "string"
}
}

### Частичное обновление отзыва. 
>Адрес 
> >/api/v1/titles/{title_id}/reviews/{review_id}/
> 
>метод 
> >PATCH
> 
>запрос
> >{
"text": "string",
"score": 1
}
> >
> ответ
> > {
"id": 0,
"text": "string",
"author": "string",
"score": 1,
"pub_date": "2019-08-24T14:15:22Z"
}