# Промежуточные обработчики

Промежуточные обработчики - необходимы в большинстве случаев как обработчики авторизации, валидации, аутентификации и других различных проверок. Промежуточные обработчики могут быть как внешними библиотеками, которые подключают глобально, так самописные промежуточные обработчики подключаемые локально в маршрутизаторы. Ниже будет подробно разобраны промежуточные обработчики, которые используются в приложении

⬅️ **Подробную информацию обо всех маршрутах смотреть в разделе [Маршрутизация](routes.md)** <br/> 
⬅️ **Подробную информацию обо всех контроллерах смотреть в разделе [Контроллеры](controllers.md)** <br/> 
↩️ [К оглавлению документации](../README.md) <br/> 

## Оглавление раздела
- [Промежуточные обработчики](#промежуточные-обработчики)
  - [Оглавление раздела](#оглавление-раздела)
  - [Описание промежуточных обработчиков](#описание-промежуточных-обработчиков)
  - [Реализация библиотечных промежуточных обработчиков](#реализация-библиотечных-промежуточных-обработчиков)
    - [bcryptjs](#bcryptjs)
      - [Использование bcryptjs в контроллере авторизации с методом запроса post по машруту /auth/login](#использование-bcryptjs-в-контроллере-авторизации-с-методом-запроса-post-по-машруту-authlogin)
      - [Использование bcryptjs в контроллере авторизации с методом запроса post по машруту /auth/register](#использование-bcryptjs-в-контроллере-авторизации-с-методом-запроса-post-по-машруту-authregister)
      - [Использование bcryptjs в контроллере авторизации с методом запроса post по машруту /auth/password](#использование-bcryptjs-в-контроллере-авторизации-с-методом-запроса-post-по-машруту-authpassword)
    - [crypto](#crypto)
    - [helmet](#helmet)
    - [express-session](#express-session)
    - [connect-flash](#connect-flash)
      - [Использование connect-flash в контроллере авторизации с методом запроса get по машруту /auth/login](#использование-connect-flash-в-контроллере-авторизации-с-методом-запроса-get-по-машруту-authlogin)
      - [Использование connect-flash в контроллере авторизации с методом запроса get по машруту /auth/reset](#использование-connect-flash-в-контроллере-авторизации-с-методом-запроса-get-по-машруту-authreset)
      - [Использование connect-flash в контроллере авторизации с методом запроса get по машруту /auth/password/:token](#использование-connect-flash-в-контроллере-авторизации-с-методом-запроса-get-по-машруту-authpasswordtoken)
      - [Использование connect-flash в контроллере авторизации с методом запроса post по машруту /auth/login](#использование-connect-flash-в-контроллере-авторизации-с-методом-запроса-post-по-машруту-authlogin)
      - [Использование connect-flash в контроллере авторизации с методом запроса post по машруту /auth/register](#использование-connect-flash-в-контроллере-авторизации-с-методом-запроса-post-по-машруту-authregister)
      - [Использование connect-flash в контроллере авторизации с методом запроса post по машруту /auth/reset](#использование-connect-flash-в-контроллере-авторизации-с-методом-запроса-post-по-машруту-authreset)
      - [Использование connect-flash в контроллере авторизации с методом запроса post по машруту /auth/password](#использование-connect-flash-в-контроллере-авторизации-с-методом-запроса-post-по-машруту-authpassword)
    - [csurf](#csurf)
      - [Использование csurf в представлении страницы "Корзина"](#использование-csurf-в-представлении-страницы-корзина)
      - [Использование csurf в представлении страницы редактирования одного курса](#использование-csurf-в-представлении-страницы-редактирования-одного-курса)
      - [Использование csurf в представлении страницы "Курсы"](#использование-csurf-в-представлении-страницы-курсы)
  - [- `<input type="hidden" name="_csrf" value="{{csrf}}">` - токен промежуточного обработчика csurf, который проверяется по сеансу пользователя.](#--input-typehidden-name_csrf-valuecsrf---токен-промежуточного-обработчика-csurf-который-проверяется-по-сеансу-пользователя)
      - [Использование csurf в представлении страницы "Профиль"](#использование-csurf-в-представлении-страницы-профиль)
      - [Использование csurf в представлении страницы "Авторизация"](#использование-csurf-в-представлении-страницы-авторизация)
    - [compression](#compression)
    - [fileMiddleware](#filemiddleware)
    - [nodemailer-sendgrid-transport](#nodemailer-sendgrid-transport)
      - [Подключение всех зависимостей nodemailer-sendgrid-transport](#подключение-всех-зависимостей-nodemailer-sendgrid-transport)
      - [Письмо подтверждающее успешную регистрацию](#письмо-подтверждающее-успешную-регистрацию)
      - [Письмо смены пароля](#письмо-смены-пароля)
  - [Реализация самописных промежуточных обработчиков](#реализация-самописных-промежуточных-обработчиков)
    - [auth](#auth)
    - [errorHandler](#errorhandler)
    - [registerValidators](#registervalidators)
      - [Валидация регистрируемой почты](#валидация-регистрируемой-почты)
      - [Валидация регистрируемого пароля](#валидация-регистрируемого-пароля)
      - [Валидация подтверждения пароля](#валидация-подтверждения-пароля)
      - [Валидация регистрируемого имени](#валидация-регистрируемого-имени)
      - [Подключение промежуточного обработчика registerValidators](#подключение-промежуточного-обработчика-registervalidators)
    - [courseValidators](#coursevalidators)
    - [UserMiddleware](#usermiddleware)
    - [varMiddleware](#varmiddleware)


## Описание промежуточных обработчиков

Промежуточные обработчики могут быть как внешними библиотеками так и утилиты написаные самим разработчиком. Одновременно с этим, сами промежуточные обработчики могут быть подключение к приложению глобально, так и локально, в зависимости от бизнес-логики приложение. <br/> 

---

Пример глобального подключения промежуточного обработчика <br/> 

```node
const flash = require('connect-flash')
app.use(flash())
```
Где: <br/> 
- `connect-flash` - библиотека из npm, которое отвечает за отображение валидированных ошибок. <br/> 
- `app.use(flash())` - подключение библиотеки connect-flash к приложению. <br/> 
- `app.use()` - метод use объекта app, создающий подключение midlleware (промежуточных обработчиков) к приложения. Объект app является результатом работы функции express. Код подключения express ниже: <br/> 

```node
const express = require('express')
const app = express()
```

---

Пример локального промежуточного обработчика <br/> 

```node
const bcrypt = require('bcryptjs')

router.post('/login', async (req, res) => {
  const {email, password} = req.body
  const candidate = await User.findOne({ email })

  if (candidate) {
    const areSame = await bcrypt.compare(password, candidate.password)
  }
})
```
Где: <br/> 
- `bcryptjs` - библиотека из npm, которая шифрует строки. <br/> 
- `router.post('/login', async (req, res) => {})` - маршрутизатор с методом запроса post, который. Подробнее о данном маршрутизаторе смотрите [Метод запроса post по маршруту '/auth/login'](controllers.md#метод-запроса-post-по-маршруту-authlogin) <br/> 
- `bcrypt.compare(password, candidate.password)` - метод compare библиотеки bcryptjs, который сравнивает два значение. В данном примере - существующий пароль, который привязан к email пользователя с базы данных и вводимый кандидатом пароль. <br/> 

---

Пример самописного промежуточного обработчика <br/> 

```node
module.exports = function(req, res, next) {
  if (!req.session.isAuthenticated) {
    return res.redirect('/auth/login')
  }

  next()
}
```
Где: <br/> 
- `function(req, res, next) {})` - промежуточные обработчики отличаются наличием третъего аргумента next, который должен обязательно быть вызван вконце логики промежуточного обработчика. <br/> 
- `if (!req.session.isAuthenticated) {}` - проверка на авторизацию пользователя. <br/> 
- `return res.redirect('/auth/login')` - метод redirect объекта ответа response, который перенаправляет пользователя на страницу по маршуту '/auth/login'. <br/> 
- `next()` - обязательный параметр, определяет завершение логики кода данного промежуточного обработчика. <br/> 

---

Таблица всех промежуточных обработчиков, которые реализованы в приложении: <br/> 

|      Название      | Подключение |              Происхождение               |                          Короткое описание                          |
| :----------------: | :---------: | :--------------------------------------: | :-----------------------------------------------------------------: |
|      bcryptjs      |  Локально   |           Библиотека bcryptjs            |                          Хеширует пароли                            |
|       crypto       |  Локально   |            Библиотека crypto             |                 Генерирует криптографические байты                  |
|       helmet       |  Глобально  |            Библиотека helmet             |    Дополняет защиту приложения путём добавление заголовков HTTP     |
|       session      |  Глобально  |        Библиотека express-session        |                       Создаёт хранилище сессий                      | 
|    connect-flash   |  Глобально  |             Библиотека flash             |             Выводит сообщения в случае ошибок валидации             |
|        csurf       |  Глобально  |             Библиотека csurf             |                       CSRF защита приложения                        |
|    compression     |  Глобально  |          Библиотека compression          |  Сжимает приложение в gzip для повышение быстродействия приложения  |
|     nodemailer     |  Локально   |           Библиотека nodemailer          |       Формирование писем, которые будут отправляться по почте       |
|   fileMiddleware   |  Глобально  |               Файл file.js               |   Обработка загружаемых картинок, которые использутся для аватара   |
|        auth        |  Локально   |               Файл auth.js               |                 Проверка на авторизацию пользователя                |
|    errorHandler    |  Глобально  |              Файл error.js               |                          Вывод страницы 404                         |
| registerValidators |  Локально   |            Файл validators.js            |    Выводит ошибки валидации при регистрации нового пользователя     |
|  courseValidators  |  Локально   |            Файл validators.js            | Выводит ошибки валидации при создании / редактировании одного курса |
|   UserMiddleware   |  Глобально  |               Файл user.js               |          Организация допуска авторизированым пользователям          |
|    varMiddleware   |  Глобально  |             Файл variables.js            |        Создание токена CSRF для авторизированого пользователя       |

↩️ [К оглавлению раздела](#оглавление-раздела) <br/> 
↩️ [К оглавлению документации](../README.md) <br/> 

## Реализация библиотечных промежуточных обработчиков

---

### bcryptjs

bcryptjs позволяет хешировать пароли. Поскольку манипуляции с пароля в приложении производятся в контроллерах авторизации, то и следовательно модуль подключается лишь здесь. Код подключения bcryptjs в файл auth.js: <br/> 

```node
const bcrypt = require('bcryptjs')
```

---

#### Использование bcryptjs в контроллере авторизации с методом запроса post по машруту /auth/login

Библиотека в данном контролере используется для сравнения существующего пароля привязанного к существуему email пользователя с базы данных и вводимый кандидатом пароль на получение авторизации. Часть кода контроллера работающий с промежуточным обработчиком: <br/>

```node
router.post('/login', async (req, res) => {
...
  const {email, password} = req.body
  const candidate = await User.findOne({ email })

  if (candidate) {
    const areSame = await bcrypt.compare(password, candidate.password)
  }
  ...
...
})
```
Где: <br/> 
- `router.post('/login', async (req, res) => {})` - метод запроса post объекта router, который принимает 2 аргумента: путь '/login' и callback функцию. <br/> 
- `{email, password}` - конкретные ключи деструктуризированные с тела объекта запроса request.body. <br/> 
- `User.findOne({ email })` - метод findOne модели User, который ищет пользователя в базе данных MongoDB по его email. <br/> 
- `bcrypt.compare(password, candidate.password)` - метод compare библиотеки bcryptjs, который сравнивает существующий пароль, который привязан к email пользователя с базы данных и вводимый кандидатом пароль. <br/> 

Подробнее о контролере смотрите [Метод запроса post по маршруту '/auth/login'](controllers.md#метод-запроса-post-по-маршруту-authlogin). <br/>
Подробнее о реализации машрутизатора смотрите [Реализация маршрутов](routes.md#реализация-маршрутов). <br/>

---

#### Использование bcryptjs в контроллере авторизации с методом запроса post по машруту /auth/register

Библиотека в данном контролере используется для хеширования пароля пользователя, который регистрируется. Часть кода контроллера работающий с промежуточным обработчиком: <br/>
```node
router.post('/register', registerValidators, async (req, res) => {
  ...
  const {email, password, name} = req.body
  ...
  const hashPassword = await bcrypt.hash(password, 10)
...
})
```
Где: <br/> 
- `router.post('/register', registerValidators, async (req, res) => {})` - метод запроса post объекта router, который принимает 3 аргумента: путь '/register', промежуточный обработчик registerValidators и callback функцию. <br/> 
- `registerValidators` - промежуточный обработчик валидирующий ошибки при регистрации. Подробнее о данном промежуточном обработчике смотрите [registerValidators](#registervalidators). <br/> 
- `{email, password, name}` - конкретные ключи деструктуризированные с тела объекта запроса request.body. <br/> 
- `bcrypt.hash(req.body.password, 10)` - метод hash библиотеки bcrypt, который принимает 2 аргумента: вводимый пользователем пароль req.body.password и количество хешируемых символов - 10. <br/> 


Подробнее о контролере смотрите [Метод запроса post по маршруту '/auth/register'](controllers.md#метод-запроса-post-по-маршруту-authregister). <br/>
Подробнее о реализации машрутизатора смотрите [Реализация маршрутов](routes.md#реализация-маршрутов). <br/>

---

#### Использование bcryptjs в контроллере авторизации с методом запроса post по машруту /auth/password

Библиотека в данном контролере используется для хеширования вводимого пароля. Часть кода контроллера, работающий с промежуточным обработчиком: <br/>
```node
router.post('/password', async (req, res) => {
  ...
  if (user) {
    user.password = await bcrypt.hash(req.body.password, 10)
    ...
})
```
Где: <br/>
- `router.post('/password', async (req, res) => {})` - метод запроса post объекта router, который принимает 2 аргумента: путь '/password' и callback функцию. <br/>
- `bcrypt.hash(req.body.password, 10)` - метод hash библиотеки bcrypt, который принимает 2 аргумента: вводимый пользователем пароль req.body.password и количество хешируемых символов - 10. <br/> 


Подробнее о данном контролере смотрите [Метод запроса post по маршруту '/auth/password'](controllers.md#метод-запроса-post-по-маршруту-authpassword). <br/>
Подробнее о реализации машрутизатора смотрите [Реализация маршрутов](routes.md#реализация-маршрутов). <br/>

↩️ [К оглавлению раздела](#оглавление-раздела) <br/> 

---

### crypto 

crypto необходим для генерации криптографически хорошо построенных искусственных случайных данных, которые в последствии будут отправлятся пользователю как уникальная ссылка для смены пароля пользователем. Код подключение и использование crypto в файле auth.js:  <br/> 
```node
const crypto = require('crypto')
...
router.post('/reset', (req, res) => {
    crypto.randomBytes(32, async (err, buffer) => {})
...
})
```
Где:  <br/> 
- `crypto` - библиотека crypto.  <br/> 
- `router.post('/reset', (req, res) => {})` - метод запроса post объекта router, который принимает 2 аргумента: путь '/reset' и callback функцию.  <br/> 
- `crypto.randomBytes(32, async (err, buffer) => {})` - метод randomBytes библиотеки crypto, который генерирует криптографические данные.  <br/> 

Подробнее о данном контролере смотрите [Метод запроса post по маршруту '/auth/reset'](controllers.md#метод-запроса-post-по-маршруту-authreset). <br/>
Подробнее о реализации машрутизатора смотрите [Реализация маршрутов](routes.md#реализация-маршрутов). <br/>

↩️ [К оглавлению раздела](#оглавление-раздела) <br/> 

---

### helmet 

helmet помогает защитить приложение от некоторых широко известных веб-уязвимостей путем соответствующей настройки заголовоков HTTP. Код отвечающий за настройку и подключение helmet в главный файл index.js:  <br/> 
```node
app.use(helmet({
  contentSecurityPolicy: {
    directives: {
      ...helmet.contentSecurityPolicy.getDefaultDirectives(),
      "img-src": ["'self'", "https:"],
      "script-src-elem": ["'self'", "https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/js/materialize.min.js", "'unsafe-inline'" ] 
    },
  },
}))
```
Где: <br/> 
- `helmet` - оболочка для различных промежуточных обработчиков, которые установлены по умолчанию.  <br/> 
- `contentSecurityPolicy: {})` - промежуточный обработчик, который устанавливает специфические настройки заголовка Content-Security-Policy. <br/> 
- `directives` - объект, где ключи - это название директив. <br/> 
- `...helmet.contentSecurityPolicy.getDefaultDirectives()` - объект директив по умолчанию. <br/> 
- `"img-src": ["'self'", "https:"]` - ряд параметров для защиты подключаемых картинок. <br/> 
- `"script-src-elem": ["'self'", "https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/js/materialize.min.js", "'unsafe-inline'" ]` - ряд параметром для защиты подключаемых скриптов. <br/> 

↩️ [К оглавлению раздела](#оглавление-раздела) <br/> 

---

### express-session

express-session создаёт объект сессии, данные которой хранятся на стороне сервера. Код, отвечающий за подключение и настройку express-session в главном файле index.js: <br/> 
```node
const session = require('express-session')
... 
app.use(session({
  secret: keys.SESSION_SECRET,
  resave: false,
  saveUninitialized: false,
  store
}))
```
Где: <br/> 
- `session` - оболочка принимающая ряд параметров настройки session. <br/> 
- `secret: keys.SESSION_SECRET` - уникальный ключ сессий. <br/> 
- `resave: false` - принудительное сохранение сеанса в хранилище сессий, даже если сеанс не изменялся во время запроса, true по умолчание. Значение false выставляется для избежания ошибок в случае параллельных запросов пользователем. <br/> 
- `saveUninitialized: false` - сохранение неинициализированных сеансов в хранилище сессий, true по умолчанию. Значение false выставляется во избежания ошибок в случае параллельных запросов пользователем и уменьшения использования хранилище сервера. <br/> 
- `store` - ключ принимающий одноименное значение объекта store. Код объекта store указан ниже: <br/> 
```node
const MongoStore = require('connect-mongodb-session')(session)
...
const store = new MongoStore({
  collection: 'sessions',
  uri: keys.MONGODB_URL
})
```
Где: <br/> 
- `MongoStore({})` - объект устанавливающий соединение express приложения и хранилище сессий в MongoDB. <br/> 
- `collection: 'sessions'` - название коллекции сессий в базе данных MongoDB. <br/> 
- `uri: keys.MONGODB_URL` - URL путь соединения с базой данных MongoDB. <br/> 

↩️ [К оглавлению раздела](#оглавление-раздела) <br/> 

---

### connect-flash

connect-flash необходим как специальная область сеанса, которая используется для хранения сообщений. Сообщения используется в связке с перенаправлениями во время действий авторизации. Сам connect-flash подключается в главном файле index.js после чего используется локально в контроллерах авторизации. Подробнее о контроллерах авторизации смотрите [Контроллеры страницы "Вход / Выход"](#контроллеры-страницы-вход--выход). Код подключения connect-flash в главном файле index.js: <br/> 
```node
const flash = require('connect-flash')
app.use(flash())
```

#### Использование connect-flash в контроллере авторизации с методом запроса get по машруту /auth/login

Библиотека в данном контролере используется для вывода ошибок валидации при вводе почты и пароля пользователя. Часть кода контроллера работающий с промежуточным обработчиком: <br/>
```node
router.get('/login', async (req, res) => {
  res.render('auth/login', {
    ...
    loginError: req.flash('loginError'),
    registerError: req.flash('registerError')
  })
})
```
Где: <br/> 
- `router.get('/login', async (req, res) => {})` -  метод запроса get объекта router, который принимает 2 аргумента: путь '/login' и callback функцию. <br/> 
- `res.render('auth/login', {})` - метод render объекта res отправляет данные в объекте ответа для последующей визуализации представления шаблонизатором handlebars. <br/> 
- `loginError: req.flash('loginError')` - ключ loginError принимающий значение сообщения ошибки валидации приходящее с телом запроса req. В зависимости от данных введённых пользователем, может вывести текст "Неверный пароль" или "Такого пользователя не существует". Часть кода представления работающий с валидатором: <br/> 
```handlebars
<div id="login" class="col s6 offset-s3">
...
  {{#if loginError}}
    <p class="alert">{{loginError}}</p>
  {{/if}}
...
</div>
```
- `registerError: req.flash('registerError')` - ключ registerError принимающий значение сообщения ошибки валидации приходящее с телом запроса req. Часть кода представления работающий с валидатором:
```handlebars
<div id="login" class="col s6 offset-s3">
...
  {{#if registerError}}
    <p class="alert">{{registerError}}</p>
  {{/if}}
...
</div>
```

Подробнее о данном контролере смотрите [Метод запроса get по маршруту '/auth/login'](controllers.md#метод-запроса-get-по-маршруту-authlogin). <br/>
Подробнее о данном представлении смотрите [Вход / Регистрация](views.md#вход--регистрация). <br/>
Подробнее о реализации машрутизатора смотрите [Реализация маршрутов](routes.md#реализация-маршрутов). <br/>

---

#### Использование connect-flash в контроллере авторизации с методом запроса get по машруту /auth/reset

Библиотека в данном контролере используется для вывода ошибок валидации при вводе данных для сброса пароля. Часть кода контроллера работающий с промежуточным обработчиком: <br/>
```node
router.get('/reset', (req, res) => {
  res.render('auth/reset', {
    ...
    error: req.flash('error')
  })
})
```
Где: 
- `router.get('/reset', (req, res) => {})` - метод запроса get объекта router, который принимает 2 аргумента: путь '/reset' и callback функцию.
- `res.render('auth/reset', {})` - метод render объекта res отправляет данные в объекте ответа для последующей визуализации представления шаблонизатором handlebars.
- `error: req.flash('error')`  - ключ error принимающий значение сообщения ошибки валидации приходящее с телом запроса req.

Подробнее о данном контролере смотрите [Метод запроса get по маршруту '/auth/reset'](controllers.md#метод-запроса-get-по-маршруту-authreset). <br/>
Подробнее о представлении смотрите [Вход / Регистрация](views.md#вход--регистрация). <br/>
Подробнее о реализации машрутизатора смотрите [Реализация маршрутов](routes.md#реализация-маршрутов). <br/>

---

#### Использование connect-flash в контроллере авторизации с методом запроса get по машруту /auth/password/:token

Библиотека в данном контролере используется для вывода ошибок валидации при вводе данных для создания нового пароля. Часть кода контроллера работающий с промежуточным обработчиком: <br/>
```node
router.get('/password/:token', async (req, res) => {
    ...
    res.render('auth/password', {
    error: req.flash('error')
  })
...
})
```
Где:
- `router.get('/password/:token', async (req, res) => {})` - метод запроса get объекта router, который принимает 2 аргумента: путь '/password/:token' и callback функцию.
- `res.render('auth/password', {})` -  метод render объекта res отправляет данные в объекте ответа для последующей визуализации представления шаблонизатором handlebars.
- `error: req.flash('error')` -  ключ error принимающий значение сообщения ошибки валидации приходящее с телом запроса req.

Подробнее о данном контролере смотрите [Метод запроса get по маршруту '/auth/password/:token'](controllers.md#метод-запроса-get-по-маршруту-authpasswordtoken). <br/>
Подробнее о представлении смотрите [Вход / Регистрация](views.md#вход--регистрация). <br/>
Подробнее о реализации машрутизатора смотрите [Реализация маршрутов](routes.md#реализация-маршрутов). <br/>

---

#### Использование connect-flash в контроллере авторизации с методом запроса post по машруту /auth/login

Библиотека в данном контролере используется для вывода ошибок валидации при вводе ошибочных или несуществующих данных пользователем в окне авторизации в свой аккаунт. Часть кода контроллера работающий с промежуточным обработчиком: <br/>
```node
router.post('/login', async (req, res) => {
    ...
        req.session.save(err => {
          if (err) {
            throw err
          }
          res.redirect('/')
        })
      } else {
        req.flash('loginError', 'Неверный пароль')
        res.redirect('/auth/login#login')
      }
    } else {
      req.flash('loginError', 'Такого пользователя не существует')
      res.redirect('/auth/login#login')
    }
...
})
```
Где:
- `router.post('/login', async (req, res) => {})` - метод запроса post объекта router, который принимает 2 аргумента: путь '/login' и callback функцию.
- `req.flash('loginError', 'Неверный пароль')` - метод flash принимающий 2 аргумента: название ошибки 'loginError' и текст ошибки 'Неверный пароль'. 
- `res.redirect('/auth/login#login')` - метод redirect объекта res перенаправляет пользователя на страницу по маршруту /auth/login#login.
- `req.flash('loginError', 'Такого пользователя не существует')` - метод flash принимающий 2 аргумента: название ошибки 'loginError' и текст ошибки 'Такого пользователя не существует'. 

Подробнее о данном контролере смотрите [Метод запроса post по маршруту '/auth/login'](controllers.md#метод-запроса-post-по-маршруту-authlogin). <br/>
Подробнее о представлении смотрите [Вход / Регистрация](views.md#вход--регистрация). <br/>
Подробнее о реализации машрутизатора смотрите [Реализация маршрутов](routes.md#реализация-маршрутов). <br/>

---

#### Использование connect-flash в контроллере авторизации с методом запроса post по машруту /auth/register

Библиотека в данном контролере используется для вывода ошибок валидации при вводе регистрируемых данных пользователем. Часть кода контроллера работающий с промежуточным обработчиком: <br/>
```node
router.post('/register', registerValidators, async (req, res) => {
    ...
    if (!errors.isEmpty()) {
      req.flash('registerError', errors.array()[0].msg)
      return res.status(422).redirect('/auth/login#register')
...
})
```
Где: 
- `router.post('/register', registerValidators, async (req, res) => {})` - метод запроса post объекта router, который принимает 2 аргумента: путь '/register' и callback функцию.
- `(!errors.isEmpty()) {}` - если поле валидации наличивает символы - выполняется тело условия.
- `req.flash('registerError', errors.array()[0].msg)` - поскольку валидатор ошибки у нас проверяется в представлении, то текст ошибки нам не обязательно делать смысловым на стороне сервера. На стороне сервера в случае ошибки валидации будет выводится первое значение встроенных текстов в объекте errors, которое приведённое к массиву, чтобы забрать текст ошибки по его индексу. Сам текст ошибки выглядит так: "Invalid value".
- `return res.status(422).redirect('/auth/login#register')` - метод redirect объекта res перенаправляет пользователя на страницу по маршруту /auth/login#register.

Подробнее о данном контролере смотрите [Метод запроса post по маршруту '/auth/register'](controllers.md#метод-запроса-post-по-маршруту-authregister). <br/>
Подробнее о представлении смотрите [Вход / Регистрация](views.md#вход--регистрация). <br/>
Подробнее о реализации машрутизатора смотрите [Реализация маршрутов](routes.md#реализация-маршрутов). <br/>

---

#### Использование connect-flash в контроллере авторизации с методом запроса post по машруту /auth/reset

Библиотека в данном контролере используется для вывода ошибок валидации при вводе почты, привязанный пароль к которой пользователь забыл и хочет сбросить. Часть кода контроллера работающий с промежуточным обработчиком: <br/>
```node
router.post('/reset', (req, res) => {
    ...
    crypto.randomBytes(32, async (err, buffer) => {
      if (err) {
        req.flash('error', 'Что-то пошло не так, повторите попытку позже')
        return res.redirect('/auth/reset')
      }

      if (candidate) {
      ...
        await transporter.sendMail(resetEmail(candidate.email, token))
        res.redirect('/auth/login')
      } else {
        req.flash('error', 'Такого email нет')
        res.redirect('/auth/reset')
      }
...
})
```
Где:
- `crypto.randomBytes(32, async (err, buffer) => {})` - метод randomBytes библиотеке crypto, который генерирует хорошо построенные криптографические искусственные данные.
- `req.flash('error', 'Что-то пошло не так, повторите попытку позже')` - метод flash принимающий 2 аргумента: название ошибки 'error' и текст ошибки 'Что-то пошло не так, повторите попытку позже'. 
- `return res.redirect('/auth/reset')` - метод redirect объекта res перенаправляет пользователя на страницу по маршруту /auth/reset.
- `transporter.sendMail(resetEmail(candidate.email, token))` - 
- `res.redirect('/auth/login')` - метод redirect объекта res перенаправляет пользователя на страницу по маршруту /auth/login.
- `req.flash('error', 'Такого email нет')` - метод flash принимающий 2 аргумента: название ошибки 'error' и текст ошибки 'Такого email нет'. 
- `res.redirect('/auth/reset')` - метод redirect объекта res перенаправляет пользователя на страницу по маршруту /auth/reset.

Подробнее о данном контролере смотрите [Метод запроса post по маршруту '/auth/reset'](controllers.md#метод-запроса-post-по-маршруту-authreset). <br/>
Подробнее о представлении смотрите [Вход / Регистрация](views.md#вход--регистрация). <br/>
Подробнее о реализации машрутизатора смотрите [Реализация маршрутов](routes.md#реализация-маршрутов). <br/>

---

#### Использование connect-flash в контроллере авторизации с методом запроса post по машруту /auth/password

Библиотека в данном контролере используется для вывода ошибок валидации вводе нового пароля пользователем. Часть кода контроллера работающий с промежуточным обработчиком: <br/>
```node
router.post('/password', async (req, res) => {

    if (user) {
      user.password = await bcrypt.hash(req.body.password, 10)
      ...
      res.redirect('/auth/login')
    } else {
      req.flash('loginError', 'Время жизни токена истекло')
      res.redirect('/auth/login')
    }
...
})
```
Где:
- `router.post('/password', async (req, res) => {})` - метод запроса post объекта router, который принимает 2 аргумента: путь '/password' и callback функцию.
- `bcrypt.hash(req.body.password, 10)` - метод hash библиотеки bcrypt который принимает два аргумента: вводимый пользователем пароль req.body.password и количество хешируемых символов - 10
- `res.redirect('/auth/login')` - метод redirect объекта res перенаправляет пользователя на страницу по маршруту /auth/login.
- `req.flash('loginError', 'Время жизни токена истекло')` - метод flash принимающий 2 аргумента: название ошибки 'loginError' и текст ошибки 'Время жизни токена истекло'.

Подробнее о данном контролере смотрите [Метод запроса post по маршруту '/auth/password'](controllers.md#метод-запроса-post-по-маршруту-authpassword). <br/>
Подробнее о представлении смотрите [Вход / Регистрация](views.md#вход--регистрация). <br/>
Подробнее о реализации машрутизатора смотрите [Реализация маршрутов](routes.md#реализация-маршрутов). <br/>

↩️ [К оглавлению раздела](#оглавление-раздела) <br/> 

---

### csurf

csrf используется для защиты от подделки межасайтовых запросов (CSRF). Код подключения connect-flash в главный файл index.js:
```node
const csrf = require('csurf')
app.use(csrf())
```
Использование csrf защиты выходит путём подключения скритого обработчика csrf в шаблони handlebars. Часть кода шаблонизатора cтраницы "Добавить курс" add.hbs который работает с промежуточным обработчиком csurf:
```handlebars
<form action="/add" method="POST">
  ...
  <input type="hidden" name="_csrf" value="{{csrf}}">
  ...
</form>  
```
Где: 
- `form action="/add" method="POST"` - форма контроллера с методом запроса post по машруту /add.
- `<input type="hidden" name="_csrf" value="{{csrf}}">` - промежуточный обработчик csurf не отображающийся в представлении, но при этом обеспечивает создание токена. Этот токен проверяется по сеансу пользователя. 

---

#### Использование csurf в представлении страницы "Корзина"
Часть кода шаблонизатора cтраницы "Корзины" card.hbs который работает с промежуточным обработчиком csurf:
```handlebars
{{#each courses}}
...
<button 
  class="btn btm-small js-remove" 
  data-id="{{id}}"
  data-csrf="{{@root.csrf}}"   
  >Удалить</button>
...
{{/each}}

<form action="/orders" method="POST"> 
  <input type="hidden" name="_csrf" value="{{csrf}}">
</form> 
```
Где: 
- `data-csrf="{{@root.csrf}}"` - поле, которое визуализирует кнопку "Удалить" только для авторизированного пользователя владеющего токеном защиты CSRF.
- `form action="/orders" method="POST"` - форма контроллера с методом запроса post по машруту /orders.
- `<input type="hidden" name="_csrf" value="{{csrf}}">` - токен промежуточного обработчика csurf, который проверяется по сеансу пользователя. 

---

#### Использование csurf в представлении страницы редактирования одного курса
Часть кода шаблонизатора cтраницы редактирования одного курса course-edit.hbs который работает с промежуточным обработчиком csurf:
```handlebars
<form action="/courses/edit" method="POST" class="course-form">
  <input type="hidden" name="_csrf" value="{{csrf}}">
</form>
...
<form action="/courses/remove" method="POST">
 <input type="hidden" name="_csrf" value="{{csrf}}">
</form>
```
Где: 
- `form action="/courses/edit" method="POST"` - форма контроллера с методом запроса post по машруту /courses/edit.
- `form action="/courses/remove" method="POST"` - форма контроллера с методом запроса post по машруту /courses/remove.
- `<input type="hidden" name="_csrf" value="{{csrf}}">` - токен промежуточного обработчика csurf, который проверяется по сеансу пользователя. 

---

#### Использование csurf в представлении страницы "Курсы"
Часть кода шаблонизатора cтраницы "Курсы" courses.hbs который работает с промежуточным обработчиком csurf:
```handlebars
{{#if @root.isAuth}}
  <form action="/card/add" method="POST">
    <input type="hidden" name="_csrf" value="{{@root.csrf}}"> 
  </form>
{{/if}}
```
Где: 
- `@root.isAuth` - права авторизированного пользовтаеля. Подробнее о правах смотрите промежуточных обработчик [varMiddleware](#varmiddleware).
- `form action="/card/add" method="POST"` - форма контроллера с методом запроса post по машруту /card/add.
- `<input type="hidden" name="_csrf" value="{{csrf}}">` - токен промежуточного обработчика csurf, который проверяется по сеансу пользователя. 
---

#### Использование csurf в представлении страницы "Профиль"
Часть кода шаблонизатора cтраницы "Профиль" profile.hbs который работает с промежуточным обработчиком csurf:
```handlebars
<form action="/profile" method="POST" enctype="multipart/form-data">
  <input type="hidden" name="_csrf" value="{{csrf}}">
</form>
```
Где: 
- `form action="/profile" method="POST" enctype="multipart/form-data"` - форма контроллера с методом запроса post по машруту /profile.
- `<input type="hidden" name="_csrf" value="{{csrf}}">` - токен промежуточного обработчика csurf, который проверяется по сеансу пользователя. 


---

#### Использование csurf в представлении страницы "Авторизация" 

**Раздел логин**

auth/login.hbs
```handlebars
<form action="/auth/login" method="POST">
  <input type="hidden" name="_csrf" value="{{csrf}}">
</form>
```

**Раздел Регистрация**

```handlebars
<form action="/auth/register" method="POST" novalidate>
  <input type="hidden" name="_csrf" value="{{csrf}}">
</form>
```

**Раздел сброса пароля**


auth/password.hbs
```handlebars
 <form action="/auth/password" method="POST">
  <input type="hidden" name="_csrf" value="{{csrf}}">
</form>
```

**Раздел смены пароля**

auth/password.hbs
```handlebars
 <form action="/auth/reset" method="POST">
  <input type="hidden" name="_csrf" value="{{csrf}}">
</form>
```


Подробнее о данном контролере смотрите [Метод запроса post по маршруту '/auth/password'](controllers.md#метод-запроса-post-по-маршруту-authpassword). <br/>
Подробнее о представлении, которое использует данные переданные вместе с методом render объекта ответа response смотрите [Вход / Регистрация](views.md#вход--регистрация). <br/>
Подробнее о реализации машрутизатора смотрите [Реализация маршрутов](routes.md#реализация-маршрутов). <br/>

↩️ [К оглавлению раздела](#оглавление-раздела) <br/> 

---

### compression

compression необходим как модуль для сжатия gzip, чтобы уменьшенить размер тела ответа и соответственно, увеличить быстродействие веб-приложения. Код подключения промежуточного обработчика в главний файл index.js:
```node
const compression = require('compression')
app.use(compression())
```

↩️ [К оглавлению раздела](#оглавление-раздела) <br/> 

---

### fileMiddleware

fileMiddleware использует библиотеку multer для обработки загружаемых файлов. Код подключения и реализации логики обработки загружаемых картинок:

```node
const multer = require('multer')

const storage = multer.diskStorage({
  destination(req, file, cb) {
    cb(null, 'images')
  },
  filename(req, file, cb) {
    cb(null, file.originalname)
  }
})

const allowedTypes = ['image/png', 'image/jpg', 'image/jpeg']

const fileFilter = (req, file, cb) => {
  if (allowedTypes.includes(file.mimetype)) {
    cb(null, true)
  } else {
    cb(null, false)
  }
}

module.exports = multer({
  storage, fileFilter
})
```
Где:
- `multer.diskStorage({})` -
- `destination(req, file, cb) {}` -
- `cb(null, 'images')` -
- `filename(req, file, cb) {}` -
- `cb(null, file.originalname)` -
- `allowedTypes = ['image/png', 'image/jpg', 'image/jpeg']` -
- `fileFilter = (req, file, cb) => {}` -
- `if (allowedTypes.includes(file.mimetype)) {}` -
- `cb(null, true)` -
- `cb(null, false)` -
- `multer({storage, fileFilter})` -

Подключение промежуточного обработчика в главный файл index.js
```node
const fileMiddleware = require('./midlleware/file')
app.use(fileMiddleware.single('avatar'))
```
Где:
- `fileMiddleware.single('avatar')` - 

↩️ [К оглавлению раздела](#оглавление-раздела) <br/> 

---

### nodemailer-sendgrid-transport

sendGrid необходим для отправки пользователю писем на почту, которую пользователь вводил при регистрации в следующих ситуациях:


|         Тип письма        |                        Задача письма                         |
| :-----------------------: | :----------------------------------------------------------: |
| Подтверждение регистрации | Уведомить пользователя об подтверждении успешной регистрации |
|   Восстановление пароля   | Прислать токен с ограниченным сроком жизни для смены пароля  |


#### Подключение всех зависимостей nodemailer-sendgrid-transport

nodemailer-sendgrid-transport подключается локально в контроллеры отвечающие за регистрацию и восстановление пароля пользователя. Код подключения сервиса nodemailer и промежуточного обработчика отправки писем nodemailer-sendgrid-transport в файл auth.js:
```node 
const nodemailer = require('nodemailer')
const sendgrid = require('nodemailer-sendgrid-transport')
```
Где:
- `` -
- `` -

Подключение файлов с формой письма с уведомлением и подтверждении успешной регистрации и письма с уведомлением о смене пароля в файл auth.js:
```node
const regEmail = require('../emails/registration')
const resetEmail = require('../emails/reset')
```
Где: 
- `regEmail` -
- `resetEmail` -

Для создание механизма отправки писем необходимо связать сервис nodemailer и промежуточный обработчик sendGrid путём создания транспорта с уникалньым ключём авторизации в сервисе nodemailer. Код подключения уникального ключа nodemailer к промежуточному обработчику sendGrid:
```node
const transporter = nodemailer.createTransport(sendgrid({
  auth: {api_key: keys.SENDGRID_API_KEY}
}))
```
Где:
- `nodemailer.createTransport(sendgrid({}))` -
- `auth: {api_key: keys.SENDGRID_API_KEY}` -

↩️ [К оглавлению раздела](#оглавление-раздела) <br/> 

#### Письмо подтверждающее успешную регистрацию

В случае успешной регистрации контроллер регистрации с методом post по маршруту /auth/register отправляет данные нового пользователя в базу данных, а после отправляет письмо об успешной регистрации на почту, котору указатель пользователь. Часть кода контроллера работающего с промежуточным обработчиком nodemailer-sendgrid-transport:
```node
router.post('/register', registerValidators, async (req, res) => {
    const {email, password, name} = req.body
    ...
    await user.save()
    await transporter.sendMail(regEmail(email))
    ...
})
```
Где: 
- `router.post('/register', registerValidators, async (req, res) => {})` -
- `{email, password, name}` -
- `user.save()` -
- `transporter.sendMail(regEmail(email))` -

Код формы письма успешной регистрации: 
```node
module.exports = function(email) {
  return {
    to: email,
    from: keys.EMAIL_FROM,
    subject: 'Аккаунт создан',
    html: `
      <h1>Добро пожаловать в наш магазин</h1>
      <p>Вы успешно создали аккаунт c email - ${email}</p>
      <hr />
      <a href="${keys.BASE_URL}">Магазин курсов</a>
    `
  }
}
```
Где: 
- `function(email) {}` - 
- `return {}` - 
- `to: email` - 
- `from: keys.EMAIL_FROM` - 
- `subject: 'Аккаунт создан'` - 
- `html: ...` - 

Подробнее о данном контролере смотрите [Метод запроса post по маршруту '/auth/register'](controllers.md#метод-запроса-post-по-маршруту-authregister). <br/>
↩️ [К оглавлению раздела](#оглавление-раздела) <br/> 

#### Письмо смены пароля

В случае успешной регистрации контроллер регистрации с методом post по маршруту /auth/reset отправляет токен для сброса пароля, а после отправляет письмо с токеном сброса пароля на почту пользователя. Часть кода контроллера работающего с промежуточным обработчиком nodemailer-sendgrid-transport:
```node
router.post('/reset', (req, res) => {
    ...
    const token = buffer.toString('hex')
    const candidate = await User.findOne({email: req.body.email})
    ...
    await transporter.sendMail(resetEmail(candidate.email, token))
    ...
})
```
Где:
- `router.post('/reset', (req, res) => {})` -
- `buffer.toString('hex')` -
- `User.findOne({email: req.body.email})` -
- `transporter.sendMail(resetEmail(candidate.email, token))` -

Код формы письма сброса пароля: 
```node
module.exports = function(email, token) {
    return {
        to: email,
        from: keys.EMAIL_FROM,
        subject: 'Восстановление доступа',
        html: `
          <h1>Вы забыли пароль?</h1>
          <p>Если нет, то проигнорируйте данное письмо</p>
          <p>Иначе нажмите на ссылку ниже: </p>
          <p><a href="${keys.BASE_URL}/auth/password/${token}">Восстановить пароль</a></p>
          <hr />
          <a href="${keys.BASE_URL}">Магазин курсов</a>
        `
      }
}
```
Где: 
- `function(email, token) {}` - 
- `return {}` - 
- `to: email` -
- `from: keys.EMAIL_FROM` -
- `subject: 'Восстановление доступа'` -
- `html: ...` - 

Подробнее о данном контролере смотрите [Метод запроса post по маршруту '/auth/reset'](controllers.md#метод-запроса-post-по-маршруту-authreset). <br/>
↩️ [К оглавлению раздела](#оглавление-раздела) <br/> 

---

## Реализация самописных промежуточных обработчиков

### auth

auth необходим для проверки сессии пользователя. Код промежуточного обработчика:
```node
module.exports = function(req, res, next) {
    if (!req.session.isAuthenticated) {
        return res.redirect('/auth/login')
    }

    next()
}
```
Где:
- `if (!req.session.isAuthenticated) {}`
- `return res.redirect('/auth/login')`
- `next()`

Подключение промежуточного обработчика происходит локально в соответствующие машрутизаторы. Перечень всех страниц с типами запроса и машрутизаторами показан в таблице "Все страницы с типами запросами и маршрутизаторами которые обрабатываются промежуточным обработчиком auth".

*Все страницы с типами запросами и маршрутизаторами которые обрабатываются промежуточным обработчиком auth* <br/>
|Название страницы| Метод запроса |    URL-путь запроса   |                                     Краткое описание                                     |
| :-------------: | :---------: | :---------------------: | :--------------------------------------------------------------------------------------: |
|     Главная     |     get     |           "/"           |                               Получение страницы "Главная"                               |
|      Курсы      |     get     |        "/courses"       |               Получение страницы "Курсы" и получение всех курсов магазина                |
|      Курсы      |     get     |      "/courses/:id"     |                            Получение одного курса из магазина                            |
|      Курсы      |     get     |   "/courses/:id/edit"   |                     Получение страницы "Редактирование одного курса"                     |
|      Курсы      |     post    |     "/courses/edit"     |                       Отправка редактируемых данных старого курса                        |
|      Курсы      |     post    |    "/courses/remove"    |                              Удаление существующего курса                                |
|  Добавить курс  |     get     |         "/add"          |                            Получение страницы "Добавить курс"                            |
|  Добавить курс  |     post    |         "/add"          |   Отправка и сохранение одного курса и последующего перенаправления на страницу курсов   |
|     Профиль     |     get     |       "/profile"        |                          Получение страницы профиля пользователя                         |
|     Профиль     |     post    |       "/profile"        |                    Отправка редактируемых данных профиля пользователя                    |
|     Корзина     |     get     |         "/card"         |                       Получение страницы "Корзина" и всех товаров                        |
|     Корзина     |     post    |       "/card/add"       |       Добавление товара в "Корзину" c помощью клавиши "Купить" на странице "Курсы"       |
|     Корзина     |    delete   |    "/card/remove/:id"   |                                   Удаление одного курса                                  |
|     Заказы      |     get     |        "/orders"        |                               Получение страницы "Заказы"                                |
|     Заказы      |     post    |        "/orders"        | Отправка данных на страницу "Заказы" через клавишу "Сделать заказ" на странице "Корзина" |

Интеграция промежуточного обработчика auth в машрутизаторы выглядит следующим образом:
```node
router.post('/', auth,  async (req, res) => {})
```
Где: 
- `router.post()` -
  - `'/'` -
  - `auth` -
  - ` async (req, res) => {}` -

Подробнее о контроллерах смотрите раздел [Контроллеры](controllers.md#контроллеры). <br/>
Подробнее о маршрутизаторах смотрите раздел [Маршрутизация](routes.md#маршрутизация). <br/>
↩️ [К оглавлению раздела](#оглавление-раздела) <br/> 

---

### errorHandler

errorHandler необходим для перенаправление на страницу 404, если пользователь делает get запрос на несуществующую страницу. Код промежуточного обработчика errorHandler:
```node
module.exports = function(req, res, next) {
    res.status(404).render('404', {
        title: 'Страница не найдена'
    })
}
```
Где:
- `function(req, res, next) {}` -
- `res.status(404)` -
- `render('404', {}` -
- `title: 'Страница не найдена'` -

Подключение промежуточного обработчика errorHandler в главный файл index.js
```node
const errorHandler = require('./midlleware/error')
app.use(errorHanler)
```

Важно! обработчик должен быть подключён **после** маршрутизаторов (порядок подключения модулей машрутизаторов и промежуточного обработчика errorHandler в файл index.js неважен), чтобы сервер верно считывал код существующий страниц:
```node
...
app.use('/', homeRoutes)
app.use('/add', addRoutes)
app.use('/courses', coursesRoutes)
app.use('/card', cardRoutes)
app.use('/orders', ordersRoutes)
app.use('/auth', authRoutes)
app.use('/profile', profileRoutes)

app.use(errorHanler)
```
Где: 
- `app.use('/', homeRoutes)` -
- `app.use(errorHanler)` - 

↩️ [К оглавлению раздела](#оглавление-раздела) <br/> 

---

### registerValidators

registerValidators валидирует данные, вводимые пользователем во время регистрации нового аккаунта, поскольку каждое поле имеет свои собственные условия валидации, то registerValidators это массив состоящий из объектов с различными свойствами:
```node
registerValidators = []
```

#### Валидация регистрируемой почты

Код, отвечающий за валидацию регистрируемой почты
```node
    body('email')
    .isEmail()
    .withMessage('Введите корректный email')
    .custom(async (value, req) => {
        try {
            const user = await User.findOne({email: value})
            if (user) {
                return Promise.reject('Такой email уже занят')
            }
        } catch (e) {
            console.log(e)
        }
      .normalizeEmail()
    })
```
Где:
- `body('email')` -
- `.isEmail()` -
- `.withMessage('Введите корректный email')` -
- `.custom(async (value, req) => {})` -
- `User.findOne({email: value})` -
- `Promise.reject('Такой email уже занят')` -
- `.normalizeEmail()` - 

#### Валидация регистрируемого пароля

Код, отвечающий за валидацию регистрируемого пароля:
```node
...
    body('password', 'Пароль должен быть минимум 6 символов')
    .isLength({min: 6, max: 56})
    .isAlphanumeric()
    .trim()
...
```
Где:
- `body('password', 'Пароль должен быть минимум 6 символов')` -
- `.isLength({min: 6, max: 56})` -
- `.isAlphanumeric()` -
- `.trim()` -

#### Валидация подтверждения пароля

Код отвечающий за валидацию подтверждение пароля:
```node
...
    body('confirm')
    .custom((value, {req}) => {
        if (value !== req.body.password) {
            throw new Error('Пароли должны совпадать')
        }
        return true
    })
    .trim()
...
```
Где:
- `body('confirm')` -
- `.custom((value, {req}) => {})` -
- `if (value !== req.body.password) {}` -
- `throw new Error('Пароли должны совпадать')` -
- `.trim()` - 

#### Валидация регистрируемого имени

Кода отвечающий за валидацию регистрируемого имени пользователя:
```node
...
body('name')
    .isLength({min: 3})
    .withMessage('Имя не может быть короче трёх символов')
    .trim()
...
```
Где:
- `body('name')` -
- `.isLength({min: 3})` -
- `.withMessage('Имя не может быть короче трёх символов')` - 
- `.trim()`

#### Подключение промежуточного обработчика registerValidators 

Подключение промежуточного обработчика происходит локально в маршрутизатор регистрации с методом запроса post:
```node
router.post('/register', registerValidators, async (req, res) => {})
```
Где: 
- `router.post()` - 
  - `'/register'` -
  - `registerValidators` -
  - `async (req, res) => {}` -

Подробнее о данном контролере смотрите раздел [Метод запроса post по маршруту '/auth/register'](controllers.md#метод-запроса-post-по-маршруту-authregister). <br/>
↩️ [К оглавлению раздела](#оглавление-раздела) <br/> 

---

### courseValidators

courseValidators валидирует данные вводимые при создании или редактировании одного курса, поскольку каждое поле имеет свои собственные условия валидации, то courseValidators это массив состоящий из объектов с различными свойствами:
```node
courseValidators = [
    body('title').isLength({min: 3}).withMessage('Минимальная длина названия три символа').trim(),
    body('price').isNumeric().withMessage('Введите корректную цену'),
    body('img', 'Введите корректный Url картинки').isURL()
]
```
Где:
- `body('title')` -
  - `.isLength({min: 3})` -
  - `.withMessage('Минимальная длина названия три символа')` -
  - `.trim()` -
- `body('price')` -
  - `.isNumeric()` -
  - `.withMessage('Введите корректную цену')` -
- `body('img', 'Введите корректный Url картинки')` -
  - `.isURL()` -

Подключение промежуточного обработчика происходит локально в соответствующие машрутизаторы. Перечень всех страниц с типами запроса и машрутизаторами показан в таблице "Все страницы с типами запросами и маршрутизаторами которые обрабатываются промежуточным обработчиком courseValidators".

*Все страницы с типами запросами и маршрутизаторами которые обрабатываются промежуточным обработчиком courseValidators* <br/>
|Название страницы| Метод запроса |    URL-путь запроса     |                                     Краткое описание                                     |
| :-------------: | :-----------: | :---------------------: | :--------------------------------------------------------------------------------------: |
|      Курсы      |      post     |     "/courses/edit"     |                       Отправка редактируемых данных старого курса                        |
|  Добавить курс  |      post     |         "/add"          |   Отправка и сохранение одного курса и последующего перенаправления на страницу курсов   |

Интеграция промежуточного обработчика courseValidators в машрутизаторы выглядит следующим образом:
```node
router.post('/', auth,  async (req, res) => {})
```
Где: 
- `router.post()` -
  - `'/'` -
  - `auth` -
  - ` async (req, res) => {}` -

Подробнее о контроллерах смотрите раздел [Контроллеры](controllers.md#контроллеры). <br/>
Подробнее о маршрутизаторах смотрите раздел [Маршрутизация](routes.md#маршрутизация). <br/>
↩️ [К оглавлению раздела](#оглавление-раздела) <br/> 

---

### UserMiddleware

UserMiddleware необходим для организации сесии для одного пользователя. Код предоставления сесии для одного пользователя:

```node
module.exports = async function(req, res, next) {
    if (!req.session.user) {
        return next()
    }
    
    req.user = await User.findById(req.session.user._id)
    next()
}
```
Где:
- `function(req, res, next) {})` -
- `if (!req.session.user) {}` -
- `req.user = await User.findById(req.session.user._id)` -
- `next()` -


Подключение промежуточного обработчика UserMiddleware из файла user.js в главный файл index.js:
```node
const UserMiddleware = require('./midlleware/user')
app.use(UserMiddleware)
```

↩️ [К оглавлению раздела](#оглавление-раздела) <br/> 

---

### varMiddleware

varMiddleware необходим для присванивание значений авторизации, которые в представлениях будут организовывать условия для визуализации тех или иных данных. Код промежуточного обработчика:
```node
module.exports = function(req, res, next) {
    res.locals.isAuth = req.session.isAuthenticated
    res.locals.csrf = req.csrfToken()
    next()
}
```

Подключение промежуточного обработчика varMiddleware из файла variables.js в главный файл index.js:
```node
const varMiddleware = require('./midlleware/variables')
app.use(UserMiddleware)
```

↩️ [К оглавлению раздела](#оглавление-раздела) <br/> 

