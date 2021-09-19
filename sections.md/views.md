# Представления

Представления организованы подключением шаблонизатора `handlebars`, который обрабатывает и виззуализирует данные, которые предоставляются в файлах `hbs`. Данные, которые отрисовывает шаблонизатор управляются контроллерами, а маршруты по которым шаблоны визуализируются управляются маршрутизатором.

⬅️ **Подробную информацию обо всех маршрутах смотреть в разделе [Маршрутизация](routes.md)**
⬅️ **Подробную информацию обо всех контроллерах смотреть в разделе [Контроллеры](controllers.md)**

## Оглавление раздела
- [Представления](#представления)
  - [Оглавление раздела](#оглавление-раздела)
  - [Структура шаблонизации](#структура-шаблонизации)
  - [Таблица всех представлений](#таблица-всех-представлений)
  - [Шаблоны](#шаблоны)
    - [Шаблон "empty"](#шаблон-empty)
    - [Шаблон "main"](#шаблон-main)
  - [Cлои](#cлои)
    - [Слой "head"](#слой-head)
    - [Слой "navbar"](#слой-navbar)
    - [Слой "footer"](#слой-footer)
  - [Основные страницы](#основные-страницы)
    - [Главная](#главная)
    - [Курсы](#курсы)
    - [Добавить курс](#добавить-курс)
    - [Профиль](#профиль)
    - [Корзина](#корзина)
    - [Заказы](#заказы)
    - [Вход / Выход](#вход--выход)
      - [Вход / Регистрация](#вход--регистрация)
      - [Сброс пароля](#сброс-пароля)
      - [Смена пароля](#смена-пароля)
  - [Дополнительные страницы](#дополнительные-страницы)
    - [Страница одного курса](#страница-одного-курса)
    - [Редактировать курс](#редактировать-курс)
    - [Страница 404](#страница-404)


## Структура шаблонизации

Визуализация данных благодаря шаблонизатору происходит по следующей процедуре:
**1. Подключение шаблонизатора в главный файл `index.js`:**

Блок кода:
```node
const exphbs = require('express-handlebars')

const hbs = exphbs.create({
  defaultLayout: 'main',
  extname: 'hbs',
  helpers: require('./utils/hbs-helpers')
})
```
Где:
- `exphbs.create({})` -
- `defaultLayout` -
- `extname` -
- `helpers` -
Блок кода:
```node
app.engine('hbs', hbs.engine)
app.set('view engine', 'hbs')
app.set('views', 'views')
```

**2. Создание шаблонов:**

Шаблон это подоснова или структура. Разработчик, исходя из целей может сформировать для определенной визуализации - конкретный шаблон или же использовать общий шаблон для нескольких визуализаций. Практика показала, что есть существенное количество участков визуализации, которые являются похожими по своей структуре. Учитывая, что `Handlebars` позволяет определить шаблон по умолчанию, то мы успешном реализовыванием две задачи в одной. В главной файле `index.js` определяем шаблон по умолчанию:
```node
...
const hbs = exphbs.create({
  defaultLayout: 'main',
  ...
})
...
```
Где:
- `exphbs.create({})` -
- `defaultLayout` - `main`

**3. Интеграция шаблонов в систему маршрутов:**

При определении маршрутизации разных страниц приложения (необходимо понимать, что под разными страницами подразумевается разными маршруты оцифровки разных кусков кода, само же приложение является по сути одностраничным) в маршрутах прописывается функция рендера или визуализации. Рассмотрим интеграцию шаблонизатора на примере маршрутизатора "add":
```node
router.get('/', auth, (req, res) => {
  res.render('add', {
    title: 'Добавить курс',
    isAdd: true
  })
})
```
Где: 
- `router.get('/', auth, (req, res) => {})` - маршрутизатор с типом запросом `get`, который перенаправляет на страницу `/add`.
  - `auth` - промежуточный обработчик авторизации.
  - `req` - объект запроса.
  - `res` - объект ответа.
- `res.render('add', {})` - функция визуализации `render` объекта ответа `res`, которая принимает три параметра:
    - `add` - обязательный параметр. Определяет код визуализации. В нашем случае код визуализации - страница `add` шаблонизатора `Handlebars`. Код визуализации может быть также строкой.
    - `{}` - необязательный параметр. Объект с локальными данными, которые передаються объектом `res` в шаблон `add`.
    - `function () {}` - необязательный параметр. Callback функция.
- `title` - ключ принимающий значение `'Добавить курс'`.
- `isAdd` - флаг-переключатель. По умолчанию флаг имеет значение: `true`.

Следует отметить, что поскольку в передаваемых, локальных данных, мы не передаём ключ: `layout` зарезервированный `handlebars`, то шаблонизатор для отрисовки страницы `add` берёт шаблон по умолчанию - `main`, который мы подключали в главном файле `index.js` выше. В случае, если бы не было шаблона по умолчанию, движок `Handlebars` вывел бы ошибку.
Вот пример визуализации страницы одного курса с использованием индивидуального шаблона:
```node
router.get('/:id', async (req, res) => {
  try {
    const course = await Course.findById(req.params.id)
    res.render('course', {
      layout: 'empty',
      title: `Курс ${course.title}`,
      course
    })
  } catch (e) {
    console.log(e)
  }
})
```
Где:
- `layout` - ключ принимающий значения шаблона `'empty'`.

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/>

## Таблица всех представлений

Представления в своём результате - это страницы отрисовки. Таблица всех представлений:
|   Блок   | Представление |                              Cтраница                             | 
| :------: | :-----------: | :---------------------------------------------------------------: |
|     -    |     index     |                         Страница "Главная"                        |
|     -    |    courses    |                         Страница "Курсы"                          |
|     -    |      add      |                     Страница "Добавить курс"                      |
|     -    |    profile    |                         Страница "Профиль"                        |
|     -    |     card      |                         Страница "Корзина"                        |
|     -    |    orders     |                         Страница "Заказы"                         |
|   auth   |     login     |               Страница входа в аккаунт пользователя               |
|   auth   |   password    |             Страница ввода нового пароля пользователя             |
|   auth   |     reset     |                Страница сброса пароля пользователя                |
|     -    |      404      |                           Страница 404                            |
|     -    |    course     |                       Страница одного курса                       |
|     -    |  course-edit  |                   Страница "Редактировать курс"                   |

⬆️ [К оглавлению раздела](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/>

## Шаблоны

Как было сказано в разделе [Структура шаблонизации](#структура-шаблонизации) шаблоны - структуры. Различные структуры определяют различную визуализацию разных блоков кода. 
Таблица всех шаблонов, которые используются в приложении:
|    Шаблон     |                         Предназначение слоя                         | 
| :-----------: | :-----------------------------------------------------------------: |
|     empty     |             Шаблон разметки страницы вывода одного курса            |
|     main      | Шаблон разметки для всех страниц кроме страницы вывода одного курса |

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/>

### Шаблон "empty"
Шаблон `empty` состоит из слоёв `head`, `body` и `footer`. Подробно о слоям описано в разделе [Слои](#cлои), а сам код блока выглядит так:
```handlebars
{{> head }}
  <body>
  {{{ body }}}
  {{> footer}}
  </body>
</html>
```
Где:
- `{{> head }}` - интеграция слоя с названием `head.hbs`. Подробно о слое `head` описано в подразделе [Слой "head"](#слой-head).
- `<body></body>` - HTML тег, который предназначен для хранения контента страницы HTML.
- `{{{ body }}}` - интеграция слоя со слоями различных страниц.
- `{{> footer}}` - интеграция слоя с названием `footer.hbs`. Подробно о слое `footer` описано в подразделе [Слой "footer"](#слой-footer).

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

### Шаблон "main"
Шаблон `main` состоит из слоёв `head`, `navbar`, `body` и `footer`. Подробно о слоям описано в разделе [Слои](#cлои), а сам код блока выглядит так:
```handlebars
{{> head }}
  <body>
    {{> navbar}}
    <div class="container">
      {{{ body }}}
    </div>
  {{> footer}}
  </body>
</html>
```
Где:
- `{{> head }}` - интеграция слоя с названием `head.hbs`. Подробно о слое `head` описано в подразделе [Слой "head"](#слой-head).
- `<body></body>` - HTML тег, который предназначен для хранения контента страницы HTML.
- `{{> navbar}}` - интеграция слоя с названием `navbar.hbs`. Подробно о слое `navbar` описано в подразделе [Слой "navbar"](#слой-footer).
- `<div class="container"></div>` - HTML тег с классом `сontainer`, который выделяет код слоя `body` в отдельный контейнер в структуре DOM-дерева.
- `{{{ body }}}` - интеграция слоя со слоями различных страниц.
- `{{> footer}}` - интеграция слоя с названием `footer.hbs`. Подробно о слое `footer` описано в подразделе [Слой "footer"](#слой-footer).

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

## Cлои

Шаблонизатор `Handlebars` поддерживает интеграцию файлов в структуры кода других файлов, тем самым позволяет избежать коппирование кода. Интегрированные файлы называются слоями шаблона или `partials`. В слои выделяются как обязательные структуры для правильного формирования DOM-дерева так и необязательные по структуре отрисовки страниц HTML, но являются одинаковыми в ряде страниц отрисовки. 

Таблица всех слоём, которые применяются в шаблонах:
|     Слои      |         Зона участка        | 
| :-----------: | :-------------------------: |
|     head      | Отвечает за header разметки |
|    navbar     | Отвечает за navbar разметки |
|    footer     | Отвечает за footer разметки |


⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/>

### Слой "head"

Cлой `head`

Блок кода:
```handlebars
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/css/materialize.min.css">
  <link rel="stylesheet" href="/index.css">
  <title>{{title}}</title>
</head>
```
Где:
- `` -
- `` -
- `` -

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

### Слой "navbar"

Cлой `navbar`

Блок кода:
```handlebars
<nav>
  <div class="nav-wrapper">
    <a href="/" class="brand-logo">Магазин курсов</a>
    <ul id="nav-mobile" class="right hide-on-med-and-down">

      {{#if isHome}}
        <li class="active"><a href="/">Главная</a></li>
      {{else}}
        <li><a href="/">Главная</a></li>
      {{/if}}

      {{#if isCourses}}
        <li class="active"><a href="/courses">Курсы</a></li>
      {{else}}
        <li><a href="/courses">Курсы</a></li>
      {{/if}}

      {{#if isAuth}}
          {{#if isAdd}}
              <li class="active"><a href="/add">Добавить курс</a></li>
          {{else}}
              <li><a href="/add">Добавить курс</a></li>
          {{/if}}


          {{#if isProfile}}
              <li class="active"><a href="/profile">Профиль</a></li>
          {{else}}
              <li><a href="/profile">Профиль</a></li>
          {{/if}}
          {{#if isCard}}
              <li class="active"><a href="/card">Корзина</a></li>
          {{else}}
              <li><a href="/card">Корзина</a></li>
          {{/if}}

          {{#if isOrder}} 

              <li class="active"><a href="/orders">Заказы</a></li>
          {{else}}
              <li><a href="/orders">Заказы</a></li>
          {{/if}}

            <li><a href="/auth/logout">Выйти</a></li>
        {{else}}
          {{#if isLogin}}
              <li class="active"><a href="/auth/login#login">Войти</a></li>
          {{else}}
              <li><a href="/auth/login#login">Войти</a></li>
          {{/if}}
        {{/if}}
    </ul>
  </div>
</nav>
```
Где:
- `<div></div>` - `class="nav-wrapper"`
- `<a href="/" class="brand-logo">Магазин курсов</a>` -
- `<ul></ul>` - `id="nav-mobile"`, ` class="right hide-on-med-and-down"`
- `{{#if}}` - 
  - `isHome` - флаг-переключатель активной страницы `Главная` в слое `navbar`. Описание логики ниже.
  - `isCourses` - флаг-переключатель активной страницы `Курсы` в слое `navbar`. Логика аналогична коду блока `isHome`.
  - `isAuth` -
  - `isAdd` -
  - `isProfile` -
  - `isCard` -
  - `isOrder` -
  - `isLogin` -
- `{{else}}` - 
- `{{/if}}`


Код флага-переключателя `isHome` выглядит так:
  ```handlebars
   {{#if isHome}}
        <li class="active"><a href="/">Главная</a></li>
      {{else}}
        <li><a href="/">Главная</a></li>
      {{/if}}
  ```
Где: 
- `{{#if isHome}}` - 
- `<li></li>` - `class="active"`
- `<a href="/">Главная</a>`
  - `"/"` - 
  - `Главная` - 

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

### Слой "footer" 

```handlebars
<script src="https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/js/materialize.min.js"></script>
<script src="/app.js"></script>
```
Где:
- `https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/js/materialize.min.js` - 
- `/app.js` -


⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>


## Основные страницы

Описать боди.

Таблица представлений основных страниц:
| Представление |            Страница приложения            | 
| :-----------: | :---------------------------------------: |
|     index     |              Страница "Главная"           |
|    courses    |              Страница "Курсы"             |
|      add      |          Страница "Добавить курс"         |
|     card      |             Страница "Корзина"            |
|    orders     |              Страница "Заказы"            |
|    profile    |              Страница "Профиль"           |
|     login     |   Страница входа в аккаунт пользователя   |
|   password    | Страница ввода нового пароля пользователя |
|     reset     |    Страница сброса пароля пользователя    |


⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/>

### Главная

Полный блок кода:
```handlebars
<h1>Добро пожаловать</h1>

<p>Здесь только лучшие курсы</p>
```
Где:
- `<h1>Добро пожаловать</h1>` - 

Конечный вид страницы "Главная":


⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

### Курсы

Полный блок кода:
```handlebars
<h1>Курсы</h1>

{{#if courses.length}}
{{#each courses}}
<div class="row">
  <div class="col s6 offset-s3">
    <div class="card">
      <div class="card-image">
        <img src="{{img}}" alt="{{title}}">
      </div>
      <div class="card-content">
        <span class="card-title">{{title}}</span>
        <p class="price">{{price}}</p>
      </div>
      <div class="card-action actions">
        <a href="/courses/{{id}}" target="_blank">Открыть курс</a>
        {{#if @root.isAuth}}

        {{#ifeq userId._id @root.userId}}
          <a href="/courses/{{id}}/edit?allow=true">Редактировать</a>
        {{/ifeq}}

        <form action="/card/add" method="POST">
          <input type="hidden" name="_csrf" value="{{@root.csrf}}">
          <input type="hidden" name="id" value="{{id}}">
          <button type="submit" class="btn btn-primary">Купить</button>
        </form>
        {{/if}}
      </div>
    </div>
  </div>
</div>
{{/each}}
{{else}}
<p>Курсов пока нет</p>
{{/if}}
```
Где:
- `<h1>Курсы</h1>` - 
- `<div class="row"></div>` - 
- `<div class="col s6 offset-s3"></div>` - 
- `<div class="card"></div>` - 
- `<div class="card-image"></div>` - 
- `<img src="" alt="">` - `{{img}}`, `{{title}}`
- `<div class="card-content"></div>` - 
- `<span class="card-title"></span>` - `{{title}}`
- `<p class="price"></p>` - `{{price}}`
- `<div class="card-action actions"></div>>`
- `<a href="/courses/{{id}}" target="_blank">Открыть курс</a>` - `/courses/{{id}}`, `_blank`, `Открыть курс`
  - `/courses/{{id}}` - 
  - `_blank` -
  - `Открыть курс` - 
- `{{#if}}` - `@root.isAuth` -
- `{{/if}}` - 
- `{{#ifeq userId._id @root.userId}}` -
  -  `userId._id` -
  -  `@root.userId` -
- `<a href="/courses/{{id}}/edit?allow=true">Редактировать</a>` - 
  - `/courses/{{id}}/edit?allow=true` -
  - `Редактировать` -
- `<form action="/card/add" method="POST">` - 
  - `action=""` - `/card/add`  
  - ` method=""` - `POST`
- `<input type="hidden" name="_csrf" value="{{@root.csrf}}">`
  - `type` - `hidden`
  - `name` - `_csrf`
  - `value` - `{{@root.csrf}}`
- `<input type="hidden" name="id" value="{{id}}">` - 
  - `name` - `id`
  - `value` - `{{id}}` 
- `<button type="submit" class="btn btn-primary">Купить</button>` - 
  - `type` - `submit`
  - `class` - `btn btn-primary`
  - `Купить`

*Код функции ifeq:*

```node
module.exports = {
    ifeq(a, b, options) {
        if (a == b) {
            return options.fn(this)
        }
        return options.inverse(this)
    }
}
```
Где: 
- `ifeq(a, b, options) {}` - 
- `if (a == b) {}` 
  - `options.fn(this)`
  - `options.inverse(this)` -

Конечный вид страницы "Курсы":

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

### Добавить курс

Полный код блока:
```handlebars
<h1>Добавить новый курс</h1>

{{#if error}}
<p class="alert">{{error}}</p>
{{/if}}

<form action="/add" method="POST">
  <div class="input-field">
    <input id="title" name="title" type="text" class="validate" required value="{{data.title}}">
    <label for="title">Название курса</label>
    <span class="helper-text" data-error="Введите название"></span>
  </div>

  <div class="input-field">
    <input id="price" name="price" type="number" class="validate" required min="1" value="{{data.price}}">
    <label for="price">Цена курса</label>
    <span class="helper-text" data-error="Введите цену"></span>
  </div>

  <div class="input-field">
    <input id="img" name="img" type="text" class="validate" required value="{{data.img}}">
    <label for="img">URL картинки</label>
    <span class="helper-text" data-error="Введите url картинки"></span>
  </div>

  <input type="hidden" name="_csrf" value="{{csrf}}">

  <button class="btn btn-primary">Добавить курс</button>
</form>
```
Где: 
- `<h1>Добавить новый курс</h1>` - 
- `{{#if error}}` -
  - `<p class="alert">{{error}}</p>` - 
- `{{/if}}` -
- `<form action="/add" method="POST"></form>` -
- `<div class="input-field"></div>` -
- `<input id="title" name="title" type="text" class="validate" required value="{{data.title}}">` -
  - `id` - `title`
  - `name` - `title`
  - `type` - `text`
  - `class` - `validate`
- `required value="{{data.title}}">` -
- `<input type="hidden" name="_csrf" value="{{csrf}}">` -
  - `type` - `hidden`
  - `name` - `_csrf` 
  - `value` - `{{csrf}}`
- `<button class="btn btn-primary">Добавить курс</button>` -
  - `class="btn btn-primary"` - 
  - `Добавить курс` - 


Конечный вид страницы "Добавить курс":

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

### Профиль

Полный код блока:
```handlebars
<h1>Профиль</h1>

<div class="row">
  <div class="col s6">
    {{#if user.avatarUrl}}
      <img 
        class="avatar"
        src="{{user.avatarUrl}}" 
        alt="avatar-{{user.name}}"
      >
    {{else}}
      <p>Аватара нет</p>
    {{/if}}
  </div>

  <div class="col s6">
    <form action="/profile" method="POST" enctype="multipart/form-data">
      <p>Ваше email: <strong>{{user.email}}</strong></p>

      <div class="input-field">
        <input id="name" name="name" type="text" class="validate" required value="{{user.name}}">
        <label for="name">Ваше имя</label>
        <span class="helper-text" data-error="Имя не может быть пустым"></span>
      </div>

      <div class="file-field input-field">
        <div class="btn">
          <span>Аватар</span>
          <input type="file" name="avatar">
        </div>
        <div class="file-path-wrapper">
          <input class="file-path validate" type="text">
        </div>
      </div>

      <input type="hidden" name="_csrf" value="{{csrf}}">

      <button type="submit" class="btn">Изменить</button>
    </form>
  </div>
</div>
```
Где:
- `<h1>Профиль</h1>` - 
- `<div class="row"></div>` -
- `<form action="/profile" method="POST" enctype="multipart/form-data">` -
  - `action` - `/profile`
  - `method` - `POST`
  - `enctype` - `multipart/form-data`
- `<p>Ваше email: <strong>{{user.email}}</strong></p>` - 
  - `{{user.email}}` -
- `<div class="input-field"></div>` - 
- `<input id="name" name="name" type="text" class="validate" required value="{{user.name}}">` -
  - `id` - `name`
  - `name` - `name`
  - `type` - `type`
  - `class` - `validate`
  - `required value="{{user.name}}"` - `{{user.name}}`
- `<label for="name">Ваше имя</label>` -
- `for` - `name`
- `<span class="helper-text" data-error="Имя не может быть пустым"></span>` -
  - `class` - `helper-text`
  - `data-error` - `"Имя не может быть пустым"`
- `<div class="file-field input-field"></div>` -
- `<div class="btn"></div>` -
- `<span>Аватар</span>` -
- `<input type="file" name="avatar">` -
  - `type` - `file`
  - `name` - `avatar`
- `<div class="file-path-wrapper"></div>` -
- `<input class="file-path validate" type="text">` -
  - `class` - `"file-path validate`
  - `type` - `"text"`
- `<input type="hidden" name="_csrf" value="{{csrf}}">` - 
  - `type` - `hidden`
  - `name` - `_csrf`
  - `value` - `{{csrf}}`
- `<button type="submit" class="btn">Изменить</button>` -
  - `type` - `submit`
  - `class` - `btn`
  - `Изменить` -


Конечный вид страницы "Профиль":

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

### Корзина

Полный блок кода:
```handlebars
<h1>Корзина</h1>

<div id="card">
{{#if courses.length}}
  <table>
    <thead>
      <tr>
        <th>Название</th>
        <th>Количество</th>
        <th>Действия</th>
      </tr>
    </thead>
    <tbody>
      
      {{#each courses}}
      <tr>
        <td>{{title}}</td>
        <td>{{count}}</td>
        <td>
          <button 
          class="btn btm-small js-remove" 
          data-id="{{id}}"
          data-csrf="{{@root.csrf}}"
          
          >Удалить</button>
        </td>
      </tr>
      {{/each}}
    </tbody>
  </table>

  <p><strong>Цена:</strong> <span class="price">{{price}}</span></p>

  <form action="/orders" method="POST">
    <input type="hidden" name="_csrf" value="{{csrf}}">
    <button type="submit" class="btn">Сделать заказ</button>
  </form>
{{else}}
  <p>Корзина пуста</p>
{{/if}}
</div>
```
Где:
- `<h1>Корзина</h1>` - 
- `<div id="card"></div>` -
- `{{#if courses.length}}` -
- `<table></table>` -
- `<thead></thead>` -
- `<tr></tr>` -
- `<th>Название</th>` - 
- `<th>Количество</th>` -
- `<th>Действия</th>` -
- `{{#each courses}}` -
- `<td>{{title}}</td>` -
- `<td>{{count}}</td>` - 
- `<button class="btn btm-small js-remove" data-id="{{id}}" data-csrf="{{@root.csrf}}">Удалить</button>` -
  - `class` - `"btn btm-small js-remove"`
  - `data-id` - `{{id}}`
  - `data-csrf` - `{{@root.csrf}}`
- `{{/each}}` - 
- `<p><strong>Цена:</strong> <span class="price">{{price}}</span></p>` -
  - `{{price}}` -
- `<form action="/orders" method="POST">` -
  - `action` - `/orders`
  - `method` - `POST`
- `<input type="hidden" name="_csrf" value="{{csrf}}">` -
  - `type` - `hidden`
  - `name` - `_csrf`
  - `value` - `{{csrf}}`
- `<button type="submit" class="btn">Сделать заказ</button>` - 
- `type` - `submit`
- `class` - `btn`
- `Сделать заказ` -
- `{{else}}` -
- `<p>Корзина пуста</p>` - 
- `{{/if}}` - 

Конечный вид страницы "Корзина":

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

### Заказы
Полный код блока: 
```handlebars
<h1>Мои заказы</h1>

{{#if orders.length}}
  {{#each orders}}
    <div class="row">
      <div class="col s6 offset-s3">
        <div class="card">
          <div class="card-content">
            <span class="card-title">
              Заказ <small>{{_id}}</small>
            </span>
            <p class="date">{{date}}</p>
            <p><em>{{user.userId.name}}</em> ({{user.userId.email}})</p>

            <ol>
              {{#each courses}}
                <li>
                  {{course.title}} (x<strong>{{count}}</strong>)
                </li>
              {{/each}}
            </ol>

            <hr>

            <p>Цену: <span class="price">{{price}}</span></p>
          </div>
        </div>
      </div>
    </div>
  {{/each}}
{{else}}
  <p>Заказов пока нет</p>
{{/if}}
```
Где:
- `<h1>Мои заказы</h1>` -
- `{{#if orders.length}}` -
- `{{#each orders}}` -
- `<div class="row"></div>` -
- `<div class="col s6 offset-s3"></div>` -
- `<div class="card"></div>` -
- `<div class="card-content"></div>` -
- `<span class="card-title">Заказ</span>` -
- `<small>{{_id}}</small>` -
- `<p class="date">{{date}}</p>` -
  - `class` - `date`
  - `{{date}}` -
- `<p><em>{{user.userId.name}}</em> ({{user.userId.email}})</p>` -
  - `{{user.userId.name}}` -
  - `{{user.userId.email}}` -
- `<ol></ol>` -
- `{{#each courses}}` -
- `<li></li>` -
- `{{course.title}} (x<strong>{{count}}</strong>)` -
  - `{{course.title}}` -
  - `{{count}}` -
- `{{/each}}` - 
- `<p>Цену: <span class="price">{{price}}</span></p>` -
  - `class` - `price`
  - `{{price}}` -
- `{{else}}` -
- `<p>Заказов пока нет</p>` - 
- `{{/if}}` - 

Конечный вид страницы "Заказы":

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

### Вход / Выход


#### Вход / Регистрация
Полный блок кода:
```handlebars
<div class="auth">
    <div class="row">
        <div class="col s12">
            <ul class="tabs">
                <li class="tab col s6"><a class="active" href="#login">Войти</a></li>
                <li class="tab col s6"><a class="active" href="#register">Регистрация</a></li>
            </ul>
        </div>
        <div id="login" class="col s6 offset-s3">

            {{#if loginError}}
                <p class="alert">{{loginError}}</p>
            {{/if}}

            <h2>Войти в магазин</h2>
            <form action="/auth/login" method="POST">
                <div class="input-field">
                    <input id="email" name="email" type="email" class="validate" required>
                    <label for="email">Email</label>
                    <span class="helper-text" data-error="Введите email"></span>
                </div>

                <div class="input-field">
                    <input id="password" name="password" type="password" class="validate" required>
                    <label for="password">Пароль</label>
                    <span class="helper-text" data-error="Введите пароль"></span>
                </div>

                <input type="hidden" name="_csrf" value="{{csrf}}">

                
                <button class="btn btn-primary" type="submit">Войти</button>
                <p><a href="/auth/reset">Забыли пароль?</a></p>
            </form>
        </div>
        <div id="register" class="col s6 offset-s3">

            {{#if registerError}}
                <p class="alert">{{registerError}}</p>
            {{/if}}

            <h2>Создать аккаунт</h2>
            <form action="/auth/register" method="POST" novalidate>
                <div class="input-field">
                    <input id="remail" name="email" type="email" class="validate" required>
                    <label for="remail">Email</label>
                    <span class="helper-text" data-error="Введите email"></span>
                </div>

                <div class="input-field">
                    <input id="name" name="name" type="text" class="validate" required>
                    <label for="name">Ваше имя</label>
                    <span class="helper-text" data-error="Введите имя"></span>
                </div>

                <div class="input-field">
                    <input id="rpassword" name="password" type="password" class="validate" required>
                    <label for="rpassword">Пароль</label>
                    <span class="helper-text" data-error="Введите пароль"></span>
                </div>

                <div class="input-field">
                    <input id="confirm" name="confirm" type="password" class="validate" required>
                    <label for="confirm">Пароль ещё раз</label>
                    <span class="helper-text" data-error="Введите пароль"></span>
                </div>

                <input type="hidden" name="_csrf" value="{{csrf}}">

                <button class="btn btn-primary" type="submit">Зарегистрироваться</button>
            </form>
        </div>
    </div>
</div>
```
Где:
*1. Раздел "Логин"*
  - `<li class="tab col s6"><a class="active" href="#login">Войти</a></li>` - 
  - `<li class="tab col s6"><a class="active" href="#register">Регистрация</a></li>` -
  - `{{#if loginError}}` -
  - `<p class="alert">{{loginError}}</p>` -
  - `{{/if}}` -
  - `<h2>Войти в магазин</h2>` -
  - `<form action="/auth/login" method="POST">` -
  - `<div class="input-field"></div>` -
  - `<input id="email" name="email" type="email" class="validate" required>` -
  - `<label for="email">Email</label>` -
  - `<span class="helper-text" data-error="Введите email"></span>` -
  - `<div class="input-field"></div>` -
  - `<input id="password" name="password" type="password" class="validate" required>` -
  - `<label for="password">Пароль</label>` -
  - `<span class="helper-text" data-error="Введите пароль"></span>` - 
  - `<input type="hidden" name="_csrf" value="{{csrf}}">` -
  - `<button class="btn btn-primary" type="submit">Войти</button>` -
  - `<p><a href="/auth/reset">Забыли пароль?</a></p>` - 
*2. Раздел "Регистрация"*
  - `{{#if registerError}}` -
  - `<p class="alert">{{registerError}}</p>` -
  - `{{/if}}` -
  - `<h2>Создать аккаунт</h2>` -
  - `<form action="/auth/register" method="POST" novalidate>` -
  - `<div class="input-field"></div>` -
  - `<input id="remail" name="email" type="email" class="validate" required>` -
  - `<label for="remail">Email</label>` -
  - `<span class="helper-text" data-error="Введите email"></span>` -
  - `<input type="hidden" name="_csrf" value="{{csrf}}">` -
  - `<button class="btn btn-primary" type="submit">Зарегистрироваться</button>` -

Конечный вид страницы "Логин":

Конечный вид страницы "Регистрация":

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

#### Сброс пароля
Полный код блока:
```handlebars
<div class="row">
    <div class="col s6 offset-s3">
        {{#if error}}
            <p class="alert">{{error}}</p>
        {{/if}}
        <h2>Забыли пароль?</h2>
        <form action="/auth/reset" method="POST">
            <div class="input-field">
                <input id="email" name="email" type="email" class="validate" required>
                <label for="email">Email</label>
                <span class="helper-text" data-error="Введите email"></span>
            </div>

            <input type="hidden" name="_csrf" value="{{csrf}}">
            <button class="btn btn-primary" type="submit">Cбросить</button>
        </form>
    </div>
</div>
```
Где:
- `{{#if error}}` -
- `<p class="alert">{{error}}</p>` -
- `{{/if}}` -
- `<h2>Забыли пароль?</h2>` -
- `<div class="input-field"></div>` -
- `<input id="email" name="email" type="email" class="validate" required>` -
- `<label for="email">Email</label>` -
- `<span class="helper-text" data-error="Введите email"></span>` -
- `<input type="hidden" name="_csrf" value="{{csrf}}">` -
- `<button class="btn btn-primary" type="submit">Cбросить</button>` -

Конечный вид страницы "Сброс пароля":

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

#### Смена пароля
Полный код блока:
```handlebars
<div class="row">
    <div class="col s6 offset-s3">
        {{#if error}}
            <p class="alert">{{error}}</p>
        {{/if}}
        <h2>Задать новый пароль</h2>
        <form action="/auth/password" method="POST">
            <div class="input-field">
                <input id="password" name="password" type="password" class="validate" required>
                <label for="password">Новый пароль</label>
                <span class="helper-text" data-error="Введите пароль"></span>
            </div>

            <input type="hidden" name="_csrf" value="{{csrf}}">
            <input type="hidden" name="userId" value="{{userId}}">
            <input type="hidden" name="token" value="{{token}}">
            <button class="btn btn-primary" type="submit">Обновить пароль</button>
        </form>
    </div>
</div>
```
Где:
- `{{#if error}}` -
- `<p class="alert">{{error}}</p>` -
- `{{/if}}` -
- `<h2>Задать новый пароль</h2>` -
- `<form action="/auth/password" method="POST"></form>` -
- `<div class="input-field"></div>` -
- `<input id="password" name="password" type="password" class="validate" required>` -
- `<label for="password">Новый пароль</label>` -
- `<span class="helper-text" data-error="Введите пароль"></span>` -
- `<input type="hidden" name="_csrf" value="{{csrf}}">` -
- `<input type="hidden" name="userId" value="{{userId}}">` -
- `<input type="hidden" name="token" value="{{token}}">` -
- `<button class="btn btn-primary" type="submit">Обновить пароль</button>` -

Конечный вид страницы "Cмена пароля":

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>
 
## Дополнительные страницы

Таблица представлений дополнительных страниц:
| Представление |        Страница приложения     | 
| :-----------: | :----------------------------: |
|    course     |      Страница одного курса     |
|  course-edit  | Страница "Редактировать курс"  |
|      404      |          Страница 404          |


↩️ [К оглавлению документации](../README.md) <br/>
⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

### Страница одного курса
Полный код блока:
```handlebars
<div class="course">
  <h1>{{course.title}}</h1>

  <img src="{{course.img}}" alt="{{course.title}}">

  <p class="price big">{{course.price}}</p>
</div>
```
Где:
- `<h1>{{course.title}}</h1>` -
- `<img src="{{course.img}}" alt="{{course.title}}">` -
- `<p class="price big">{{course.price}}</p>` -

Конечный вид страницы одного курса:

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

### Редактировать курс
Полный блок кода:
```handlebars
<h1>Редактировать {{course.title}}</h1>

<form action="/courses/edit" method="POST" class="course-form">
  <div class="input-field">
    <input id="title" name="title" type="text" class="validate" required value="{{course.title}}">
    <label for="title">Название курса</label>
    <span class="helper-text" data-error="Введите название"></span>
  </div>

  <div class="input-field">
    <input id="price" name="price" type="number" class="validate" required min="1"  value="{{course.price}}">
    <label for="price">Цена курса</label>
    <span class="helper-text" data-error="Введите цену"></span>
  </div>

  <div class="input-field">
    <input id="img" name="img" type="text" class="validate" required  value="{{course.img}}">
    <label for="img">URL картинки</label>
    <span class="helper-text" data-error="Введите url картинки"></span>
  </div>

  <input type="hidden" name="id" value="{{course.id}}">
  <input type="hidden" name="_csrf" value="{{csrf}}">

  <button type="submit" class="btn btn-primary">Редактировать курс</button>
</form>

<form action="/courses/remove" method="POST">
  <input type="hidden" name="_csrf" value="{{csrf}}">
  <input type="hidden" name="id" value="{{ course.id }}">
  <button class="btn red">Удалить курс</button>
</form>
```
Где:
- `<h1>Редактировать {{course.title}}</h1>` -
- `<form action="/courses/edit" method="POST" class="course-form"></form>` -
- `<div class="input-field"></div>` -
- `<input id="title" name="title" type="text" class="validate" required value="{{course.title}}">` -
- `<label for="title">Название курса</label>` -
- `<span class="helper-text" data-error="Введите название"></span>` -
- `<input id="price" name="price" type="number" class="validate" required min="1"  value="{{course.price}}">` -
- `<label for="price">Цена курса</label>` -
- `<span class="helper-text" data-error="Введите цену"></span>` -
- `<input id="img" name="img" type="text" class="validate" required  value="{{course.img}}">` -
- `<label for="img">URL картинки</label>` -
- `<span class="helper-text" data-error="Введите url картинки"></span>` -
- `<input type="hidden" name="id" value="{{course.id}}">` -
- `<input type="hidden" name="_csrf" value="{{csrf}}">` -
- `<button type="submit" class="btn btn-primary">Редактировать курс</button>` -
- `<form action="/courses/remove" method="POST"></form>` -
- `<input type="hidden" name="_csrf" value="{{csrf}}">` -
- `<input type="hidden" name="id" value="{{ course.id }}">` -
- `<button class="btn red">Удалить курс</button>` -

Конечный вид страницы "Редактировать курс":

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

### Страница 404
Полный блок кода:
```handlebars
<h1>404 Ошибка</h1>
<p>Страница не найдена</p>
<hr>

<a href="/">Вернуться на главную страницу</a>
```
Где:
- `<h1>404 Ошибка</h1>` -
- `<p>Страница не найдена</p>` -
- `<a href="/">Вернуться на главную страницу</a>` -

Конечный вид страницы "Страница 404":

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>



















