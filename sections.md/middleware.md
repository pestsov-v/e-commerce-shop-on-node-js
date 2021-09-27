# Промежуточные обработчики

Промежуточные обработчики - необходимы в большинстве случаев как обработчики авторизации, валидации, аутентификации и других различных проверок. Промежуточные обработчики могут быть как внешними библиотеками, которые подключают глобально, так самописные промежуточные обработчики подключаемые локально в маршрутизаторы. Ниже будет подробно разобраны промежуточные обработчики, которые используются в приложении

⬅️ **Подробную информацию обо всех маршрутах смотреть в разделе [Маршрутизация](routes.md)** <br/> 
⬅️ **Подробную информацию обо всех контроллерах смотреть в разделе [Контроллеры](controllers.md)** <br/> 
↩️ [К оглавлению документации](../README.md) <br/> 

## Оглавление раздела
- [Промежуточные обработчики](#промежуточные-обработчики)
  - [Оглавление раздела](#оглавление-раздела)
  - [Описание промежуточных обработчиков](#описание-промежуточных-обработчиков)
    - [Пример глобального подключения промежуточного обработчика](#пример-глобального-подключения-промежуточного-обработчика)
    - [Пример локального промежуточного обработчика](#пример-локального-промежуточного-обработчика)
    - [Пример самописного промежуточного обработчика](#пример-самописного-промежуточного-обработчика)
    - [Таблица всех промежуточных обработчиков приложения](#таблица-всех-промежуточных-обработчиков-приложения)
  - [Реализация библиотечных промежуточных обработчиков](#реализация-библиотечных-промежуточных-обработчиков)
    - [bcryptjs](#bcryptjs)
      - [Использование bcryptjs в контроллере авторизации с методом запроса post по машруту /auth/login](#использование-bcryptjs-в-контроллере-авторизации-с-методом-запроса-post-по-машруту-authlogin)
      - [Использование bcryptjs в контроллере авторизации с методом запроса post по машруту /auth/register](#использование-bcryptjs-в-контроллере-авторизации-с-методом-запроса-post-по-машруту-authregister)
      - [Использование bcryptjs в контроллере авторизации с методом запроса post по машруту /auth/password](#использование-bcryptjs-в-контроллере-авторизации-с-методом-запроса-post-по-машруту-authpassword)
    - [crypto](#crypto)
    - [helmet](#helmet)
    - [session](#session)
    - [connect-flash](#connect-flash)
      - [Использование connect-flash в контроллере авторизации с методом запроса get по машруту /auth/login](#использование-connect-flash-в-контроллере-авторизации-с-методом-запроса-get-по-машруту-authlogin)
      - [Использование connect-flash в контроллере авторизации с методом запроса get по машруту /auth/reset](#использование-connect-flash-в-контроллере-авторизации-с-методом-запроса-get-по-машруту-authreset)
      - [Использование connect-flash в контроллере авторизации с методом запроса get по машруту /auth/password/:token](#использование-connect-flash-в-контроллере-авторизации-с-методом-запроса-get-по-машруту-authpasswordtoken)
      - [Использование connect-flash в контроллере авторизации с методом запроса post по машруту /auth/login](#использование-connect-flash-в-контроллере-авторизации-с-методом-запроса-post-по-машруту-authlogin)
      - [Использование connect-flash в контроллере авторизации с методом запроса post по машруту /auth/register](#использование-connect-flash-в-контроллере-авторизации-с-методом-запроса-post-по-машруту-authregister)
      - [Использование connect-flash в контроллере авторизации с методом запроса post по машруту /auth/reset](#использование-connect-flash-в-контроллере-авторизации-с-методом-запроса-post-по-машруту-authreset)
      - [Использование connect-flash в контроллере авторизации с методом запроса post по машруту /auth/password](#использование-connect-flash-в-контроллере-авторизации-с-методом-запроса-post-по-машруту-authpassword)
    - [csurf](#csurf)
    - [compression](#compression)
    - [fileMiddleware](#filemiddleware)
  - [Самописные промежуточные обработчики](#самописные-промежуточные-обработчики)
    - [auth](#auth)
    - [errorHandler](#errorhandler)
    - [registerValidators](#registervalidators)
    - [courseValidators](#coursevalidators)
    - [UserMiddleware](#usermiddleware)
    - [varMiddleware](#varmiddleware)
  - [SendGrid](#sendgrid)


## Описание промежуточных обработчиков

Промежуточные обработчики могут быть как внешними библиотеками так и написаны самим разработчиком. Одновременно с этим, сами промежуточные обработчики могут быть подключение к приложению глобально, так и локально, в зависимости от бизнес-логики.

### Пример глобального подключения промежуточного обработчика
```node
const flash = require('connect-flash')
app.use(flash())
```
Где: 
- `connect-flash` - библиотека из npm, которое отвечает за отображение валидированных ошибок.
- `app.use(flash())` - подключение библиотеки connect-flash к приложению.
  - `app.use()` - метод use объекта app, создающий подключение midlleware (промежуточных обработчиков) к приложения. Объект app является результатом работы функции express. Код подключения express ниже:
```node
const express = require('express')
const app = express()
```

---

### Пример локального промежуточного обработчика
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
Где: 
- `bcryptjs` - библиотека из npm, которая шифрует строки.
- `router.post('/login', async (req, res) => {})` - маршрутизатор с методом запроса post, который. Подробнее о данном маршрутизаторе смотрите [Метод запроса post по маршруту '/auth/login'](controllers.md#метод-запроса-post-по-маршруту-authlogin)
- `bcrypt.compare(password, candidate.password)` - метод compare библиотеки bcryptjs, который сравнивает два значение. В данном примере - существующий пароль, который привязан к email пользователя с базы данных и вводимый кандидатом пароль.

---

### Пример самописного промежуточного обработчика
```node
module.exports = function(req, res, next) {
  if (!req.session.isAuthenticated) {
    return res.redirect('/auth/login')
  }

  next()
}
```
Где:
- `function(req, res, next) {})` - промежуточные обработчики отличаются наличием третъего аргумента next, который должен обязательно быть вызван вконце логики промежуточного обработчика.
- `if (!req.session.isAuthenticated) {}` - проверка на авторизацию пользователя.
- `return res.redirect('/auth/login')` - метод redirect объекта ответа response, который перенаправляет пользователя на страницу по маршуту '/auth/login'.
- `next()` - обязательный параметр, определяет завершение логики кода данного промежуточного обработчика.

---

### Таблица всех промежуточных обработчиков приложения

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
|      sendgrid      |  Локально   | Библиотека nodemailer-sendgrid-transport |                       Отрпавка писем по почте                       |
|   fileMiddleware   |  Глобально  |               Файл file.js               |   Обработка загружаемых картинок, которые использутся для аватара   |
|        auth        |  Локально   |               Файл auth.js               |                 Проверка на авторизацию пользователя                |
|    errorHandler    |  Глобально  |              Файл error.js               |                          Вывод страницы 404                         |
| registerValidators |  Локально   |            Файл validators.js            |    Выводит ошибки валидации при регистрации нового пользователя     |
|  courseValidators  |  Локально   |            Файл validators.js            | Выводит ошибки валидации при создании / редактировании одного курса |
|   UserMiddleware   |  Глобально  |               Файл user.js               |          Организация допуска авторизированым пользователям          |
|    varMiddleware   |  Глобально  |             Файл variables.js            |        Создание токена CSRF для авторизированого пользователя       |


↩️ [К оглавлению документации](../README.md) <br/> 

## Реализация библиотечных промежуточных обработчиков

---

### bcryptjs

bcryptjs позволяет хешировать пароли. Поскольку манипуляции с пароля в приложении производятся в контроллерах авторизации, то и следовательно модуль подключается лишь здесь. Код подключения bcryptjs:
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
Где:
- `router.post('/login', async (req, res) => {})` - метод запроса post объекта router, который принимает 2 аргумента: путь '/login' и callback функцию.
- `{email, password}` - конкретные ключи деструктуризированные с тела объекта запроса request.body.
- `User.findOne({ email })` - поиске пользователя в базе данных MongoDB по его email.
- `bcrypt.compare(password, candidate.password)` - метод compare библиотеки bcryptjs, который сравнивает существующий пароль, который привязан к email пользователя с базы данных и вводимый кандидатом пароль.

Подробнее о данном контролере смотрите [Метод запроса post по маршруту '/auth/login'](controllers.md#метод-запроса-post-по-маршруту-authlogin). <br/>
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
Где:
- `router.post('/register', registerValidators, async (req, res) => {})` - метод запроса post объекта router, который принимает 3 аргумента: путь '/register', промежуточный обработчик registerValidators и callback функцию.
  - `registerValidators` - промежуточный обработчик валидирующий ошибки при регистрации. Подробнее о данном промежуточном обработчике смотрите [registerValidators](#registervalidators).
- `{email, password, name}` - конкретные ключи деструктуризированные с тела объекта запроса request.body.
- `bcrypt.hash(req.body.password, 10)` - метод hash библиотеки bcrypt, который принимает 2 аргумента:
  - `req.body.password` - вводимый пользователем пароль.
  - `10` - количество хешируемых символов.

Подробнее о данном контролере смотрите [Метод запроса post по маршруту '/auth/register'](controllers.md#метод-запроса-post-по-маршруту-authregister). <br/>
Подробнее о реализации машрутизатора смотрите [Реализация маршрутов](routes.md#реализация-маршрутов). <br/>

---

#### Использование bcryptjs в контроллере авторизации с методом запроса post по машруту /auth/password

Библиотека в данном контролере используется для хеширования вводимого пароля. Часть кода контроллера работающий с промежуточным обработчиком: <br/>
```node
router.post('/password', async (req, res) => {
  ...
  if (user) {
    user.password = await bcrypt.hash(req.body.password, 10)
    ...
})
```
Где: 
- `router.post('/password', async (req, res) => {})` - метод запроса post объекта router, который принимает 2 аргумента: путь '/password' и callback функцию.
- `bcrypt.hash(req.body.password, 10)` - метод hash библиотеки bcrypt, который принимает 2 аргумента:
  - `req.body.password` - вводимый пользователем пароль.
  - `10` - количество хешируемых символов.

Подробнее о данном контролере смотрите [Метод запроса post по маршруту '/auth/password'](controllers.md#метод-запроса-post-по-маршруту-authpassword). <br/>
Подробнее о реализации машрутизатора смотрите [Реализация маршрутов](routes.md#реализация-маршрутов). <br/>

---

### crypto 

crypto необходим для генерации криптографически хорошо построенных искусственных случайных данных и количества байтов, которые в последствии будут отправлятся пользователю на почту для защиты при процедуре смены пароля пользователем. Код подключение и использование crypto:
```node
const crypto = require('crypto')
...
router.post('/reset', (req, res) => {
    crypto.randomBytes(32, async (err, buffer) => {})
...
})
```
Где:
- `crypto` - подключаем библиотеку crypto.
- `router.post('/reset', (req, res) => {})` - метод запроса post объекта router, который принимает 2 аргумента: путь '/reset' и callback функцию.
- `crypto.randomBytes(32, async (err, buffer) => {})` - метод randomBytes библиотеке crypto, который генерирует хорошо построенные криптографические искусственные данные.

Подробнее о данном контролере смотрите [Метод запроса post по маршруту '/auth/reset'](controllers.md#метод-запроса-post-по-маршруту-authreset). <br/>
Подробнее о реализации машрутизатора смотрите [Реализация маршрутов](routes.md#реализация-маршрутов). <br/>

---

### helmet 

helmet помогает защитить приложение от некоторых широко известных веб-уязвимостей путем соответствующей настройки заголовоков HTTP. Код отвечающий за настройку и подключение helmet:

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
Где: 
- `helmet` - оболочка для различных промежуточных обработчиков, которые установлены по умолчанию. 
- `contentSecurityPolicy: {})` - промежуточный обработчик, который устанавливает специфические настройки заголовка `content-Security-Policy`. 
- `directives` - объект, где ключи - это название директив. 
- `...helmet.contentSecurityPolicy.getDefaultDirectives()` - объект директив по умолчанию. 
- `"img-src": ["'self'", "https:"]` - ряд параметров для защиты подключаемых картинок.
- `"script-src-elem": ["'self'", "https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/js/materialize.min.js", "'unsafe-inline'" ]` - ряд параметром для защиты подключаемых скриптов.

### session

express-session создаёт объект сессии, данные которое хранятся на стороне сервера. Код, отвечающий за подключение и настройку express-session:
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
Где:
- `session` - оборолочка принимающая ряд параметров настройки session.
- `secret: keys.SESSION_SECRET` - 
- `resave: false` - принудительное сохранение сеанса в хранилище сессий, даже если сеанс не изменялся во время запроса. Определяем значение `false` во избежания ошибок в случае параллельных запросов пользователем.
- `saveUninitialized: false` - сохранение неинициализированных сеансов в хранилище сессий. Определяем значение `false` во избежания ошибок в случае параллельных запросов пользователем и уменьшения использования хранилище сервера. 
- `store` - ключ принимающий одноименное значение объекта `store`. Код объекта store указан ниже:
```node
const MongoStore = require('connect-mongodb-session')(session)
...
const store = new MongoStore({
  collection: 'sessions',
  uri: keys.MONGODB_URL
})
```
Где:
- `MongoStore({})` - объект устанавливающий соединение express приложения и хранилище сессий в MongoDB.
- `collection: 'sessions'` - название коллекции сессий в базе данных MongoDB.
- `uri: keys.MONGODB_URL` - URL путь соединения с базой данных MongoDB.


### connect-flash

connect-flash необходим как специальная область сеанса, которая используется для хранения сообщений. Сообщения используется в связке с перенаправлениями во время действий авторизации. Сам connect-flash подключается в главном файле index.js после чего используется локально в контроллерах авторизации. Подробнее о контроллерах авторизации смотрите [Контроллеры страницы "Вход / Выход"](#контроллеры-страницы-вход--выход). Код подключения connect-flash в главном файле index.js:
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
Где:
- `router.get('/login', async (req, res) => {})` -  метод запроса get объекта router, который принимает 2 аргумента: путь '/login' и callback функцию.
- `res.render('auth/login', {})` - метод render объекта res отправляет данные в объекте ответа для последующей визуализации представления шаблонизатором handlebars.
- `loginError: req.flash('loginError')` - ключ loginError принимающий значение сообщения ошибки валидации приходящее с телом запроса req. В зависимости от данных введённых пользователем, может вывести текст "Неверный пароль" или "Такого пользователя не существует". Часть кода представления работающий с валидатором:
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
Подробнее о представлении, которое использует данные переданные вместе с методом render объекта ответа response смотрите [Вход / Регистрация](views.md#вход--регистрация). <br/>
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
Подробнее о представлении, которое использует данные переданные вместе с методом render объекта ответа response смотрите [Вход / Регистрация](views.md#вход--регистрация). <br/>
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
Подробнее о представлении, которое использует данные переданные вместе с методом render объекта ответа response смотрите [Вход / Регистрация](views.md#вход--регистрация). <br/>
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
Подробнее о представлении, которое использует данные переданные вместе с методом render объекта ответа response смотрите [Вход / Регистрация](views.md#вход--регистрация). <br/>
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
- `(!errors.isEmpty()) {}` - если поле валидации не является не пустым выполняется тело условия.
- `req.flash('registerError', errors.array()[0].msg)` - поскольку валидатор ошибки у нас проверяется в представлении, то текст ошибки нам не обязательно делать смысловым на стороне сервера. На стороне сервера в случае ошибки валидации будет выводится первое значение встроенных текстов в объекте errors, которое приведённое к массиву, чтобы забрать текст ошибки по его индексу. Сам текст ошибки выглядит так: "Invalid value".
- `return res.status(422).redirect('/auth/login#register')` - метод redirect объекта res перенаправляет пользователя на страницу по маршруту /auth/login#register.

Подробнее о данном контролере смотрите [Метод запроса post по маршруту '/auth/register'](controllers.md#метод-запроса-post-по-маршруту-authregister). <br/>
Подробнее о представлении, которое использует данные переданные вместе с методом render объекта ответа response смотрите [Вход / Регистрация](views.md#вход--регистрация). <br/>
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
Подробнее о представлении, которое использует данные переданные вместе с методом render объекта ответа response смотрите [Вход / Регистрация](views.md#вход--регистрация). <br/>
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
Подробнее о представлении, которое использует данные переданные вместе с методом render объекта ответа response смотрите [Вход / Регистрация](views.md#вход--регистрация). <br/>
Подробнее о реализации машрутизатора смотрите [Реализация маршрутов](routes.md#реализация-маршрутов). <br/>

---



### csurf

csrf используется для защиты от подделки межасайтовых запросов (CSRF). Код подключения connect-flash в главный файл index.js:
```node
const csrf = require('csurf')
app.use(csrf())
```
Использование csrf защиты выходит путём подключения скритого обработчика csrf в шаблони handlebars. Использование защиты в представлении add.hbs с методом запроса post по машруту /add:
```handlebars
<form action="/add" method="POST">
  ...
  <input type="hidden" name="_csrf" value="{{csrf}}">
  ...
</form>  
```
Где: 
- `form action="/add" method="POST"` - форма контроллера с методом запроса post по машруту /add.
- <input type="hidden" name="_csrf" value="{{csrf}}"> - промежуточный обработчик csurf не отображающийся в представлении, но при этом обеспечивает создание токена. Этот токен проверяется по сеансу пользователя. 



card.hbs
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


course-edit.hbs
```handlebars
<form action="/courses/edit" method="POST" class="course-form">
  <input type="hidden" name="_csrf" value="{{csrf}}">
</form>
```
```handlebars
<form action="/courses/remove" method="POST">
 <input type="hidden" name="_csrf" value="{{csrf}}">
</form>
```

courses.hbs
```handlebars
{{#if @root.isAuth}}
  <form action="/card/add" method="POST">
    <input type="hidden" name="_csrf" value="{{@root.csrf}}"> 
  </form>
{{/if}}
```

profile.hbs
```handlebars
<form action="/profile" method="POST" enctype="multipart/form-data">
  <input type="hidden" name="_csrf" value="{{csrf}}">
</form>
```

auth/login.hbs
```handlebars
<form action="/auth/login" method="POST">
  <input type="hidden" name="_csrf" value="{{csrf}}">
</form>
```

```handlebars
<form action="/auth/register" method="POST" novalidate>
  <input type="hidden" name="_csrf" value="{{csrf}}">
</form>
```

auth/password.hbs
```handlebars
 <form action="/auth/password" method="POST">
  <input type="hidden" name="_csrf" value="{{csrf}}">
</form>
```

auth/password.hbs
```handlebars
 <form action="/auth/reset" method="POST">
  <input type="hidden" name="_csrf" value="{{csrf}}">
</form>
```



### compression

```node
app.use(compression())
```

### fileMiddleware



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



## Самописные промежуточные обработчики

### auth

```node
module.exports = function(req, res, next) {
    if (!req.session.isAuthenticated) {
        return res.redirect('/auth/login')
    }

    next()
}
```

### errorHandler

```node
module.exports = function(req, res, next) {
    res.status(404).render('404', {
        title: 'Страница не найдена'
    })
}
```

### registerValidators


```node
registerValidators = []
```

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
      .normalizeEmail(),
    })
```

```node
    body('password', 'Пароль должен быть минимум 6 символов')
    .isLength({min: 6, max: 56})
    .isAlphanumeric()
    .trim(),
```

```node
    body('confirm')
    .custom((value, {req}) => {
        if (value !== req.body.password) {
            throw new Error('Пароли должны совпадать')
        }
        return true
    })
    .trim()
```

```node
...
body('name')
    .isLength({min: 3})
    .withMessage('Имя не может быть короче трёх символов')
    .trim()
...
```



### courseValidators

```node
courseValidators = [
    body('title').isLength({min: 3}).withMessage('Минимальная длина названия три символа').trim(),
    body('price').isNumeric().withMessage('Введите корректную цену'),
    body('img', 'Введите корректный Url картинки').isURL()
]
```


### UserMiddleware

module.exports = async function(req, res, next) {
    if (!req.session.user) {
        return next()
    }
    
    req.user = await User.findById(req.session.user._id)
    next()
}

### varMiddleware


```node
module.exports = function(req, res, next) {
    res.locals.isAuth = req.session.isAuthenticated
    res.locals.csrf = req.csrfToken()
    next()
}
```









-----------------------------------------------------------------------------------------------------------------------------------------------------------------

## SendGrid


Все почтовые письма: 
|         Тип письма        |                        Задача письма                         |
| :-----------------------: | :----------------------------------------------------------: |
| Подтверждение регистрации | Уведомить пользователя об подтверждении успешной регистрации |
|   Восстановление пароля   | Прислать токен с ограниченным сроком жизни для смены пароль  |

```node
const nodemailer = require('nodemailer')
const sendgrid = require('nodemailer-sendgrid-transport')
```

```node
const regEmail = require('../emails/registration')
const resetEmail = require('../emails/reset')
```

regEmail
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

resetEmail
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


```node
const transporter = nodemailer.createTransport(sendgrid({
  auth: {api_key: keys.SENDGRID_API_KEY}
}))
```



auth.js route
router post register
```node
router.post('/register', registerValidators, async (req, res) => {
    const {email, password, name} = req.body
    ...
    await transporter.sendMail(regEmail(email))
    ...
})
```

auth.js route
router post reset

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



-----------------------------------------------------------------------------------------------------------------------------------------------------------------
