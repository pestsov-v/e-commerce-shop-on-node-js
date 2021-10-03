# Представления

Представления организованы подключением шаблонизатора Handlebars, который обрабатывает и виззуализирует данные, которые предоставляются в файлах hbs. Данные, которые отрисовывает шаблонизатор управляются контроллерами, а маршруты по которым шаблоны визуализируются управляются маршрутизатором.

⬅️ Подробную информацию обо всех маршрутах смотрите в разделе [Маршрутизация](routes.md). <br/>
⬅️ Подробную информацию обо всех контроллерах смотрите в разделе [Контроллеры](controllers.md). <br/>
↩️ [К оглавлению документации](../README.md). <br/> 

## Оглавление раздела

---

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


---


## Структура шаблонизации

Процедура подключения шаблонизатора:

**1. Подключение шаблонизатора в главный файл index.js:** <br/>

Подключение базовых параметров Handlebars в главный файл index.js: <br/>
```node
const exphbs = require('express-handlebars')

const hbs = exphbs.create({
  defaultLayout: 'main',
  extname: 'hbs',
  helpers: require('./utils/hbs-helpers')
})
...
```
Где: <br/>
- `exphbs.create({})` - метод create объекта exphbs, который принимает список аргументов для инициализации базовых параметров шаблонизатора Handlebars. <br/>
- `defaultLayout` - обязательный аргумент. Определяет слой по умолчанию 'main'. <br/>
- `extname` - обязательный аргумент. Указывает на расширение файлов. <br/>
- `helpers` - необязательный параметр. Может быть как строкой, в случае одного промежуточного обработчика для Handlebars так и объектом, в случае передачи нескольких промежуточних обработчиков. <br/>

Инициализация шаблонизатора: <br/>
```node
...
app.engine('hbs', hbs.engine)
app.set('view engine', 'hbs')
app.set('views', 'views')
...
```
Где: <br/>
- `app.engine('hbs', hbs.engine)` - метод engine экземпляра app фреймворка express принимающий объект подключения базовых параметров hbs и метод запуска hbs.engine. <br/>
- `app.set('view engine', 'hbs')` - метод set экземпляра app фреймворка express определяющий запуск шаблонизатора hbs. <br/>
- `app.set('views', 'views')` - метод set экземпляра app фреймворка express определяющий корневую папку представлений views. <br/>

**2. Создание шаблонов:** <br/>

Шаблон - это подоснова или структура. Разработчик, исходя из целей формирует определённую визуализацию - конкретный шаблон или же использует общий шаблон для нескольких визуализаций. Для удобства создаются представления со схожей структурой. Учитывая, что Handlebars позволяет определить шаблон по умолчанию, а это в свою очередь позволяет решить две задачи одним действием. В главной файле index.js определяем шаблон по умолчанию: <br/>
```node
...
const hbs = exphbs.create({
  defaultLayout: 'main'
  ...
})
...
```
Где: <br/>
- `exphbs.create({})` - метод create объекта exphbs, который принимает список аргументов для инициализации базовых параметров шаблонизатора Handlebars. <br/>
- `defaultLayout` - ключ принимающий значение шаблона по умолчанию main. <br/>

**3. Интеграция шаблонов в систему маршрутов:** <br/>

При определении маршрутизации разных страниц приложения в маршрутах прописывается функция визуализации. Интеграция шаблонизатора на примере маршрутизатора "add": <br/>
```node
router.get('/', auth, (req, res) => {
  res.render('add', {
    title: 'Добавить курс',
    isAdd: true
  })
})
```
Где: <br/>
- `router.get('/', auth, (req, res) => {})` - маршрутизатор с типом запросом get, который перенаправляет на страницу '/add'. <br/>
  - `auth` - промежуточный обработчик авторизации. Подробнее о данном промежуточном обработчике смотрите [auth](middleware.md##auth)<br/>
  - `req` - объект запроса. <br/>
  - `res` - объект ответа. <br/>
- `res.render('add', {})` - функция визуализации render объекта ответа res, которая принимает три аргумента: <br/>
    - `add` - обязательный параметр. Определяет код визуализации. В нашем случае код визуализации - страница 'add' шаблонизатора Handlebars. Код визуализации может быть также строкой. <br/>
    - `{}` - необязательный параметр. Объект с локальными данными, которые передаються объектом res в шаблон 'add'. <br/>
    - `function () {}` - необязательный параметр. Callback-функция. <br/>
- `title` - ключ принимающий значение 'Добавить курс'. <br/>
- `isAdd` - флаг-переключатель. По умолчанию флаг имеет значение: true. <br/>

Следует отметить, что поскольку в передаваемых, локальных данных, мы не передаём ключ layout зарезервированный handlebars, то шаблонизатор для отрисовки страницы 'add' берёт шаблон по умолчанию - main, который был подключён в главном файле index.js выше. В случае, если бы шаблон по умолчанию отсутствовал, движок Handlebars вывел бы ошибку с требованием предоставить шаблон визуализации. Шаблоны могут быть также индивидуальными, как в случае шаблона одного курса: <br/>
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
Где:  <br/>
- `layout` - ключ принимающий значения шаблона 'empty'. <br/>

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела). <br/>
↩️ [К оглавлению документации](../README.md). <br/>

---

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

⬆️ [К оглавлению раздела](#оглавление-раздела). <br/>
↩️ [К оглавлению документации](../README.md). <br/>

---

## Шаблоны

Шаблоны - это структуры. Различные структуры определяют различную визуализацию разных блоков кода. <br/>

Таблица всех шаблонов, которые используются в приложении: <br/>
|    Шаблон     |                         Предназначение слоя                         | 
| :-----------: | :-----------------------------------------------------------------: |
|     empty     |             Шаблон разметки страницы вывода одного курса            |
|     main      | Шаблон разметки для всех страниц кроме страницы вывода одного курса |

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела). <br/>
↩️ [К оглавлению документации](../README.md). <br/>

---

### Шаблон "empty"

Шаблон empty состоит из слоёв head, body и footer. Подробно о слоям описано в разделе [Слои](#cлои), а сам код блока выглядит так: <br/>
```handlebars
{{> head }}
  <body>
  {{{ body }}}
  {{> footer}}
  </body>
</html>
```
Где: <br/>
- `{{> head }}` - интеграция слоя с названием head.hbs. Подробно о слое head описано в подразделе [Слой "head"](#слой-head). <br/>
- `{{{ body }}}` - интеграция слоя со слоями различных страниц. <br/>
- `{{> footer}}` - интеграция слоя с названием footer.hbs. Подробно о слое footer описано в подразделе [Слой "footer"](#слой-footer). <br/>

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела). <br/>

---

### Шаблон "main"

Шаблон main состоит из слоёв head, navbar, body и footer. Подробно о слоях описано в разделе [Слои](#cлои), а сам код блока выглядит так: <br/>
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
Где: <br/>
- `{{> head }}` - интеграция слоя с названием head.hbs. Подробно о слое head описано в подразделе [Слой "head"](#слой-head). <br/>
- `<body></body>` - тег контента. <br/>
- `{{> navbar}}` - интеграция слоя с названием navbar.hbs. Подробно о слое navbar описано в подразделе [Слой "navbar"](#слой-footer). <br/>
- `<div class="container"></div>` - контейнер с классом сontainer, который выделяет код слоя body в отдельный контейнер в структуре DOM-дерева. <br/>
- `{{{ body }}}` - интеграция слоя со слоями различных страниц. <br/>
- `{{> footer}}` - интеграция слоя с названием footer.hbs. Подробно о слое footer описано в подразделе [Слой "footer"](#слой-footer). <br/>

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела). <br/>
↩️ [К оглавлению документации](../README.md). <br/>

## Cлои

---

Шаблонизатор Handlebars поддерживает интеграцию файлов в структуры кода других файлов, тем самым позволяет избежать коппирование кода. Интегрированные файлы называются слоями шаблона или partials. В слои могут выносится как обязательные структуры для правильного формирования DOM-дерева так и необязательные, по структуре отрисовки страниц HTML, но являются одинаковыми в ряде страниц отрисовки. <br/>

Таблица всех слоёв, которые применяются в шаблонах:
|     Слои      |         Зона участка        | 
| :-----------: | :-------------------------: |
|     head      | Отвечает за header разметки |
|    navbar     | Отвечает за navbar разметки |
|    footer     | Отвечает за footer разметки |


⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела). <br/>
↩️ [К оглавлению документации](../README.md). <br/>

---

### Слой "head"

Код слоя head: <br/>
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
Где: <br/>
- `{{title}}` - данные заголовка title приходящие вместе с данными шаблона, в функции контроллера. <br/>

Передачу данных контроллером в шаблонизатор страницы index.hbs: <br/>
```node
router.get('/', (req, res) => {
  res.render('index', {
    title: 'Главная страница',
    isHome: true
  })
})
```
Где: <br/>
- `title` -данные заголовка title о которых говорилось выше. <br/>

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела). <br/>
↩️ [К оглавлению документации](../README.md). <br/>

---

### Слой "navbar"

Код слоя navbar: <br/>
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
Где: <br/>
- `<a href="/" class="brand-logo">Магазин курсов</a>` - ссылка ведущая на главную страницу сайта. <br/>
- `<ul id="nav-mobile" class="right hide-on-med-and-down">` - список с ID id="nav-mobile" и классом class="right hide-on-med-and-down". <br/>
- `{{#if}}` - *возможность шаблонизатора Handlebars*. Условие, выполнение которого срабатывает на флаг-переключатель активной страницы. <br/>
  - `isHome` - *возможность шаблонизатора Handlebars*. Флаг-переключатель активной страницы Главная в слое navbar. Описание логики ниже. <br/>
  - `isCourses` - *возможность шаблонизатора Handlebars*. Флаг-переключатель активной страницы Курсы в слое navbar. Логика аналогична коду блока isHome. <br/>
- `{{else}}` - *возможность шаблонизатора Handlebars*. Условие срабатывающее если условие if == false. <br/>
- `{{/if}}` - *возможность шаблонизатора Handlebars*. Закрытие тега условия if. <br/>

Флаг-переключатель isHome: <br/>
  ```handlebars
   {{#if isHome}}
        <li class="active"><a href="/">Главная</a></li>
      {{else}}
        <li><a href="/">Главная</a></li>
      {{/if}}
  ```
Где: <br/>
- `{{#if isHome}}` - *возможность шаблонизатора Handlebars*. Условие, тело которого срабатывает при активности флага isHome. <br/>
- `<li>class="active"</li>` - класс, отвечающий за активность ссылки. <br/>
- `<a href="/">Главная</a>` - ссылка на страницу "Главная". <br/>

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела). <br/>
↩️ [К оглавлению документации](../README.md). <br/>

---

### Слой "footer" 

Код слоя footer: <br/>
```handlebars
<script src="https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/js/materialize.min.js"></script>
<script src="/app.js"></script>
```
Где: <br/>
- `https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/js/materialize.min.js` - подключение CDN фреймворка materialize. <br/>
- `/app.js` - подключение скриптов для стилизации контейнеров, классов и т.п. <br/> 

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела). <br/>
↩️ [К оглавлению документации](../README.md). <br/>

---

## Основные страницы

Структура сайта имеет несколько представлений, которые переключаются посредством вызова кода контроллеров действиями пользователя. <br/>

Таблица представлений основных страниц: <br/>
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

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела). <br/>
↩️ [К оглавлению документации](../README.md). <br/>

---

### Главная

Код шаблона index.hbs: <br/>
```handlebars
<h1>Добро пожаловать</h1>

<p>Здесь только лучшие курсы</p>
```
Где:
- `<h1>Добро пожаловать</h1>` - заголовок страницы.

Итоговый вид представления "Главная": <br/>

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела). <br/>
↩️ [К оглавлению документации](../README.md). <br/>

---

### Курсы

Код шаблона сourses.hbs: <br/>
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
Где: <br/>
- `<img src="{{img}}" alt="{{title}}">` - данные отправленные контроллером для визуализации в шаблоне курсов courses.hbs. <br/>
- `<span class="card-title">{{title}}</span>` - текстовый контейнер с классом "card-title" содержащий формат заголовка title. <br/>
- `<div class="card-action actions"></div>>` - контейнер с классом class="card-action actions" содержащий действие, в случае нажатия на ссылку.<br/>
- `<a href="/courses/{{id}}" target="_blank">Открыть курс</a>` - ссылка, которая перенаправляет на страницу одного курса. <br/>
  - `/courses/{{id}}` - *возможность шаблонизатора Handlebars*. Маршрут страницы одного курса. <br/>
- `{{#if}} @root.isAuth` - *возможность шаблонизатора Handlebars*. Условие проверки авторизации пользователя @root.isAuth при которой, isAuth === true выполняется тело условия. 
- `{{/if}}` - *возможность шаблонизатора Handlebars*. Закрытие условия. <br/>
- `{{#ifeq userId._id @root.userId}}` - *возможность шаблонизатора Handlebars*. Промежуточный обработчик ifeq передающийся в базовых аргументах hbs в главном файле index.js. Код промежуточного обработчика указан ниже. <br/>
  -  `userId._id` - ID пользователя который хранится в базе данных MongoDB. <br/>
  -  `@root.userId` - ID пользователя, который авторизировался на сайте. <br/>
- `<a href="/courses/{{id}}/edit?allow=true">Редактировать</a>` - ссылка на редактирование данных одного курса. <br/>
  - `/courses/{{id}}/edit?allow=true` - URL-путь редактирование данных одного курса. <br/>
- `<form action="/card/add" method="POST">` - *возможность шаблонизатора Handlebars*. Метод запроса post по маршруту 'card/add', который выполняет функции контроллера добавление одного курса в корзину. <br/>
- `<input type="hidden" name="_csrf" value="{{@root.csrf}}">` - *возможность шаблонизатора Handlebars*. Cкрытый обработчик прав доступа авторизации {{@root.csrf}}. <br/>
- `<input type="hidden" name="id" value="{{id}}">` - *возможность шаблонизатора Handlebars*. Cкрытый обработчик значения {{id}}. <br/>

⬅️ Подробнее о контроллере, который используется в шаблоне "Курсы" смотрите [Метод запроса post по маршруту '/card/add'](controllers.md/#метод-запроса-post-по-маршруту-cardadd).

*Функция ifeq:* <br/>

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
Где: <br/>
- `ifeq(a, b, options) {}` - функция ifeq принимающая два аргумента: <br/>
  - `a` - параметр, принимающий в примере выше userId._id. <br/>
  - `b` - параметр, принимающий в примере выше @root.userId. <br/>
  - `options` - передаваемые функция, метод, данные и т.д. <br/>
- `if (a == b) {}`- условие сравнения ID пользователя из базы данных с ID пользователем авторизированным на сайте. <br/>
  - `options.fn(this)` - свойство fn объекта options передающий контекст функции-обработчика. В примере выше - это возможность редактирование данных одного курса. <br/>
  - `options.inverse(this)` - свойство inverse объекта options убирающий контекст функции-обработчика. В примере выше - это возможность редактирование данных одного курса. <br/>

Итоговый вид представления "Курсы": <br/>

[![2.png](https://i.postimg.cc/BbhRX9wv/2.png)](https://postimg.cc/hQd25wnF)

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела). <br/>
↩️ [К оглавлению документации](../README.md). <br/>

---

### Добавить курс

Код шаблона add.hbs: <br/>
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
Где: <br/>
- `{{#if error}}` - условие, при котором выполняется текст ошибки. <br/>
  - `<p class="alert">{{error}}</p>` - параграф с классом alert с текстом ошибки {{error}}. <br/>
- `{{/if}}` - *возможность шаблонизатора Handlebars*. Закрытие условия сравнения. <br/>
- `<form action="/add" method="POST"></form>` - *возможность шаблонизатора Handlebars*. Метод запроса post по маршруту '/add/', который выполняет функции контроллера добавление одного курса в базу данных. <br/>
- `<input id="title" name="title" type="text" class="validate" required value="{{data.title}}">` - input содержащий ID id="title" поля ввода, его название name="title", тип type="text", класс class="validate" и обязательный атрибут данных required value="{{data.title}}. <br/>
- `<input type="hidden" name="_csrf" value="{{csrf}}">` - *возможность шаблонизатора Handlebars*. Cкрытый обработчик прав доступа авторизации {{@root.csrf}}. <br/>

⬅️ Подробнее о контроллере, который используется в шаблоне "Добавить курс" смотрите [Метод запроса post по маршруту '/add'](controllers.md/#метод-запроса-post-по-маршруту-add). <br/>

Итоговый вид представления "Добавить курс": <br/>

[![3.png](https://i.postimg.cc/MTqL5KCT/3.png)](https://postimg.cc/zL2pX5XZ)

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

---

### Профиль

Код шаблона profile.hbs: <br/>
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
Где: <br/>
- `<form action="/profile" method="POST" enctype="multipart/form-data">` - *возможность шаблонизатора Handlebars*. Метод запроса post по маршруту '/profile', который выполняет функции контроллера добавление данных пользователя в его профиль. 
  - `enctype="multipart/form-data"` - cвойство которое добавляется в HTML форму для корректной работы модуля multer. <br/>
- `<p>Ваше email: <strong>{{user.email}}</strong></p>` - параграф с текстом почты пользователя. <br/>
- `<input id="name" name="name" type="text" class="validate" required value="{{user.name}}">` - input содержащий ID id="name" поля ввода, его название name="name", тип type="text", класс class="validate" и обязательный атрибут данных required value="{{user.name}}. <br/>
- `<input type="hidden" name="_csrf" value="{{csrf}}">` - *возможность шаблонизатора Handlebars*. Cкрытый обработчик прав доступа авторизации {{@root.csrf}}. <br/>

⬅️ Подробнее о контроллере, который используется в шаблоне "Профиль" смотрите [Метод запроса post по маршруту '/profile'](controllers.md/#метод-запроса-post-по-маршруту-add). <br/>

 вид представления "Профиль": <br/>

[![4.png](https://i.postimg.cc/vZnSdVMT/4.png)](https://postimg.cc/K1Z5P41h)

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела). <br/>

---

### Корзина

Код шаблона card.hbs: <br/>
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
Где: <br/>
- `{{#if courses.length}}` - условие, которое выводит все курсы из массива курсов courses. <br/>
- `{{#each courses}}` - условие, которое выполняется для каждого курса из массива курсов courses. <br/>
- `<button class="btn btm-small js-remove" data-id="{{id}}" data-csrf="{{@root.csrf}}">Удалить</button>` - кнопка удаляющая один курс по его ID data-id="{{id}}" с проверкой авторизации пользователя data-csrf="{{@root.csrf}}. <br/>
- `{{/each}}` - *возможность шаблонизатора Handlebars*. Закрытие условия each. <br/>
- `<p><strong>Цена:</strong> <span class="price">{{price}}</span></p>` - текстовый контейнер выводящий итоговую стоимость {{price}}. <br/>
- `<form action="/orders" method="POST">` - *возможность шаблонизатора Handlebars*. Метод запроса post по маршруту '/orders', который выполняет функции контроллера добавление одного заказа. <br/>
- `<input type="hidden" name="_csrf" value="{{csrf}}">` - *возможность шаблонизатора Handlebars*. Cкрытый обработчик прав доступа авторизации {{@root.csrf}}. <br/>

Итоговый вид представления "Корзина": <br/>


⬅️ Подробнее о контроллере, который используется в шаблоне "Корзина" смотрите [Метод запроса post по маршруту '/orders'](controllers.md/#метод-запроса-post-по-маршруту-cardadd). <br/>
⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>

---

### Заказы

Код шаблона orders.hbs: <br/>
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
Где: <br/>
- `<h1>Мои заказы</h1>` - заголовок страницы "Заказы". <br/>
- `{{#if orders.length}}` - условие, которое выводит все заказы из массива заказаов courses orders. <br/>
- `{{#each orders}}` - условие, которое выполняется для каждого заказа из массива заказов orders. <br/>
- `<small>{{_id}}</small>` - уникальный ID заказа. <br/>
- `<p class="date">{{date}}</p>` - параграф с классом class="date" содержащий дату выдачи заказа. <br/>
- `<p><em>{{user.userId.name}}</em> ({{user.userId.email}})</p>` - параграф содержащий имя пользователя и его почту в скобках. <br/>
- `{{#each courses}}` - условие, которое выполняется для каждого заказа в массиве курсов courses. <br/>
- `{{course.title}} (x<strong>{{count}}</strong>)` - выводит название курса {{course.title}} и в скобках количество единиц этого курса {{count}}. <br/>
- `<p>Цену: <span class="price">{{price}}</span></p>` - текстовый параграф, который выводит итоговую стоимость всех курсов {{price}}. <br/>

Итоговый вид представления "Заказы": <br/>

[![6.png](https://i.postimg.cc/fy3fjGRy/6.png)](https://postimg.cc/N2cXQPTv)

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела). <br/>
↩️ [К оглавлению документации](../README.md). <br/>

---

### Вход / Выход

Управление логированием пользователя наличивает несколько страниц. Таблица всех представлений логирования: <br/>
| Представление |        Страница приложения     | 
| :-----------: | :----------------------------: |
|    login      |         Страница логина        |
|   password    |     Страница сброса пароля     |
|     reset     |      Страница смены пароля     |

#### Вход / Регистрация

---

Код шаблона auth/login.hbs: <br/>
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
Где: <br/>
*1. Раздел "Логин"* <br/>
- `{{#if loginError}}` - условие, которое выполняется при ошибке ввода логина loginError. <br/>
- `<form action="/auth/login" method="POST">` - *возможность шаблонизатора Handlebars*. Метод запроса post по маршруту '/auth/login', который выполняет функции контроллера ввода данных пользователя для авторизации на сайте. <br/>
- `<input type="hidden" name="_csrf" value="{{csrf}}">` - *возможность шаблонизатора Handlebars*. Cкрытый обработчик прав доступа авторизации {{@root.csrf}}. <br/>
- `<p><a href="/auth/reset">Забыли пароль?</a></p>` - ccылка введующая на страницу "Сброса пароля". <br/>

⬅️ Подробнее о контроллере, который используется в разделе "Логин", шаблона Авторизации смотрите [Метод запроса post по маршруту '/auth/login'](controllers.md/#метод-запроса-post-по-маршруту-authlogin). <br/>

Итоговый вид страницы "Логин": <br/>

[![3.png](https://i.postimg.cc/hjY9j21B/3.png)](https://postimg.cc/ZBr9sFQ7)

*2. Раздел "Регистрация"* <br/>
- `{{#if registerError}}` - условие, которое выполняется при ошибке ввода почты без cимвола "@" - registerError. <br/>
- `<form action="/auth/register" method="POST" novalidate>` - *возможность шаблонизатора Handlebars*. Метод запроса post по маршруту '/auth/register', который выполняет функции контроллера добавление нового пользователя в базу данных. <br/>

⬅️ Подробнее о контроллере, который используется в разделе "Регистрация", шаблона Авторизации смотрите [Метод запроса post по маршруту '/auth/register'](controllers.md/#метод-запроса-post-по-маршруту-authregister). <br/>

Итоговый вид страницы "Регистрация": <br/>

[![4.png](https://i.postimg.cc/KvBs6Tmz/4.png)](https://postimg.cc/JG7qJGDV)

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела). <br/>
↩️ [К оглавлению документации](../README.md). <br/>

---

#### Сброс пароля

Код шаблона auth/password.hbs: <br/>
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
Где: <br/>
- `{{#if error}}` - условие, которое выполняется при ошибке ввода логика error. <br/>
- `<form action="/auth/reset" method="POST"`> - *возможность шаблонизатора Handlebars*. Метод запроса post по маршруту '/auth/login', который выполняет функции контроллера cброса пароля. <br/>
- `<input type="hidden" name="_csrf" value="{{csrf}}">` - *возможность шаблонизатора Handlebars*. Cкрытый обработчик прав доступа авторизации {{@root.csrf}}. <br/>
- `<button class="btn btn-primary" type="submit">Cбросить</button>` - кнопка выполняющая код контроллера запроса post по маршруту '/auth/reset'. <br/>

Итоговый вид страницы "Сброс пароля": <br/>

[![5.png](https://i.postimg.cc/5NBn7MmQ/5.png)](https://postimg.cc/rKp1s33q)

⬅️ Подробнее о контроллере, который используется в шаблоне "Сброс пароля" смотрите [Метод запроса post по маршруту '/auth/reset'](controllers.md/#метод-запроса-post-по-маршруту-authreset). <br/>
⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела). <br/>
↩️ [К оглавлению документации](../README.md). <br/>

---

#### Смена пароля

Код шаблона auth/reset.hbs:
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
Где: <br/>
- `{{#if error}}` - условие, которое выполняется при ошибке ввода логика error. <br/>
- `<form action="/auth/password" method="POST"></form>` - *возможность шаблонизатора Handlebars*. Метод запроса post по маршруту '/auth/password', который выполняет функции контроллера cмены пароля. <br/>
- `<input type="hidden" name="_csrf" value="{{csrf}}">` - *возможность шаблонизатора Handlebars*. Cкрытый обработчик прав доступа авторизации {{@root.csrf}}.<br/>
- `<input type="hidden" name="userId" value="{{userId}}">` - *возможность шаблонизатора Handlebars*. Cкрытый обработчик проверяющий ID пользователя {{userId}}. <br/>
- `<input type="hidden" name="token" value="{{token}}">` - *возможность шаблонизатора Handlebars*. Cкрытый обработчик проверяющий наличие токена пользователя {{token}}. <br/>
- `<button class="btn btn-primary" type="submit">Обновить пароль</button>` - кнопка выполняющая код контроллера запроса post по маршруту '/auth/password'. <br/>



⬅️ Подробнее о контроллере, который используется в шаблоне "Смена пароля" смотрите [Метод запроса `post` по маршруту `'/auth/password'`](controllers.md/#метод-запроса-post-по-маршруту-authpassword). <br/>
⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела). <br/>
↩️ [К оглавлению документации](../README.md). <br/>

---

## Дополнительные страницы

Таблица представлений дополнительных страниц: <br/>
| Представление |        Страница приложения     | 
| :-----------: | :----------------------------: |
|    course     |      Страница одного курса     |
|  course-edit  | Страница "Редактировать курс"  |
|      404      |          Страница 404          |

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела). <br/>
↩️ [К оглавлению документации](../README.md). <br/>

---

### Страница одного курса

Код шаблона courses.hbs: <br/>
```handlebars
<div class="course">
  <h1>{{course.title}}</h1>

  <img src="{{course.img}}" alt="{{course.title}}">

  <p class="price big">{{course.price}}</p>
</div>
```
Где: <br/>
- `<h1>{{course.title}}</h1>` - заголовок с названием курса {{course.title}}. <br/>
- `<img src="{{course.img}}" alt="{{course.title}}">` - картинка курса {{course.img}} с названием заголовка курса {{course.title}}. <br/>
- `<p class="price big">{{course.price}}</p>` - параграф с классом class="price big" показывающий цену курса {{course.price}}. <br/>

Итоговый вид страницы одного курса: <br/>

[![2.png](https://i.postimg.cc/xCP1p8qF/2.png)](https://postimg.cc/VrdwdYqj)

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела). <br/>
↩️ [К оглавлению документации](../README.md). <br/>

---

### Редактировать курс

Код шаблона course-edit.hbs: <br/>
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
Где: <br/>
- `<form action="/courses/edit" method="POST" class="course-form"></form>` -  *возможность шаблонизатора Handlebars*. Метод запроса post по маршруту '/courses/edit', который выполняет функции контроллера редактирования курса. <br/>
- `<input id="title" name="title" type="text" class="validate" required value="{{course.title}}">` - input содержащий ID id="title" поля ввода, его название name="title", тип type="text", класс `class="validate"` и обязательным атрибутом required, который должен наличивать значение заголовка курса {{course.title}}. <br/>
- `<input type="hidden" name="id" value="{{course.id}}">` - *возможность шаблонизатора Handlebars*. Cкрытый обработчик проверки ID курса {{@root.csrf}}. <br/>
- `<input type="hidden" name="_csrf" value="{{csrf}}">` - *возможность шаблонизатора Handlebars*. Cкрытый обработчик прав доступа авторизации {{@root.csrf}}. <br/>
- `<button type="submit" class="btn btn-primary">Редактировать курс</button>` - кнопка выполняющая код контроллера запроса post по маршруту '/courses/edit'. <br/>
- `<form action="/courses/remove" method="POST"></form>` - *возможность шаблонизатора Handlebars*. Метод запроса post по маршруту '/courses/edit', который выполняет функции контроллера удаление курса. <br/>

На странице курсов, как было сказано выше, пользотель может редактировать только те курсы, которые сам создал: <br/>

[![2.png](https://i.postimg.cc/VLXmK5T9/2.png)](https://postimg.cc/4m4Dd4Xn)

Итоговый вид страницы "Редактировать курс": <br/>

[![3.png](https://i.postimg.cc/WbxTngX1/3.png)](https://postimg.cc/vc7J81JJ)

⬅️ Подробнее о контроллерах, который используется в шаблоне "Редактировать курс" смотрите [Метод запроса post по маршруту '/courses/remove'](controllers.md/#метод-запроса-post-по-маршруту-coursesremove) и [Метод запроса post по маршруту '/courses/edit'](controllers.md/#метод-запроса-get-по-маршруту-coursesidedit). <br/>
⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md). <br/>

---

### Страница 404

Код шаблона 404.hbs: <br/>
```handlebars
<h1>404 Ошибка</h1>
<p>Страница не найдена</p>
<hr>

<a href="/">Вернуться на главную страницу</a>
```
Где: <br/>
- `<h1>404 Ошибка</h1>` - заголовок страницы 404. <br/>
- `<p>Страница не найдена</p>` - параграф с текстом Страница не найдена. <br/>
- `<a href="/">Вернуться на главную страницу</a>` - ccылка ведущая на главную страницу сайта. <br/>

Итоговый вид страницы "Страница 404": <br/>

[![1.png](https://i.postimg.cc/yYCz3sdD/1.png)](https://postimg.cc/23GtPpnY)

⬆️ [К оглавлению раздела "Представления"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md). <br/>