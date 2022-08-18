## YaMDb API (База отзывов о фильмах, книгах и музыке)

### Описание
Проект **YaMDb** собирает отзывы (Review) пользователей на произведения (Titles). Произведения делятся на категории: «Книги», «Фильмы», «Музыка». Список категорий (Category) может быть расширен администратором (например, можно добавить категорию «Изобразительное искусство» или «Ювелирка»).

### Пользовательские роли
* **Аноним** — может просматривать описания произведений, читать отзывы и комментарии.
* **Аутентифицированный пользователь** (user) — может, как и Аноним, читать всё, дополнительно он может публиковать отзывы и ставить оценку произведениям (фильмам/книгам/песенкам), может комментировать чужие отзывы; может редактировать и удалять свои отзывы и комментарии. Эта роль присваивается по умолчанию каждому новому пользователю.
* **Модератор** (moderator) — те же права, что и у Аутентифицированного пользователя плюс право удалять любые отзывы и комментарии.
* **Администратор** (admin) — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.
* **Суперюзер Django** — обладет правами администратора (admin)

### Алгоритм регистрации пользователей
1. Пользователь отправляет POST-запрос на добавление нового пользователя с параметрами email и username на эндпоинт /api/v1/auth/signup/.
2. YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на адрес email.
3. Пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен).
4. При желании пользователь отправляет PATCH-запрос на эндпоинт /api/v1/users/me/ и заполняет поля в своём профайле (описание полей — в документации).

### Как запустить проект:

Клонировать репозиторий и перейти в него в командной строке:

```
git clone https://github.com/Romashovm/api_final_yatube.git
```

```
cd api_final_yatube
```

Cоздать и активировать виртуальное окружение:
Команда для установки виртуального окружения на Mac или Linux:
```
python3 -m venv env
source env/bin/activate
```
Команда для Windows должна быть такая:
```
python -m venv venv
source venv/Scripts/activate
```

```
python -m pip install --upgrade pip
```

Установить зависимости из файла requirements.txt:

```
pip install -r requirements.txt
```

Выполнить миграции:

```
python manage.py migrate
```

Запустить проект:

```
python manage.py runserver
```

### Заполнение таблиц тестовыми данными 
* Таблица reviews_user
  ```
  python manage.py parse_users_csv
  ```

* Таблица reviews_category
  ```
  python manage.py parse_category_csv
  ```

* Таблица reviews_genre
  ```
  python manage.py parse_genre_csv
  ```

* Таблица reviews_title
  ```
  python manage.py parse_title_csv
  ```

* Таблица reviews_title_genre
  ```
  python manage.py parse_genre_title_csv
  ```

## Примеры запросов:

### Запрос для просмотра категорий анонимным пользователем:
```
    [GET].../api/v1/categories/
```
### Пример ответа:
```
    {
    "count": 3,
    "next": null,
    "previous": null,
    "results": [
        {
            "name": "Книга",
            "slug": "book"
        },
        {
            "name": "Музыка",
            "slug": "music"
        },
        {
            "name": "Фильм",
            "slug": "movie"
        }
    ]
}
```
### Запрос на получение токена от зарегистрированного пользователя:
```
    [POST].../api/v1/auth/token/
{
  "username":"GrumpyCat"
  "confirmation_code":"61o-bb231266379b3908be63"
}
```
### Пример ответа:
```
{
  "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjU2MDAxNzkxLCJqdGkiOiIxNjlmOWY1MWE5NzE0MDIyYmI2ZDg2MTFhY2YxMTAyMCIsInVzZXJfaWQiOjEwNn0.C3TPNI7HPl6NWFE0fePPp7G-pF6fEvr0ohtC6me4z4A"
}
```

### Запрос на добавление отзыва пользователем GrumpyCat:
```
    [POST].../api/v1/titles/24/reviews/
{
  "text": "Очень люблю эту книгу. YOU ALWAYS GET BACK TO THE BASICS",
  "score": "10"
}
```
### Пример ответа:
```
{
  "id": 1,
  "author": "GrumpyCat",
  "title": "Generation П",
  "text": "Очень люблю эту книгу. YOU ALWAYS GET BACK TO THE BASICS",
  "pub_date": "2022-06-16T16:35:10.330811Z",
  "score": 10
}   
```


Полная документация доступна по адресу http://127.0.0.1:8000/redoc/
## Участники:

[Павел Бойков](https://github.com/pavelboykov). Управление пользователями (Auth и Users): система регистрации и аутентификации, права доступа, работа с токеном, система подтверждения e-mail, поля.

[Мария Пирогова](https://github.com/Maliarda). Категории (Categories), жанры (Genres) и произведения (Titles): модели, view и эндпойнты.

[Михаил Ромашов](https://github.com/Romashovm). Отзывы (Review) и комментарии (Comments): модели и view, эндпойнты. Рейтинги произведений.

