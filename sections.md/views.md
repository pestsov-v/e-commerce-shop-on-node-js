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

Порядок действий:
**1. Подключение шаблонизатора в главный файл `index.js`:**
Код, отвечающий за подключение базовых параметров `Handlebars` в главный файл `index.js`:
```node
const exphbs = require('express-handlebars')

const hbs = exphbs.create({
  defaultLayout: 'main',
  extname: 'hbs',
  helpers: require('./utils/hbs-helpers')
})
```
Где:
- `exphbs.create({})` - метод `create` объекта `exphbs`, который принимает список аргументов для инициализации базовых параметров шаблонизатора `Handlebars`.
- `defaultLayout` - обязательный аргумент. Определяет слой по умолчанию `'main'`. 
- `extname` - обязательный аргумент. Указывает на расширение файлов.
- `helpers` - необязательный параметр. Может быть как строкой, в случае одного промежуточного обработчика для `Handlebars` так и объектом, в случае передачи нескольких промежуточних обработчиков (`middleware`).

Код, отвечающий за инициализацию шаблонизатора:
```node
app.engine('hbs', hbs.engine)
app.set('view engine', 'hbs')
app.set('views', 'views')
```
Где:
- `app.engine('hbs', hbs.engine)` - метод `engine` экземпляра `app` фреймворка `express` принимающий объект подключения базовых параметров `hbs` и метод запуска `hbs.engine`.
- `app.set('view engine', 'hbs')` - метод `set` экземпляра `app` фреймворка `express` определяющий запуск шаблонизатора `hbs`. 
- `app.set('views', 'views')` - метод `set` экземпляра `app` фреймворка `express` определяющий корневую папку представлений `views`.

**2. Создание шаблонов:**
Шаблон это подоснова или структура. Разработчик, исходя из целей формирует определённую визуализацию - конкретный шаблон или же использовать общий шаблон для нескольких визуализаций. Для удобства создают представления подобной структуры. Учитывая, что `Handlebars` позволяет определить шаблон по умолчанию, то мы успешном реализовыванием две задачи в одной. В главной файле `index.js` определяем шаблон по умолчанию:
```node
...
const hbs = exphbs.create({
  defaultLayout: 'main',
  ...
})
...
```
Где:
- `exphbs.create({})` - метод `create` объекта `exphbs`, который принимает список аргументов для инициализации базовых параметров шаблонизатора `Handlebars`.
- `defaultLayout` - ключ принимающий значение шаблона по умолчанию `main`.

**3. Интеграция шаблонов в систему маршрутов:**

При определении маршрутизации разных страниц приложения (необходимо понимать, что под разными страницами подразумевается разными маршруты оцифровки разных кусков кода, само же приложение является одностраничным) в маршрутах прописывается функция рендера или визуализации. Интеграцию шаблонизатора на примере маршрутизатора "add":
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

Следует отметить, что поскольку в передаваемых, локальных данных, мы не передаём ключ `layout` зарезервированный `handlebars`, то шаблонизатор для отрисовки страницы `add` берёт шаблон по умолчанию - `main`, который мы подключали в главном файле `index.js` выше. В случае, если бы не было шаблона по умолчанию, движок `Handlebars` вывел бы ошибку с требованием предоставить шаблон визуализации. Шаблоны могут быть также индивидуальными, как в случае шаблона одного курса.
Код страницы одного курса:
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

Полный Блок кода слоя `head`:
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
- `{{title}}` - данные `title` приходящие вместе с данными шаблона в функции контроллера. 

Код, определяющий передачу данных контроллером в шаблонизатор страницы `index.hbs`:
```node
router.get('/', (req, res) => {
  res.render('index', {
    title: 'Главная страница',
    isHome: true
  })
})
```
Где:
- `title` -данные `title` о которых говорилось выше.

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

### Слой "navbar"

Полный блок кода слоя `navbar`:
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
- `<div>class="nav-wrapper"</div>` - контейнер с классом `nav-wrapper`.
- `<a href="/" class="brand-logo">Магазин курсов</a>` - ссылка ведущая на главную страницу сайта, где тело ссылки `Магазин курсов`.
- `<ul id="nav-mobile" class="right hide-on-med-and-down">` - список с ID `id="nav-mobile"` и классом `class="right hide-on-med-and-down"`.
- `{{#if}}` - *возможность шаблонизатора `Handlebars`*. Условие, выполнение которого срабатывает на флаг-переключатель активной страницы.
  - `isHome` - *возможность шаблонизатора `Handlebars`*. Флаг-переключатель активной страницы `Главная` в слое `navbar`. Описание логики ниже.
  - `isCourses` - *возможность шаблонизатора `Handlebars`*. Флаг-переключатель активной страницы `Курсы` в слое `navbar`. Логика аналогична коду блока `isHome`.
  - `isAuth` - Аналогично `isHome`.
  - `isAdd` - Аналогично `isHome`.
  - `isProfile` - Аналогично `isHome`.
  - `isCard` - Аналогично `isHome`.
  - `isOrder` - Аналогично `isHome`.
  - `isLogin` - Аналогично `isHome`.
- `{{else}}` - *возможность шаблонизатора `Handlebars`*. Условие, которое срабатывает на флаг-переключатель.
- `{{/if}}` - *возможность шаблонизатора `Handlebars`*. Закрытие тега условия `if`.


Код флага-переключателя `isHome` выглядит так:
  ```handlebars
   {{#if isHome}}
        <li class="active"><a href="/">Главная</a></li>
      {{else}}
        <li><a href="/">Главная</a></li>
      {{/if}}
  ```
Где: 
- `{{#if isHome}}` - *возможность шаблонизатора `Handlebars`*. Условие, тело которого срабатывает при активной ссылке `isHome`.
- `<li>class="active"</li>` - класс, отвечающий за активность ссылки.
- `<a href="/">Главная</a>` - ссылка на страницу.
  - `"/"` - путь машрутизатора ссылки.
  - `Главная` - название ссылки на страницу.

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

### Слой "footer" 
Полный блок кода слоя `footer`.
```handlebars
<script src="https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/js/materialize.min.js"></script>
<script src="/app.js"></script>
```
Где:
- `https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/js/materialize.min.js` - подключение `CDN` фреймворка `materialize`.
- `/app.js` - подключение скриптов для стилизации контейнеров классов и т.д.


⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>


## Основные страницы

Структура сайта наличивает несколько представлений, которые переключаются посредством вызова кода контроллеров действиями пользователя. 

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

Полный блок кода шаблона `index.hbs`:
```handlebars
<h1>Добро пожаловать</h1>

<p>Здесь только лучшие курсы</p>
```
Где:
- `<h1>Добро пожаловать</h1>` - заголовок страницы.

Конечный вид представления "Главная":


⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

### Курсы

Полный блок кода шаблона `сourses.hbs`:
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
- `<h1>Курсы</h1>` - заголовок страницы "Курсы".
- `<div class="row"></div>` - класс `row` определяющий строку в структуре фреймворка `Materialize CSS`.
- `<div class="col s6 offset-s3"></div>` - класс `col s6 offset-s3` определяющий ширину строки, которая занимает 6 колонок `s6` и расстояние от краёв страницы `s3` в структуре карточки фреймворка `Materialize CSS`. Подробности особенности подобных колонок читайте информацию про адаптивные сетки. 
- `<div class="card"></div>` - класс `"card"` определяющий одну карточку в структуре фреймворка `Materialize CSS`.
- `<div class="card-image"></div>` - класс `"card-image"` определяющий картинку карточки. 
- `<img src="{{img}}" alt="{{title}}">` - данные отправленные контроллером для визуализации в шаблоне `courses.hbs`.
- `<div class="card-content"></div>` - класс `"card-content"` определяющий информативное наполнение карточки.
- `<span class="card-title">{{title}}</span>` - класс `"card-title"` определяющий формат заголовка `title`. Под понятием "формат" имеется в виду визуализированный вид данных, который покрывается стилями CSS.
- `<p class="price"></p>` - класс `"price"` определяющий формат цены `price`. Под понятием "формат" имеется в виду визуализированный вид данных, который покрывается стилями CSS.
- `<div class="card-action actions"></div>>` - класс `class="card-action actions"` определяющий действие, в случае нажатия на ссылку.
- `<a href="/courses/{{id}}" target="_blank">Открыть курс</a>` - ссылка, которая перенаправляет на страницу одного курса
  - `/courses/{{id}}` - *возможность шаблонизатора `Handlebars`*. Маршрут страницы одного курса. 
- `{{#if}} @root.isAuth` - *возможность шаблонизатора `Handlebars`*. Условие проверки авторизации пользователя `@root.isAuth` при которое, `isAuth === true` выполняется тело условия. 
- `{{/if}}` - *возможность шаблонизатора `Handlebars`*. Закрытие условия.
- `{{#ifeq userId._id @root.userId}}` - *возможность шаблонизатора `Handlebars`*. Промежуточный обработчик `ifeq` передающийся в базовых параметрах `hbs` в главном файле `index.js`. Код промежуточного обработчика указан ниже.
  -  `userId._id` - ID пользователя который хранится в базе данных `MongoDB`.
  -  `@root.userId` - ID пользователя, который авторизировался на сайте.
- `<a href="/courses/{{id}}/edit?allow=true">Редактировать</a>` - ссылка на редактирование данных одного курса.
  - `/courses/{{id}}/edit?allow=true` - URL-путь редактирование данных одного курса.
- `<form action="/card/add" method="POST">` - *возможность шаблонизатора `Handlebars`*. Тип запроса `post` по маршруту `'card/add'`, который выполняет функции контроллера добавление одного курса в корзину. Подробнее о данном контроллере смотрите [Тип запроса `post` по маршруту `'/card/add'`](controllers.md/#тип-запроса-post-по-маршруту-cardadd).
- `<input type="hidden" name="_csrf" value="{{@root.csrf}}">` - *возможность шаблонизатора `Handlebars`*. Cкрытый обработчик на права доступа только авторизированому пользователю`{{@root.csrf}}`.
- `<input type="hidden" name="id" value="{{id}}">` - *возможность шаблонизатора `Handlebars`*. Cкрытый обработчик значения `{{id}}`.
- `<button type="submit" class="btn btn-primary">Купить</button>` - Кнопка выполняющая код контроллера запроса `post` по маршруту `'card/add'`.

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
- `ifeq(a, b, options) {}` - функция `ifeq` принимающая два параметра.
  - `a` - параметр, принимающий в примере выше `userId._id`.
  - `b` - параметр, принимающий в примере выше `@root.userId`.
  - `options` - передаваемые функция, метод, данные и т.д.
- `if (a == b) {}`- условие сравнения ID пользователя в базе данных с ID пользователем авторизированным на сайте.
  - `options.fn(this)` - свойство `fn` объекта `options` передающий контекст функции-обработчика. В примере выше это возможность редактирование данных одного курса.
  - `options.inverse(this)` - свойство `inverse` объекта `options` убирающий контекст функции-обработчика. В примере выше это возможность редактирование данных одного курса.

Конечный вид представления "Курсы":

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

### Добавить курс

Полный блок кода шаблона `add.hbs`:
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
- `<h1>Добавить новый курс</h1>` - заголовок страницы "Добавить курс".
- `{{#if error}}` - условие, при котором выполняется текст ошибки.
  - `<p class="alert">{{error}}</p>` - параграф с классом `alert` с текстом ошибки `{{error}}`.
- `{{/if}}` - закрытие блока сравнения.
- `<form action="/add" method="POST"></form>` - *возможность шаблонизатора `Handlebars`*. Тип запроса `post` по маршруту `'/add/'`, который выполняет функции контроллера добавление одного курса в базу данных. Подробнее о данном контроллере смотрите [Тип запроса `post` по маршруту `'/add'`](controllers.md/#тип-запроса-post-по-маршруту-add)
- `<div class="input-field"></div>` - контейнер с классом `iput-field`, который принимает `input` с названием курса. 
- `<input id="title" name="title" type="text" class="validate" required value="{{data.title}}">` - `input` определяющий ID `id="title"` поля ввода, его название `name="title"`, тип `type="text"`, класс `class="validate"` и обязательные атрибуты данных `required value="{{data.title}}`.
- `<input type="hidden" name="_csrf" value="{{csrf}}">` - *возможность шаблонизатора `Handlebars`*. Cкрытый обработчик на права доступа только авторизированому пользователю`{{@root.csrf}}`.
- `<button class="btn btn-primary">Добавить курс</button>` - Кнопка выполняющая код контроллера запроса `post` по маршруту `'/add'`.

Конечный вид представления "Добавить курс":

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

### Профиль

Полный код блока шаблона `profile.hbs`:
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
- `<h1>Профиль</h1>` - Заголовок страницы "Профиль".
- `<div class="row"></div>` - контейнер с классом `row` определяющий строку в структуре фреймворка `Materialize CSS`.
- `<form action="/profile" method="POST" enctype="multipart/form-data">` - *возможность шаблонизатора `Handlebars`*. Тип запроса `post` по маршруту `'/profile'`, который выполняет функции контроллера добавление данных пользователя в его профиль. Подробнее о данном контроллере смотрите [Тип запроса `post` по маршруту `'/profile'`](controllers.md/#тип-запроса-post-по-маршруту-add).
  - `enctype="multipart/form-data"` - cвойство которое нужно добавить в HTML форму для корректной работы модуля `multer`.
- `<p>Ваше email: <strong>{{user.email}}</strong></p>` - параграф с текстом почты пользователя.
- `<div class="input-field"></div>` - контейнер с классом `iput-field`, который принимает `input` с названием курса. 
- `<input id="name" name="name" type="text" class="validate" required value="{{user.name}}">` - `input` определяющий ID `id="name"` поля ввода, его название `name="name"`, тип `type="text"`, класс `class="validate"` и обязательные атрибуты данных `required value="{{user.name}}`.
- `<label for="name">Ваше имя</label>` - Подсказывающий текст `input` определяющийся для `input` с именем `name`.
- `<span class="helper-text" data-error="Имя не может быть пустым"></span>` - текстовый контейнер с классом `class="helper-text"` который всплывает в случае ошибки и выводит текст ошибки `data-error="Имя не может быть пустым"`.
- `<div class="btn"></div>` - контейнер с классом `class="btn"` содержащий кнопку.
- `<span>Аватар</span>` - текстовый контейнер хранящий текст `Аватар`.
- `<input type="file" name="avatar">` - `input` принимающий тип `type="file"` с названием `name="avatar"`.
- `<div class="file-path-wrapper"></div>` - контейнер с классом `class="file-path-wrapper"`. 
- `<input class="file-path validate" type="text">` - `input` принимающий тип пути файла `type="file-path validate"` с названием `name="text"`.
- `<input type="hidden" name="_csrf" value="{{csrf}}">` - *возможность шаблонизатора `Handlebars`*. Cкрытый обработчик на права доступа только авторизированому пользователю`{{@root.csrf}}`.
- `<button type="submit" class="btn">Изменить</button>` - кнопка выполняющая код контроллера запроса `post` по маршруту `'/profile'`.

Конечный вид представления "Профиль":

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

### Корзина

Полный блок кода шаблона `card.hbs`:
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
- `<h1>Корзина</h1>` - заголовок страницы "Корзина".
- `<div id="card"></div>` - контейнер с ID `id="card"`.
- `{{#if courses.length}}` - условие, которое выполняется при условии, если есть хотя бы один курс `courses.length`
- `<table></table>` - тег таблицы.
- `<thead></thead>` - тег заголовка таблицы.
- `<tr></tr>` - тег одной строчки таблицы.
- `<th>Название</th>` - тег заголовка колонки таблицы "Название".
- `<th>Количество</th>` - тег заголовка колонки таблицы "Количество".
- `<th>Действия</th>` - тег заголовка колонки таблицы "Действия".
- `{{#each courses}}` - условие, которое выполняется для каждого курса в массиве курсов `courses`.
- `<td>{{title}}</td>` - название курса.
- `<td>{{count}}</td>` - количество единиц одного курса.
- `<button class="btn btm-small js-remove" data-id="{{id}}" data-csrf="{{@root.csrf}}">Удалить</button>` - кнопка удаляющая один курс по его ID `data-id="{{id}}"` и с дополнительной проверкой на предмет авторизации пользователя `data-csrf="{{@root.csrf}}`.
- `{{/each}}` - *возможность шаблонизатора `Handlebars`*. Закрытие условия `each`.
- `<p><strong>Цена:</strong> <span class="price">{{price}}</span></p>` - текстовый контейнер выводящий итоговую стоимость `{{price}}`.
- `<form action="/orders" method="POST">` - *возможность шаблонизатора `Handlebars`*. Тип запроса `post` по маршруту `'/orders'`, который выполняет функции контроллера добавление одного заказа. Подробнее о данном контроллере смотрите [Тип запроса `post` по маршруту `'/orders'`](controllers.md/#тип-запроса-post-по-маршруту-cardadd).
- `<input type="hidden" name="_csrf" value="{{csrf}}">` - *возможность шаблонизатора `Handlebars`*. Cкрытый обработчик на права доступа только авторизированому пользователю`{{@root.csrf}}`.
- `<button type="submit" class="btn">Сделать заказ</button>` - кнопка выполняющая код контроллера запроса `post` по маршруту `'/orders'`.

Конечный вид представления "Корзина":

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

### Заказы
Полный код блока шаблона `orders.hbs`: 
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
- `<h1>Мои заказы</h1>` - Заголовок страницы "Заказы".
- `{{#if orders.length}}` - условие, которое выполняется при условии, если есть хотя бы один заказ `orders.length}`
- `{{#each orders}}` - условие, которое выполняется для каждого заказа в массиве заказов `orders`.
- `<div class="row"></div>` - контейнер с классом `row` определяющий строку.
- `<div class="col s6 offset-s3"></div>` - контейнер с классом `col s6 offset-s3` определяющий ширину колонки `col s6` и отступ от краёв адаптивной сетки `offset-s3` в структуре фреймворка `Materialize CSS`.
- `<div class="card"></div>` - контейнер с классом `card` определяющий карточку.
- `<div class="card-content"></div>` - контейнер с классом `card-content` определяющий наполнение карточки.
- `<span class="card-title">Заказ</span>` - текстовый контейнер с классом `class="card-title"` определяющий заголовок, текст которого `Заказ`.
- `<small>{{_id}}</small>` - уникальный ID заказа.
- `<p class="date">{{date}}</p>` - параграф с классом `class="date"` определяющий дату выдачи заказа.
- `<p><em>{{user.userId.name}}</em> ({{user.userId.email}})</p>` - параграф определяющий имя пользователя и его почту в скобках.
- `{{#each courses}}` -  условие, которое выполняется для каждого заказа в массиве курсов `courses`.
- `<li></li>` - нумерованый список курсов.
- `{{course.title}} (x<strong>{{count}}</strong>)` - выводит название курса `{{course.title}}` и в скобках количество единиц этого курса `{{count}}`.
- `<p>Цену: <span class="price">{{price}}</span></p>` - текстовый параграф, который выводит итоговую стоимость всех курсов `{{price}}`.

Конечный вид представления "Заказы":

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

### Вход / Выход


#### Вход / Регистрация
Полный блок кода шаблона `auth/login.hbs`:
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
  - `<li class="tab col s6"><a class="active" href="#login">Войти</a></li>` -  элемент списка с классом `class="tab col s6"` занимающий половину адаптивной разметки и содержащий ссылку на графу `Войти` - `href="#login"`.
  - `<li class="tab col s6"><a class="active" href="#register">Регистрация</a></li>` -  элемент списка с классом `class="tab col s6"` занимающий половину адаптивной разметки и содержащий ссылку на графу `Регистрация` - `href="#login"`.
  - `{{#if loginError}}` - условие, которое выполняется при ошибке ввода логика `loginError`.
  - `<form action="/auth/login" method="POST">` - *возможность шаблонизатора `Handlebars`*. Тип запроса `post` по маршруту `'/auth/login'`, который выполняет функции контроллера ввода данных пользователя для авторизации на сайте. Подробнее о данном контроллере смотрите [Тип запроса `post` по маршруту `'/auth/login'`](controllers.md/#тип-запроса-post-по-маршруту-authlogin).
  - `<input id="email" name="email" type="email" class="validate" required>` - `input` определяющий ID `id="email"` поля ввода, его название `name="email"`, тип `type="email"`, класс `class="validate"` и обязательным атрибутом `required`, который выдаст ошибку в случае, если поле `email` будет пустым.
  - `<label for="email">Email</label>` - подсказывающий текст `input` определяющийся для `input` с именем `Email`.
  - `<span class="helper-text" data-error="Введите email"></span>` - текстовый контейнер с классом `class="helper-text"` который всплывает в случае ошибки и выводит текст ошибки `data-error="Введите email"`.
  - `<input id="password" name="password" type="password" class="validate" required>` - `input` определяющий ID `id="password"` поля ввода, его название `name="password"`, тип `type="password"`, класс `class="validate"` и обязательным атрибутом `required`, который выдаст ошибку в случае, если поле `password` будет пустым.
  - `<label for="password">Пароль</label>` - подсказывающий текст `input` определяющийся для `input` с именем `password`.
  - `<span class="helper-text" data-error="Введите пароль"></span>` - текстовый контейнер с классом `class="helper-text"` который всплывает в случае ошибки и выводит текст ошибки `data-error="Введите пароль"`.
  - `<input type="hidden" name="_csrf" value="{{csrf}}">` - *возможность шаблонизатора `Handlebars`*. Cкрытый обработчик на права доступа только авторизированому пользователю`{{@root.csrf}}`.
  - `<button class="btn btn-primary" type="submit">Войти</button>` - кнопка выполняющая код контроллера запроса `post` по маршруту `'/auth/login'`.
  - `<p><a href="/auth/reset">Забыли пароль?</a></p>` - ccылка введующая на страницу "Сброса пароля".
  - 
*2. Раздел "Регистрация"*
  - `{{#if registerError}}` - условие, которое выполняется при ошибке ввода логика `registerError`
  - `<form action="/auth/register" method="POST" novalidate>` - *возможность шаблонизатора `Handlebars`*. Тип запроса `post` по маршруту `'/auth/register'`, который выполняет функции контроллера добавление данных пользователя в базу данных. Подробнее о данном контроллере смотрите [Тип запроса `post` по маршруту `'/auth/register'`](controllers.md/#тип-запроса-post-по-маршруту-authregister).

Конечный вид страницы "Логин":

Конечный вид страницы "Регистрация":

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

#### Сброс пароля
Полный код блока `password.hbs`:
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
- `{{#if error}}` - условие, которое выполняется при ошибке ввода логика `error`.
- `<form action="/auth/reset" method="POST"`> - *возможность шаблонизатора `Handlebars`*. Тип запроса `post` по маршруту `'/auth/login'`, который выполняет функции контроллера cброса пароля. Подробнее о данном контроллере смотрите [Тип запроса `post` по маршруту `'/auth/reset'`](controllers.md/#тип-запроса-post-по-маршруту-authreset).
- `<input id="email" name="email" type="email" class="validate" required>` - `input` определяющий ID `id="email"` поля ввода, его название `name="email"`, тип `type="email"`, класс `class="validate"` и обязательным атрибутом `required`, который выдаст ошибку в случае, если поле `email` будет пустым.
- `<label for="email">Email</label>` - подсказывающий текст `input` определяющийся для `input` с именем `Email`.
- `<span class="helper-text" data-error="Введите email"></span>` - текстовый контейнер с классом `class="helper-text"` который всплывает в случае ошибки и выводит текст ошибки `data-error="Введите email"`.
- `<input type="hidden" name="_csrf" value="{{csrf}}">` - *возможность шаблонизатора `Handlebars`*. Cкрытый обработчик на права доступа только авторизированому пользователю`{{@root.csrf}}`.
- `<button class="btn btn-primary" type="submit">Cбросить</button>` - кнопка выполняющая код контроллера запроса `post` по маршруту `'/auth/reset'`.

Конечный вид страницы "Сброс пароля":

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

#### Смена пароля
Полный код блока шаблона `reset.hbs`:
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
- `{{#if error}}` - условие, которое выполняется при ошибке ввода логика `error`.
- `<form action="/auth/password" method="POST"></form>` - *возможность шаблонизатора `Handlebars`*. Тип запроса `post` по маршруту `'/auth/password'`, который выполняет функции контроллера cмены пароля. Подробнее о данном контроллере смотрите [Тип запроса `post` по маршруту `'/auth/password'`](controllers.md/#тип-запроса-post-по-маршруту-authpassword).
- `<input id="password" name="password" type="password" class="validate" required>` - `input` определяющий ID `id="password"` поля ввода, его название `name="password"`, тип `type="password"`, класс `class="validate"` и обязательным атрибутом `required`, который выдаст ошибку в случае, если поле `password` будет пустым.
- `<label for="password">Новый пароль</label>` - подсказывающий текст `input` определяющийся для `input` с именем `password`.
- `<span class="helper-text" data-error="Введите пароль"></span>` - текстовый контейнер с классом `class="helper-text"` который всплывает в случае ошибки и выводит текст ошибки `data-error="Введите пароль"`.
- `<input type="hidden" name="_csrf" value="{{csrf}}">` - *возможность шаблонизатора `Handlebars`*. Cкрытый обработчик на права доступа только авторизированому пользователю`{{@root.csrf}}`.
- `<input type="hidden" name="userId" value="{{userId}}">` - *возможность шаблонизатора `Handlebars`*. Cкрытый обработчик на проверку ID пользователя`{{userId}}`.
- `<input type="hidden" name="token" value="{{token}}">` - *возможность шаблонизатора `Handlebars`*. Cкрытый обработчик токена пользователя `{{token}}`.
- `<button class="btn btn-primary" type="submit">Обновить пароль</button>` - кнопка выполняющая код контроллера запроса `post` по маршруту `'/auth/password'`

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
Полный код блока шаблона `courses.hbs`:
```handlebars
<div class="course">
  <h1>{{course.title}}</h1>

  <img src="{{course.img}}" alt="{{course.title}}">

  <p class="price big">{{course.price}}</p>
</div>
```
Где:
- `<h1>{{course.title}}</h1>` - заголовок с названием курса `{{course.title}}`
- `<img src="{{course.img}}" alt="{{course.title}}">` - картинка курса `{{course.img}}` с названием заголовка курса `{{course.title}}`.
- `<p class="price big">{{course.price}}</p>` - параграф с классом `class="price big"` показывающий цену курса `{{course.price}}`.

Конечный вид страницы одного курса:

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

### Редактировать курс
Полный блок кода шаблона `course-edit.hbs`:
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
- `<h1>Редактировать {{course.title}}</h1>` - заголовок редактирование одного курса.
- `<form action="/courses/edit" method="POST" class="course-form"></form>` -  *возможность шаблонизатора `Handlebars`*. Тип запроса `post` по маршруту `'/courses/edit'`, который выполняет функции контроллера cмены пароля. Подробнее о данном контроллере смотрите [Тип запроса `post` по маршруту `'/auth/password'`](controllers.md/#тип-запроса-get-по-маршруту-coursesidedit).
- `<div class="input-field"></div>` - контейнер с классом `iput-field`, который принимает `input` с названием курса. 
- `<input id="title" name="title" type="text" class="validate" required value="{{course.title}}">` - `input` определяющий ID `id="title"` поля ввода, его название `name="title"`, тип `type="text"`, класс `class="validate"` и обязательным атрибутом `required`, который должен наличивать значение заголовка курса `{{course.title}}`.
- `<label for="title">Название курса</label>` - подсказывающий текст `Название курса` определяющийся для `input` с именем `title`.
- `<span class="helper-text" data-error="Введите название"></span>` - текстовый контейнер с классом `class="helper-text"` который всплывает в случае ошибки и выводит текст ошибки `data-error="Введите название"`.
- `<input id="price" name="price" type="number" class="validate" required min="1" value="{{course.price}}">` - `input` определяющий ID `id="price"` поля ввода, его название `name="price"`, тип `type="number"`, класс `class="validate"` и обязательным атрибутом `required`, который должен наличивать значение заголовка курса `{{course.title}}` и быть в количестве не меньше 1 `min="1"`.
- `<input type="hidden" name="id" value="{{course.id}}">` - *возможность шаблонизатора `Handlebars`*. Cкрытый обработчик на проверку ID курса `{{@root.csrf}}`.
- `<input type="hidden" name="_csrf" value="{{csrf}}">` - *возможность шаблонизатора `Handlebars`*. Cкрытый обработчик на права доступа только авторизированому пользователю `{{@root.csrf}}`.
- `<button type="submit" class="btn btn-primary">Редактировать курс</button>` - кнопка выполняющая код контроллера запроса `post` по маршруту `'/courses/edit'`.
- `<form action="/courses/remove" method="POST"></form>` - *возможность шаблонизатора `Handlebars`*. Тип запроса `post` по маршруту `'/courses/edit'`, который выполняет функции контроллера cмены пароля. Подробнее о данном контроллере смотрите [Тип запроса `post` по маршруту `'/auth/password'`](controllers.md/#тип-запроса-post-по-маршруту-coursesremove).

Конечный вид страницы "Редактировать курс":

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

### Страница 404
Полный блок кода шаблона `404.hbs`:
```handlebars
<h1>404 Ошибка</h1>
<p>Страница не найдена</p>
<hr>

<a href="/">Вернуться на главную страницу</a>
```
Где:
- `<h1>404 Ошибка</h1>` - заголовок страницы `404`.
- `<p>Страница не найдена</p>` - Параграф с текстом `Страница не найдена`.
- `<a href="/">Вернуться на главную страницу</a>` - ccылкав ведущая на главную страницу сайта.

Конечный вид страницы "Страница 404":

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>
