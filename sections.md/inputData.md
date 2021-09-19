## Вводные данные

### Режими приложения

При создании частных, а не Open Source, проектов есть ряд данных, которые нельзя выносить в готовое приложение, поэтому существует два различных режима приложения:
- режим разработки (development_product).
- режим готового приложения (production_product).

#### Режим разработки 
Полный перечень данных данного приложения:
- `MONGODB_URL` - URL-ссылка подключение доступа пользователя MongoDB с определенными правами. 
- `SESSION_SECRET` - секретный ключ сессии.
- `SENDGRID_API_KEY` - ключ доступа в Ваш аккаунт sendgrid
- `EMAIL_FROM` - название email, который используется для рассылки сообщений пользователям приложения.
- `BASE_URL` - URL-ссылка на домен приложения.

А в коде они выглядят так:
```node
module.exports = {
    MONGODB_URL: `mongodb+srv://<username>:<password>@<database name>.<strict name>.<collection>`,
    SESSION_SECRET: '<personal secret value>',
    SENDGRID_API_KEY: '<sendgrid api key>',
    EMAIL_FROM: '<email>',
    BASE_URL: '<url>'
  }
```

По скольку разработка происходит локально, то и данные хранятся в отдельном файле в формате: ключ-значение, поэтому вводные данные берутся из отдельного файла по ссылке-ключу на значение определенного значения.

#### Режим готового приложения

При развёртывании приложений в PaaS сервисах, таких как Heroku, вводные данные могут отличаться, поскольку в режиме разработки `BASE_URL` является локальный `localhost` Вашего компьютера, а при разворачивании этого же приложения, но в сервисе Heroku, `BASE_URL` уже будет внутренним url-адресом Heroku.

Для этого, в Heroku и подобных ему сервисах есть интерфейса ввода уникальных данных, где в одно поле вводят ключ, а в другое значение. В самом же приложении входные данные должны быть изменены не на ключ-значение, а на ключ-ссылку.

#### Переключатель режимов

Чтобы иметь возможность запускать приложение в разных режимах, необходима функция обработчик, запускающая тот или иной режим по условию. В приложении за это отвечает файл `keys.js`.
Блок кода файла `keys.js`
```node
if (process.env.NODE_ENV === 'production') {
  module.exports = require('./keys.prod')
} else {
  module.exports = require('./keys.dev')
}
```
Где:
- `process.env.NODE_ENV === 'production'` - режим готового приложения.
- `module.exports = require()` - подключение ключей-ссылок из файла `'./keys.prod'`.
*Код файла `'./keys.prod'`:*
```node
module.exports = {
    MONGODB_URL: process.env.MONGODB_URL,
    SESSION_SECRET: process.env.SESSION_SECRET,
    SENDGRID_API_KEY: process.env.SENDGRID_API_KEY,
    EMAIL_FROM: process.env.EMAIL_FROM,
    BASE_URL: process.env.BASE_URL
  }
```
Где:
- `MONGODB_URL` - ключ на ссылку `process.env.MONGODB_URL,`
    - `process` - глобальный объект серверной платформы `Node.js`
    - `env` - объект содержащий среду разработки.
    - `MONGODB_URL` - ссылка-значение на URL-ссылку подключения доступа в `MongoDB` с правами.
- `module.exports = require()` - подключение ключей-значений из файла `'./keys.dev'`.

#### MONGODB_URL

Необходим для подключения к базе данных и работы MongoStore.

**1.Подключение MongoDb:**
Блок кода:
```node
const mongoose = require('mongoose')

await mongoose.connect(keys.MONGODB_URL, {
    useNewUrlParser: true,
    useFindAndModify: false
})
```
Где:
- `mongoose.connect({})` -
- `keys.MONGODB_URL` -
- `useNewUrlParser` - `true`
- `useFindAndModify` - `false`



**2.Подключение MongoStore:**
Блок кода:
```node
const session = require('express-session')
const MongoStore = require('connect-mongodb-session')(session)

const store = new MongoStore({
  collection: 'sessions',
  uri: keys.MONGODB_URL
})
```
Где: 
- `store` - `new MongoStore({})`
- `collection` - `'sessions'`
- `uri` - `keys.MONGODB_URL`



#### SESSION_SECRET

Необходим для создания сессий.

#### SENDGRID_API_KEY

Необходим для подключения аккаунта `sendGrid`


#### EMAIL_FROM

ключ-значения строки отправителя почты.

#### BASE_URL

Необходим для гиперссылок отправляемые пользователю на почту с помощью `sendGrid`.


#### process.env.PORT (Режим готового приложения)

Необходим для создания ссылки на URL сформирован `Heroku` при развёртывании приложения в сервисе `Heroku`. В режиме разработки принимается номер локального хоста разработчика. 


