# Маршрутизация



⬅️ **Подробную информацию обо всех контроллерах смотреть в разделе [Контроллеры](routes.md)**

## Оглавление раздела
- [Маршрутизация](#маршрутизация)
  - [Оглавление раздела](#оглавление-раздела)
    - [Структура маршрутизации](#структура-маршрутизации)
      - [Подключение всех маршрутизаторов в главный файл `index.js`](#подключение-всех-маршрутизаторов-в-главный-файл-indexjs)
    - [Таблица всех маршрутизаторов](#таблица-всех-маршрутизаторов)
    - [Маршрутизаторы страницы "Главная"](#маршрутизаторы-страницы-главная)
    - [Маршрутизаторы страницы "Курсы"](#маршрутизаторы-страницы-курсы)
    - [Маршрутизаторы страницы "Добавить курс"](#маршрутизаторы-страницы-добавить-курс)
    - [Маршрутизаторы страницы "Профиль"](#маршрутизаторы-страницы-профиль)
    - [Маршрутизаторы страницы "Корзина"](#маршрутизаторы-страницы-корзина)
    - [Маршрутизаторы страницы "Заказы"](#маршрутизаторы-страницы-заказы)
    - [Маршрутизаторы страницы "Вход / Выход"](#маршрутизаторы-страницы-вход--выход)


### Структура маршрутизации

Все маршрутизаторы подключаются через главный файл `index.js`, а каждый маршрутизатор формирует перенаправление контроллеров по своей странице. Описание каждого маршрутизатора описано в подразделах. 

#### Подключение всех маршрутизаторов в главный файл `index.js`
Подключение модулей маршрутизаторов в главный фалй `index.js`:
```node
const homeRoutes = require('./routes/home')
const cardRoutes = require('./routes/card')
const addRoutes = require('./routes/add')
const ordersRoutes = require('./routes/orders')
const coursesRoutes = require('./routes/courses')
const authRoutes = require('./routes/auth')
const profileRoutes = require('./routes/profile')
```

Подключение маршрутизаторов в приложение:
```node
app.use('/', homeRoutes)
app.use('/add', addRoutes)
app.use('/courses', coursesRoutes)
app.use('/card', cardRoutes)
app.use('/orders', ordersRoutes)
app.use('/auth', authRoutes)
app.use('/profile', profileRoutes)
```
Где:
- `app.use('/', homeRoutes)` - 
- `app.use('/add', addRoutes)` - 
- `app.use('/courses', coursesRoutes)` - 
- `app.use('/card', cardRoutes)` - 
- `app.use('/orders', ordersRoutes)` - 
- `app.use('/auth', authRoutes)` - 
- `app.use('/profile', profileRoutes)` - 

⬆️ [К оглавлению раздела "Маршрутизация"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/>

### Таблица всех маршрутизаторов

Все запросы ко всем страницам приложения:
|Название страницы| Тип запроса |    URL-путь запроса     |       middleware       |                                     Краткое описание                                     |
| :-------------: | :---------: | :---------------------: | :--------------------: | :--------------------------------------------------------------------------------------: |
|     Главная     |     get     |           "/"           |           -            |                               Получение страницы "Главная"                               |
|      Курсы      |     get     |        "/courses"       |           -            |               Получение страницы "Курсы" и получение всех курсов магазина                |
|      Курсы      |     get     |      "/courses/:id"     |           -            |                            Получение одного курса из магазина                            |
|      Курсы      |     get     |   "/courses/:id/edit"   |          auth          |                     Получение страницы "Редактирование одного курса"                     |
|      Курсы      |     post    |     "/courses/edit"     | auth, courseValidators |                       Отправка редактируемых данных старого курса                        |
|      Курсы      |     post    |    "/courses/remove"    |          auth          |                              Удаление существующего курса                                |
|  Добавить курс  |     get     |         "/add"          |          auth          |                            Получение страницы "Добавить курс"                            |
|  Добавить курс  |     post    |         "/add"          | auth, courseValidators |   Отправка и сохранение одного курса и последующего перенаправления на страницу курсов   |
|     Профиль     |     get     |       "/profile"        |          auth          |                          Получение страницы профиля пользователя                         |
|     Профиль     |     post    |       "/profile"        |          auth          |                    Отправка редактируемых данных профиля пользователя                    |
|     Корзина     |     get     |         "/card"         |          auth          |                       Получение страницы "Корзина" и всех товаров                        |
|     Корзина     |     post    |       "/card/add"       |          auth          |       Добавление товара в "Корзину" c помощью клавиши "Купить" на странице "Курсы"       |
|     Корзина     |    delete   |    "/card/remove/:id"   |          auth          |                                   Удаление одного курса                                  |
|     Заказы      |     get     |        "/orders"        |          auth          |                               Получение страницы "Заказы"                                |
|     Заказы      |     post    |        "/orders"        |          auth          | Отправка данных на страницу "Заказы" через клавишу "Сделать заказ" на странице "Корзина" |
|   Вход / выход  |     get     |       "/auth/login"     |           -            |                   Отправка данных пользователя для входа в приложение                    |
|   Вход / выход  |     get     |       "/auth/loguot"    |           -            |                    Выход из приложения и закрытия сессии пользователя                    |
|   Вход / выход  |     get     |       "/auth/reset"     |           -            |                         Получение страницы восстановление пароля                         |
|   Вход / выход  |     get     | "/auth/password/:token" |           -            |                         Получение токена на восстановление пароля                        |
|   Вход / выход  |     post    |       "/auth/login"     |           -            |                   Отправка данных пользователя для входа в приложение                    |
|   Вход / выход  |     post    |      "/auth/register"   |    courseValidators    |           Отправка регистрационных данных для получения доступа в приложение             |
|   Вход / выход  |     post    |       "/auth/reset"     |           -            |       Отправка ранее регистрируемой почты для восстановления доступа в приложение        |
|   Вход / выход  |     post    |     "/auth/password"    |           -            |                   Отправка пароля пользователя для доступа в приложение                  |


⬆️ [К оглавлению раздела "Маршрутизация"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/>

### Маршрутизаторы страницы "Главная"

Все запросы страницы "Главная"
|Название страницы| Тип запроса |    URL-путь запроса     |       middleware       |                                     Краткое описание                                     |
| :-------------: | :---------: | :---------------------: | :--------------------: | :--------------------------------------------------------------------------------------: |
|     Главная     |     get     |           "/"           |           -            |                               Получение страницы "Главная"                               |


Блок кода
```node
const {Router} = require('express')
const router = Router()

router.get('/', (req, res) => {})

module.exports = router
```
Где:
- `Router` -
- `router` -
- `router.get('/', (req, res) => {})` -


⬆️ [К оглавлению раздела "Маршрутизация"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/>

### Маршрутизаторы страницы "Курсы"

Все запросы страницы "Курсы"
|Название страницы| Тип запроса |    URL-путь запроса     |       middleware       |                                     Краткое описание                                     |
| :-------------: | :---------: | :---------------------: | :--------------------: | :--------------------------------------------------------------------------------------: |
|      Курсы      |     get     |        "/courses"       |           -            |               Получение страницы "Курсы" и получение всех курсов магазина                |
|      Курсы      |     get     |      "/courses/:id"     |           -            |                            Получение одного курса из магазина                            |
|      Курсы      |     get     |   "/courses/:id/edit"   |          auth          |                     Получение страницы "Редактирование одного курса"                     |
|      Курсы      |     post    |     "/courses/edit"     | auth, courseValidators |                       Отправка редактируемых данных старого курса                        |
|      Курсы      |     post    |    "/courses/remove"    |          auth          |                              Удаление существующего курса                                |

Блок кода:
```node
const {Router} = require('express')
const Course = require('../models/course')
const auth = require('../midlleware/auth')
const {validationResult} = require ('express-validator')
const {courseValidators} = require('../utils/validators')
const router = Router()

router.get('/', async (req, res) => {})
router.get('/:id/edit', auth, async (req, res) => {})
router.post('/edit', auth, courseValidators, async (req, res) => {})
router.post('/remove', auth, async (req, res) => {})
router.get('/:id', async (req, res) => {})

module.exports = router
```
Где:
- `Router` -
- `Course` - `require('../models/course')`
- `auth` - `require('../midlleware/auth')`
- `validationResult` -
- `courseValidators` -
- `router` -
- `router.get('/', async (req, res) => {})` -
- `router.get('/:id/edit', auth, async (req, res) => {})` -
- `router.post('/edit', auth, courseValidators, async (req, res) => {})` -
- `router.post('/remove', auth, async (req, res) => {})` -
- `router.get('/:id', async (req, res) => {})` -


⬆️ [К оглавлению раздела "Маршрутизация"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/>

### Маршрутизаторы страницы "Добавить курс"

Все запросы страницы "Добавить курс"
|Название страницы| Тип запроса |    URL-путь запроса     |       middleware       |                                     Краткое описание                                     |
| :-------------: | :---------: | :---------------------: | :--------------------: | :--------------------------------------------------------------------------------------: |
|  Добавить курс  |     get     |         "/add"          |          auth          |                            Получение страницы "Добавить курс"                            |
|  Добавить курс  |     post    |         "/add"          | auth, courseValidators |   Отправка и сохранение одного курса и последующего перенаправления на страницу курсов   |

Блок кода:
```node
const {Router} = require('express')
const Course = require('../models/course')
const auth = require('../midlleware/auth')
const {validationResult} = require ('express-validator')
const {courseValidators} = require('../utils/validators')
const router = Router()

router.get('/', auth, (req, res) => {})
router.post('/', auth, courseValidators, async (req, res) => {})

module.exports = router
```
Где:
- `Router` -
- `Course` - `require('../models/course')`
- `auth` - `require('../midlleware/auth')`
- `validationResult` -
- `router` -
- `router.get('/', auth, (req, res) => {})` -
- `router.post('/', auth, courseValidators, async (req, res) => {})` -


⬆️ [К оглавлению раздела "Маршрутизация"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/>

### Маршрутизаторы страницы "Профиль"

Все запросы страницы "Профиль"
|Название страницы| Тип запроса |    URL-путь запроса     |       middleware       |                                     Краткое описание                                     |
| :-------------: | :---------: | :---------------------: | :--------------------: | :--------------------------------------------------------------------------------------: |
|     Профиль     |     get     |       "/profile"        |          auth          |                          Получение страницы профиля пользователя                         |
|     Профиль     |     post    |       "/profile"        |          auth          |                    Отправка редактируемых данных профиля пользователя                    |

Блок кода:
```node
const {Router} = require('express')
const auth = require('../midlleware/auth')
const User = require('../models/user')
const router = Router()

router.get('/', auth, async (req, res) => {})
router.post('/', auth, async (req, res) => {})

module.exports = router
```
Где:
- `Router` -
- `auth` - `require('../midlleware/auth')`
- `User` - `require('../models/user')`
- `router` -
- `router.get('/', auth, async (req, res) => {})` -
- `router.post('/', auth, async (req, res) => {})` -

⬆️ [К оглавлению раздела "Маршрутизация"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/>

### Маршрутизаторы страницы "Корзина"

Все запросы страницы "Корзина"
|Название страницы| Тип запроса |    URL-путь запроса     |       middleware       |                                     Краткое описание                                     |
| :-------------: | :---------: | :---------------------: | :--------------------: | :--------------------------------------------------------------------------------------: |
|     Корзина     |     get     |         "/card"         |          auth          |                       Получение страницы "Корзина" и всех товаров                        |
|     Корзина     |     post    |       "/card/add"       |          auth          |       Добавление товара в "Корзину" c помощью клавиши "Купить" на странице "Курсы"       |
|     Корзина     |    delete   |    "/card/remove/:id"   |          auth          |                                   Удаление одного курса                                  |

Блок кода:
```node
const {Router} = require('express')
const Course = require('../models/course')
const auth = require('../midlleware/auth')
const router = Router()

router.post('/add', auth, async (req, res) => {})
router.delete('/remove/:id', auth, async (req, res) => {})
router.get('/', auth, async (req, res) => {})

module.exports = router
```
Где:
- `Router` -
- `Course` - `require('../models/course')`
- `auth` - `require('../midlleware/auth')`
- `router` -
- `router.post('/add', auth, async (req, res) => {})` -
- `router.delete('/remove/:id', auth, async (req, res) => {})` -
- `router.get('/', auth, async (req, res) => {})` -


⬆️ [К оглавлению раздела "Маршрутизация"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/>

### Маршрутизаторы страницы "Заказы"

Все запросы страницы "Заказы"
|Название страницы| Тип запроса |    URL-путь запроса     |       middleware       |                                     Краткое описание                                     |
| :-------------: | :---------: | :---------------------: | :--------------------: | :--------------------------------------------------------------------------------------: |
|     Заказы      |     get     |        "/orders"        |          auth          |                               Получение страницы "Заказы"                                |
|     Заказы      |     post    |        "/orders"        |          auth          | Отправка данных на страницу "Заказы" через клавишу "Сделать заказ" на странице "Корзина" |

Блок кода:
```node
const {Router} = require('express')
const Order = require('../models/order')
const auth = require('../midlleware/auth')
const router = Router()

router.get('/', auth, async (req, res) => {})
router.post('/', auth, async (req, res) => {})

module.exports = router
```
Где:
- `Router` -
- `Order` - `require('../models/order')`
- `auth` - `require('../midlleware/auth')`
- `router` -
- `router.get('/', auth, async (req, res) => {})` -
- `router.post('/', auth, async (req, res) => {})` -


⬆️ [К оглавлению раздела "Маршрутизация"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/>

### Маршрутизаторы страницы "Вход / Выход"

Все запросы страницы "Войти / выйти"
|Название страницы| Тип запроса |    URL-путь запроса     |       middleware       |                                     Краткое описание                                     |
| :-------------: | :---------: | :---------------------: | :--------------------: | :--------------------------------------------------------------------------------------: |
|   Вход / выход  |     get     |       "/auth/login"     |           -            |                   Отправка данных пользователя для входа в приложение                    |
|   Вход / выход  |     get     |       "/auth/loguot"    |           -            |                    Выход из приложения и закрытия сессии пользователя                    |
|   Вход / выход  |     get     |       "/auth/reset"     |           -            |                         Получение страницы восстановление пароля                         |
|   Вход / выход  |     get     | "/auth/password/:token" |           -            |                         Получение токена на восстановление пароля                        |
|   Вход / выход  |     post    |       "/auth/login"     |           -            |                   Отправка данных пользователя для входа в приложение                    |
|   Вход / выход  |     post    |      "/auth/register"   |    courseValidators    |           Отправка регистрационных данных для получения доступа в приложение             |
|   Вход / выход  |     post    |       "/auth/reset"     |           -            |       Отправка ранее регистрируемой почты для восстановления доступа в приложение        |
|   Вход / выход  |     post    |     "/auth/password"    |           -            |                   Отправка пароля пользователя для доступа в приложение                  |

Блок кода:
```node
const {Router} = require('express')
const bcrypt = require('bcryptjs')
const crypto = require('crypto')
const {validationResult} = require('express-validator')
const nodemailer = require('nodemailer')
const sendgrid = require('nodemailer-sendgrid-transport')
const User = require('../models/user')
const keys = require('../keys')
const regEmail = require('../emails/registration')
const resetEmail = require('../emails/reset')
const router = Router()
const {registerValidators} = require ('../utils/validators')

router.get('/login', async (req, res) => {})
router.get('/logout', async (req, res) => {})
router.post('/login', async (req, res) => {})
router.post('/register', registerValidators, async (req, res) => {})
router.get('/reset', (req, res) => {})
router.get('/password/:token', async (req, res) => {})
router.post('/reset', (req, res) => {})
router.post('/password', async (req, res) => {})
```
Где:
- `Router` -
- `bcrypt` -
- `crypto` -
- `validationResult` -
- `nodemailer` -
- `sendgrid` -
- `User` -
- `keys` -
- `regEmail` -
- `resetEmail` -
- `router` -
- `registerValidators` -
- `router.get('/login', async (req, res) => {})` -
- `router.get('/logout', async (req, res) => {})` -
- `router.post('/login', async (req, res) => {})` -
- `router.post('/register', registerValidators, async (req, res) => {})` -
- `router.get('/reset', (req, res) => {})` -
- `router.get('/password/:token', async (req, res) => {})` -
- `router.post('/reset', (req, res) => {})` -
- `router.post('/password', async (req, res) => {})` -


⬆️ [К оглавлению раздела "Маршрутизация"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/>


























