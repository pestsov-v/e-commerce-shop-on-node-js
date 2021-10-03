# Вводные данные

Поскольку приложение использует другие сервисы, такие как MongoDB или sendGrid, то нам необходимы уникальные вводные данные для подключения этих сервисов к приложению. 

## Оглавление раздела

- [Вводные данные](#вводные-данные)
  - [Оглавление раздела](#оглавление-раздела)
  - [Режим разработки](#режим-разработки)
    - [Режим разработки](#режим-разработки-1)
    - [Режим готового приложения](#режим-готового-приложения)
    - [Переключатель режимов](#переключатель-режимов)
    - [MONGODB_URL](#mongodb_url)
    - [SESSION_SECRET](#session_secret)
    - [SENDGRID_API_KEY](#sendgrid_api_key)
    - [EMAIL_FROM](#email_from)
    - [BASE_URL](#base_url)
  - [process.env.PORT (Режим готового приложения)](#processenvport-режим-готового-приложения)

## Режим разработки

---

### Режим разработки 
Полный перечень данных данного приложения: <br/> 
- `MONGODB_URL` - URL-ссылка подключение доступа пользователя MongoDB с определенными правами. <br/> 
- `SESSION_SECRET` - секретный ключ сессии. <br/> 
- `SENDGRID_API_KEY` - ключ доступа в Ваш аккаунт sendGrid. <br/> 
- `EMAIL_FROM` - название email, который используется для рассылки сообщений пользователям приложения. <br/> 
- `BASE_URL` - URL-ссылка на домен приложения. <br/> 

А в коде они выглядят так: <br/> 
```node
module.exports = {
    MONGODB_URL: `mongodb+srv://<username>:<password>@<database name>.<strict name>.<collection>`,
    SESSION_SECRET: '<personal secret value>',
    SENDGRID_API_KEY: '<sendgrid api key>',
    EMAIL_FROM: '<email>',
    BASE_URL: '<url>'
  }
```

По скольку разработка происходит локально, то и данные хранятся в отдельном файле в формате: ключ-значение, поэтому вводные данные берутся из отдельного файла по ссылке-ключу на значение определенного значения. <br/> 

⬆️ [К оглавлению раздела](#оглавление-раздела). <br/> 
↩️ [К оглавлению документации](../README.md). <br/> 

---

### Режим готового приложения

При развёртывании приложений в PaaS сервисах, таких как Heroku, вводные данные могут отличаться, поскольку в режиме разработки BASE_URL является локальный localhost Вашего компьютера, а при разворачивании этого же приложения, но в сервисе Heroku, BASE_URL уже будет внутренним url-адресом Heroku. <br/> 

Для этого, в Heroku и подобных ему сервисах есть интерфейса ввода уникальных данных, где в одно поле вводят ключ, а в другое значение. В самом же приложении входные данные должны быть изменены не на ключ-значение, а на ключ-ссылку. <br/> 

⬆️ [К оглавлению раздела](#оглавление-раздела). <br/> 
↩️ [К оглавлению документации](../README.md). <br/> 

---

### Переключатель режимов

Чтобы иметь возможность запускать приложение в разных режимах, необходима функция обработчик, запускающая тот или иной режим по условию. В приложении за это отвечает файл keys.js.
Код файла keys.js: <br/> 
```node
if (process.env.NODE_ENV === 'production') {
  module.exports = require('./keys.prod')
} else {
  module.exports = require('./keys.dev')
}
```
Где: <br/> 
- `process.env.NODE_ENV === 'production'` - режим готового приложения. <br/> 
- `module.exports = require()` - подключение ключей-ссылок из файла './keys.prod'. <br/> 
*Код файла './keys.prod':* <br/> 
```node
module.exports = {
    MONGODB_URL: process.env.MONGODB_URL,
    SESSION_SECRET: process.env.SESSION_SECRET,
    SENDGRID_API_KEY: process.env.SENDGRID_API_KEY,
    EMAIL_FROM: process.env.EMAIL_FROM,
    BASE_URL: process.env.BASE_URL
  }
```
Где: <br/> 
- `MONGODB_URL` - ключ на ссылку process.env.MONGODB_URL. <br/> 
    - `process` - глобальный объект серверной платформы Node.js. <br/> 
    - `env` - объект содержащий среду разработки. <br/> 
    - `MONGODB_URL` - ссылка-значение на URL-ссылку подключения доступа в MongoDB с правами. <br/> 
- `module.exports = require()` - подключение ключей-значений из файла './keys.dev'. <br/> 

⬆️ [К оглавлению раздела](#оглавление-раздела). <br/> 
↩️ [К оглавлению документации](../README.md). <br/> 

---

### MONGODB_URL

Представляет из себя URL путь доступа в базу данных, необходима для подключения к базе данных и работы MongoStore. Сперва находим официальный сайт MongoDB в поисковике: <br/> 

[![1.png](https://i.postimg.cc/WbBTnstL/1.png)](https://postimg.cc/kBv0M3xf)

Нажимаем на кнопку "Sign in": <br/> 

[![2.png](https://i.postimg.cc/XJYbJJ1F/2.png)](https://postimg.cc/qtSYDkTM)

Регистрируемся с помощью аккаунта в Google: <br/> 

[![3.png](https://i.postimg.cc/gc5WzGVX/3.png)](https://postimg.cc/fkxF5ngD)

Выбираем аккаунт Google через который будем регистрироваться: <br/> 

[![4.png](https://i.postimg.cc/L61MhGDy/4.png)](https://postimg.cc/6ytPmjDR)

Подтверждаем правила использования сервиса: <br/> 

[![5.png](https://i.postimg.cc/4xRRcwFX/5.png)](https://postimg.cc/vchKF7sK)

Выбираем бесплатный тариф, его достаточно для успешного тестирования приложения:

[![6.png](https://i.postimg.cc/3wKMSNT6/6.png)](https://postimg.cc/bZFFdzdH)

Выбираем желанный сервис AWS и ближайший сервер к Вашему месторасположению, проверяем что кластер действительно "Free" и создаём кластер нажимая на кнопку "Create cluster": <br/> 

[![7.png](https://i.postimg.cc/3RcPxZv5/7.png)](https://postimg.cc/Pp166Yy2)

Ожидаем подготовку кластера, после чего переходим по вкладке "Database Access" чтобы добавить нового пользователя для управления базой данных: <br/> 

[![8.png](https://i.postimg.cc/Z0k2rKHw/8.png)](https://postimg.cc/gLDgppfh)

Добавляем нового пользователя: <br/> 

[![9.png](https://i.postimg.cc/44rx86FS/9.png)](https://postimg.cc/bd9PrtPx)

Проверяем метод аутентификации по паролю и заполняем поля логина и пароля и добавляем нового пользователя: <br/> 

[![10.png](https://i.postimg.cc/NjxGfWG3/10.png)](https://postimg.cc/qNzH1D3w)

Переходим во влкдаку "Network Access" чтобы добавить IP адресс, с которого будет возможность войти в базу данных: <br/> 

[![11.png](https://i.postimg.cc/7LsxS02X/11.png)](https://postimg.cc/Y4FKH48m)

Добавляем новый IP адресс: <br/> 

[![12.png](https://i.postimg.cc/ydD7jxhK/12.png)](https://postimg.cc/dkKMtqSN)

Выбирая вкладку "add current in address" Вы привязываете текущий IP адресс, если выберите вкладку "allow access from anywhere", то зайти в базу данных можно будет с любого IP адресс: <br/> 

[![13.png](https://i.postimg.cc/BZ1q18Qc/13.png)](https://postimg.cc/MXqkCpBv)

Ожидаем пока статус "pending" поменяется на статус "active" и переходим в базу данных: <br/> 

[![14.png](https://i.postimg.cc/cL1yT6Zz/14.png)](https://postimg.cc/hfY5tDX8)

Выбираем вкладку подключения "Connect": <br/> 

[![15.png](https://i.postimg.cc/Z5XHnDfL/15.png)](https://postimg.cc/68Ln1zx7)

Выбираем вкладку подключения к нашему приложению "Connect your application": <br/> 

[![16.png](https://i.postimg.cc/XJfQTckk/16.png)](https://postimg.cc/ftbcddFV)

Коппируем URL путь доступа доступа в базу данных: <br/> 

[![17.png](https://i.postimg.cc/h485wgCR/17.png)](https://postimg.cc/c6HhtqyF)


**1.Подключение MongoDB:** <br/> 

Подключение MongoDB в главный файл index.js: <br/> 
```node
const mongoose = require('mongoose')

await mongoose.connect(keys.MONGODB_URL, {
    useNewUrlParser: true,
    useFindAndModify: false
})
```
Где: <br/> 
- `mongoose.connect({})` - метод connect библиотеки mongoose принимающая 2 аргумента: URL путь доступа к базе данных и объект с рядом свойств. <br/> 
- `keys.MONGODB_URL` - URL путь доступа к базе данных. <br/> 
- `useNewUrlParser` - свойство анализатора строк, который необходимо включить true. <br/> 
- `useFindAndModify` - свойство управляющее подключением новых методом взамен старых, true по умолчанию. <br/> 


**2.Подключение MongoStore:**  <br/> 

Логика подключения MongoStore в главном файле index.js: <br/> 
```node
const session = require('express-session')
const MongoStore = require('connect-mongodb-session')(session)

const store = new MongoStore({
  collection: 'sessions',
  uri: keys.MONGODB_URL
})
```
Где: <br/> 
- `store` - экземпляр хранилища MongoDB new MongoStore({}) принимающий объект c перечнем свойств. <br/> 
- `collection` - ключ принимайющий значение название коллекции 'sessions' в базе данных MongoDB. <br/> 
- `uri` - ключ принимайющий значение URL пути доступа к базе данных keys.MONGODB_URL. <br/> 

⬆️ [К оглавлению раздела](#оглавление-раздела). <br/> 
↩️ [К оглавлению документации](../README.md). <br/> 

---

### SESSION_SECRET

Необходим для создания сессий. Может быть любым, который Вы захотите. <br/> 

⬆️ [К оглавлению раздела](#оглавление-раздела). <br/> 
↩️ [К оглавлению документации](../README.md). <br/> 

---

### SENDGRID_API_KEY

Необходим для подключения аккаунта sendGrid для отправки писем пользователю при регистрации или смены пароля. Для привязки приложения к аккаунту sendGrid необходим уникальный ключ к API sendGrid. Чтобы получить ключ, сперва необходимо найти официальный сайт sendGrid в поисковике: <br/> 

[![1.png](https://i.postimg.cc/rm5RjfNM/1.png)](https://postimg.cc/cg4J4Mzb)

Нажимаем на кнопку регистрации "start for free": <br/> 

[![2.png](https://i.postimg.cc/vTzgFDKK/2.png)](https://postimg.cc/RJWVwC41)

Заполняем поля почты "Email address" и пароля "Password", потверждаем, что Вы не робот и подтверждаем условия приватности сервиса: <br/> 

[![3.png](https://i.postimg.cc/XNcyyDvg/3.png)](https://postimg.cc/BtbvrpH8)

Заполняем индивидуальные данные аккаунта (эти данные могут быть и не существующими) sendGrid: <br/> 

[![4.png](https://i.postimg.cc/4yBd2q7k/4.png)](https://postimg.cc/tnnpYrN2)

Открываем меню настроек "Settings": <br/> 

[![5.png](https://i.postimg.cc/MT3NsY63/5.png)](https://postimg.cc/nj73rBQq)

Выбираем вкладку API ключей "API Keys": <br/> 

[![6.png](https://i.postimg.cc/DfDRL5Y8/6.png)](https://postimg.cc/pyf0tQvH)

Cоздаём новый API ключ: <br/> 

[![7.png](https://i.postimg.cc/903K5Q9y/7.png)](https://postimg.cc/zbj0T50f)

Выбираем название для ключа и поле "Full Access": <br/> 

[![8.png](https://i.postimg.cc/bwMRFFkt/8.png)](https://postimg.cc/K1PL1f9G)

Коппируем API ключ: <br/> 

[![9.png](https://i.postimg.cc/sDg9fRJW/9.png)](https://postimg.cc/23gBTMMj)

Добавляем значение в файл config.js: <br/> 

[![10.png](https://i.postimg.cc/Kcq3N3nK/10.png)](https://postimg.cc/BtFvZ67s)

⬆️ [К оглавлению раздела](#оглавление-раздела). <br/> 
↩️ [К оглавлению документации](../README.md). <br/> 

---

### EMAIL_FROM

Ключ-значения строки отправителя почты. Возвращаемся в Ваш персональный аккаунт sendGrid и выбираем создание индефикатора отправителя "Create a sender identity": <br/> 

[![10.png](https://i.postimg.cc/Xvd91NnR/10.png)](https://postimg.cc/B8Z8X0Lp)

Или выбираем другую ссылку, но также на создание того же простого отправителя "Create a Single Sender": <br/> 

[![11.png](https://i.postimg.cc/d3HCZL9y/11.png)](https://postimg.cc/64ZqD6gW)

Запонляем поля данных (достоверными данными должны быть только почты отправления, все остальные данные могут быть несуществующими) отправителя: <br/> 

[![12.png](https://i.postimg.cc/GhtGwxcJ/12.png)](https://postimg.cc/CzyzbDnz)

Добавляем значение в файл config.js: <br/> 

[![13-1.png](https://i.postimg.cc/xTZH0N8r/13-1.png)](https://postimg.cc/bsQdgJ1L)

⬆️ [К оглавлению раздела](#оглавление-раздела). <br/> 
↩️ [К оглавлению документации](../README.md). <br/> 

---

### BASE_URL

Необходим для гиперссылок отправляемые пользователю на почту с помощью sendGrid, в режиме разработки состоит из номера локалхоста. <br/> 

⬆️ [К оглавлению раздела](#оглавление-раздела). <br/> 
↩️ [К оглавлению документации](../README.md). <br/> 

---

## process.env.PORT (Режим готового приложения)

Необходим для создания ссылки на URL сформирован `Heroku` при развёртывании приложения в сервисе `Heroku`. В режиме разработки принимается номер локального хоста разработчика. <br/> 


