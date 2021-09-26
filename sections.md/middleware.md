# Промежуточные обработчики

Промежуточные обработчики - необходимы в большинстве случаев как обработчики авторизации, валидации, аутентификации и других различных проверок. Промежуточные обработчики могут быть как внешними библиотеками так собствено ручно созданные небольшие утилиты, которые в последствии интегрируют в необходимые маршрутизаторы.

⬅️ **Подробную информацию обо всех маршрутах смотреть в разделе [Маршрутизация](routes.md)** <br/> 
⬅️ **Подробную информацию обо всех контроллерах смотреть в разделе [Контроллеры](controllers.md)** <br/> 
↩️ [К оглавлению документации](../README.md) <br/> 

## Оглавление подраздела

- [Структура промежуточных обработчиков](#)
- [Глобальные промежуточные обработчики](#)
  - [authentificationMiddleware](#)
  - [userSessionMiddleware](#)
  - [fileMiddleware](#)
- [Локальные промежуточные обработчики](#)
  - [auth](#)
  - [registerValidators](#)
  - [courseValidators](#)


## Структура промежуточных обработчиков



## Библиотечные промежуточные обработчики

Глобальные промежуточные обработчики:
|          Функция    -      | Пакет npm |                        Короткое описание                         |
| :------------------------: | :-------: | :--------------------------------------------------------------: |
| authentificationMiddleware |   crsf    | Определяет наличие crsf токен, как определение прав пользователя |
|   userSessionMiddleware    |  session  |   Определяет и сохраняет данные сессий отдельных пользователей   |
|       fileMiddleware       |  multer   |                Загрузка определенных типов файлов                |


### bcryptjs

```node
const bcrypt = require('bcryptjs')
```

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

```node
router.post('/register', registerValidators, async (req, res) => {
    ...
    const {email, password, name} = req.body
    ...
    const hashPassword = await bcrypt.hash(password, 10)
...
})
```

```node
router.post('/password', async (req, res) => {
    ...

    if (user) {
      user.password = await bcrypt.hash(req.body.password, 10)
      ...
})
```


### crypto 

```node
const crypto = require('crypto')
```

```node
router.post('/reset', (req, res) => {
    crypto.randomBytes(32, async (err, buffer) => {})
...
})
```

### helmet 

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





### flash
```node
app.use(flash())
```


### csurf
```node
app.use(csrf())
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

Самописные промежуточные обработчики:
|      Функция       |     Пакет npm     |                         Короткое описание                         |
| :----------------: | :---------------: | :---------------------------------------------------------------: |
|         auth       |      session      | Если пользователь зарегестирован - доступ есть, иначе доступа нет |
| registerValidators | express-validator |           Проверяет правильность данных при регистрации           |
|  courseValidators  | express-validator |          Проверяет правильность данных при создании курса         |

### auth

```node
module.exports = function(req, res, next) {
    if (!req.session.isAuthenticated) {
        return res.redirect('/auth/login')
    }

    next()
}
```

### erroHandler

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
