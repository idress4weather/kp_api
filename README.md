# КиноПоиск API (Gem) [![Build Status](https://travis-ci.org/groverz/kp_api.svg?branch=master)](https://travis-ci.org/groverz/kp_api) [![Gem Version](https://badge.fury.io/rb/kp_api.svg)](https://badge.fury.io/rb/kp_api)
Этот gem создан для упрощения работы с КиноПоиск API в проектах на Ruby.
Гем работает через API от кинопоиска. 
Используется соответственно не официально, прикидываясь андроидом.

### Что нового
* Изменено название гема.
* Восстановлена работа API использует последнюю версию подписи 5.0.0.
* Написаны тесты.
* Добавлен travis-ci

## Установка
Добавьте эту строку в Gemfile вашего приложения:
```ruby
gem 'kp_api'
```

Затем выполните:
```bash
$ bundle
```    

## Использование
### Глобальные функции
```ruby
api = KpApi::[Class] 
```
```ruby
api.status 
#   > true  [Получена следующая станица]
#   > false [Достигли конца пагинации]
```
```ruby
#   Оригинальный хеш кинопоиска
api.data 
```
```ruby
#   Оригинальный хеш кинопоиска (используется в некоторых классах для дополнительного запроса). 
#   Пример: film.peoples_full)
api.data2 
```
```ruby
#   Статус парсинга json 
#   Boolean
api.status 
```
```ruby
#   Статус парсинга json дополнительного запроса.
#   Boolean
api.status2
```
### Исключения 
При ошибке выбрасывается исключение __KpApi::ApiError__. Доступные методы.
```ruby
# http код ответа
e.code
=> "404"
```
```ruby
# Тело ответа
e.body
=> [key:false]
```
### Классы 
Практически каждый метод имеет отдельный класс. При инициализации класса 
выполняется запрос к АПИ, возвращая текущий объект или исключение.
#### Фильм
```ruby
film = KpApi::Film.new(film_id)
```
```ruby
#   Вся информация о фильме(включая rating, rent, budget).
film.view
```
```ruby
# Рейтинги фильма и т.д.
film.rating
```
```ruby
# Даты премьер и т.д.
film.rent
```
```ruby
# Бюджет и сборы.
film.budget
```
```ruby
# Люди связанные с фильмом(Не полный список, смотрите ниже).
film.peoples
```
```ruby
# Люди связанные с фильмом(Полный список). Генерирует новый запрос к API.
film.peoples_full
```
#### Люди(Режиссеры, актеры, операторы и т.д.).
Сервер не выдаёт __404__ на не существующего человека.
Исключение реализовано проверкой(NameRu, nameEn).
```ruby
people = KpApi::People.new(people_id)
```
```ruby
# Детальная информация о человеке.
people.view
```
```ruby
# Фильмы связанные с конкретным человеком.
people.films
```
```ruby
# Массив id фильмов связанных с конкретным человеком.
people.films_ids
```
#### Категории
Список категорий, не понятно для чего нужен параметр `country_id`, но без него не
проходит запрос. По умолчанию 2(Россия).
```ruby
category = KpApi::Category.new(country_id=2)
```
```ruby
# Страны
category.countries
```
```ruby
# Страны (требует доп. запроса)
category.cities
```
```ruby
# Жанры
category.genres
```
#### Сегодня в кино
Список фильмов идущих сегодня в кино.
Если города не существует возвращает пустой массив.

* Параметр  `city_id`    не обязательный, по умолчанию 1(Москва). 
* Параметр  `country_id` не обязательный, по умолчанию 2(Россия). 
Возможно не используется, т.к. первый уникальный, но приложение передает его в заголовках.

Доступны только эти страны:
* 2 - Россия
* 62 - Украина
* 69 - Беларусь
* 122 - Казахстан
* 71 - Узбекистан
```ruby
today = KpApi::Today.new(city_id=1, country_id=2)
```
```ruby
#   Список фильмов
today.view
```
```ruby
#   Массив id фильмов
today.film_ids
```
### Поиск
#### Глобальный поиск
```ruby
search = KpApi::GlobalSearch.new('Бразилия')
```
```ruby
#   Найдено ли что-нибудь
search.found?
```
```ruby
#   Количество найденных фильмов
search.films_count
```
```ruby
#   Количество найденных фильмов
search.peoples_count
```
```ruby
#   Скорее всего вы искали этот фильм
search.youmean
```
```ruby
#   Остальные найденные фильмы(лимит: 3)
search.films
```
```ruby
#   Или даже этих людей
search.peoples
```
#### Поиск по фильмам
```ruby
search = KpApi::FilmSearch.new('Привет')
```
```ruby
#   Найдено ли что-нибудь)
search.found?
```
```ruby
#   Список фильмов (не более 20)
search.view
```
```ruby
#   Количество найденных фильмов
search.films_count
```
```ruby
#   Текущая страница
search.current_page
```
```ruby
#   Количество страниц
search.page_count
```
```ruby
#   Следующая страница(новый запрос к АПИ)
search.next_page
#   > true  [Получена следующая станица]
#   > false [Достигли конца пагинации]
```
#### Поиск по людям
```ruby
search = KpApi::PeopleSearch.new('Андрей')
```
```ruby
#   Список людей (не более 20)
search.view
```
```ruby
#   Количество найденных фильмов
search.peoples_count
```
```ruby
#   Текущая страница
search.current_page
```
```ruby
#   Количество страниц
search.page_count
```
```ruby
#   Следующая страница
search.next_page
#   > true  [Получена следующая станица]
#   > false [Достигли конца пагинации]
```
#### Живой поиск
@to-do
### Топы
@to-do



##Благодарность    
Всем кто связывался со мной. (@to-do)


## License
The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
