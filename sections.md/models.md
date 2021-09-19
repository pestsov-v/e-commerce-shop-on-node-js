# Модели


⬅️ **Подробную информацию обо всех контроллерах смотреть в разделе [Контроллеры](routes.md)** <br/>
⬅️ **Подробную информацию обо всех промежуточных обработчиках смотреть в разделе [Промежуточные обработчики](middleware.md))** <br/>
↩️ [К оглавлению документации](../README.md) <br/> 

## Оглавление раздела
- [Модели](#модели)
  - [Оглавление раздела](#оглавление-раздела)
  - [Структура моделизации](#структура-моделизации)
  - [Таблица всех моделей](#таблица-всех-моделей)
  - [Модель "Course"](#модель-course)
  - [Работа с моделью "Course"](#работа-с-моделью-course)
  - [Модель "Order"](#модель-order)
  - [Работа с моделью "Order"](#работа-с-моделью-order)
  - [Модель "User"](#модель-user)
  - [Работа с моделью "User"](#работа-с-моделью-user)


## Структура моделизации


⬆️ [К оглавлению раздела "Модели"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/> <br/>

## Таблица всех моделей

Таблица всех моделей:

| Модель |                              Предназначение                             |
| :----: | :---------------------------------------------------------------------: |
| course |          Схема полей с типами данных для создания одного курса          |
|  order |          Схема полей с типами данных для создания одного заказа         |
|  user  | Схема полей с типами данных для создания одного уникального пользователя|



⬆️ [К оглавлению раздела "Модели"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/> <br/>

## Модель "Course"
Полный блок кода:
```node
const {Schema, model} = require('mongoose')

const courseSchema = new Schema({
  title: {
    type: String,
    required: true
  },
  price: {
    type: Number,
    required: true
  },
  img: String,
  userId: {
    type: Schema.Types.ObjectId,
    ref: 'User'
  }
})

courseSchema.method('toClient', function() {
  const course = this.toObject()

  course.id = course._id
  delete course._id

  return course
})

module.exports = model('Course', courseSchema)
```
Где: 
- `Schema, model` -
- `new Schema({})` -
- `title` - 
  - `type` - `String` 
  - `required` - `true`
- `price` -
- `type` - `Number`
- `img` - `String`
- `userId` -
  - `type` - `Schema.Types.ObjectId`
  - `ref` - `'User'`
- `courseSchema.method('toClient', function() {})`
- `this.toObject()`
- `course.id` `course._id`
- `delete course._id`


⬆️ [К оглавлению раздела "Модели"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/> <br/>

## Работа с моделью "Course"

Использование модели `Course` в контроллере 

с типом запроса `post` маршрутизатора `/add`
Блок кода:
```node
const Course = require('../models/course')
...
  const course = new Course({
    title: req.body.title,
    price: req.body.price,
    img: req.body.img,
    userId: req.user
  })
...
```
Где:
- new Course({})
- `title` - `req.body.title`
- `price` - `req.body.price` 
- `img` - `req.body.img`
- `userId` - `req.user`

Использование модели `Course` в контроллере 

с типом запроса `post` маршрутизатора `/card/add`
Блок кода:
```node
const Course = require('../models/course')
...
  const course = await Course.findById(req.body.id)
  await req.user.addToCart(course)
  res.redirect('/card')
})
```
Где: 
- `Course.findById()` - `req.body.id`
- `req.user.addToCart()` - `course`

Использоваине модели `Course` в контроллерах страницы "Курсы"

Использование типом запроса `get` маршрутизатора `/courses`
Блок кода:
```node
const Course = require('../models/course')

...
    const courses = await Course.find()
    .populate('userId', 'email name')
    .select('price title img')
  ...
```
Где:
- `Course.find()` -
- `populate()` - `'userId', 'email name'`
- `select()` - `'price title img'`

Использование типом запроса `get` маршрутизатора `/courses/:id/edit`
Блок кода:
```node
const Course = require('../models/course')
...
const course = await Course.findById(req.params.id)

  if (course.userId.toString() !== req.user._id.toString()) {
        return res.redirect('/courses')
      }

  res.render('course-edit', {
      title: `Редактировать ${course.title}`,
      course
...
```
Где:
- `Course.findById(req.params.id)` -
- `course.userId.toString()` -
- `req.user._id.toString()` -
- `res.redirect()` - `'/courses'`
- `res.render('course-edit', {})` - `course`

Использование типом запроса `post` маршрутизатора `/edit`
Блок кода:
```node
const Course = require('../models/course')
...
const course = await Course.findById(id)
    if (course.userId.toString() !== req.user._id.toString()) {
      res.redirect('/courses')
    }
    Object.assign(course, req.body)
    await course.save()
  ...
```
Где:
- `Course.findById(id)` -
- `course.userId.toString()` -
- `req.user._id.toString()` -
- `res.redirect()` - `'/courses'`
- `Object.assign()` - `course, req.body`
- `course.save()` -

Использование типом запроса `post` маршрутизатора `/remove`
Блок кода:
```node
const Course = require('../models/course')
...
    await Course.deleteOne({
      _id: req.body.id, 
      userId: req.user._id
    })
    res.redirect('/courses')
...
```
Где:
- `Course.deleteOne({})` -
- `_id` - `req.body.id`
- `userId` - `req.user._id`
- `res.redirect()` - `'/courses'`

Использование типом запроса `get` маршрутизатора `/:id`
Блок кода:
```node
const Course = require('../models/course')
...
  const course = await Course.findById(req.params.id)
    res.render('course', {
      layout: 'empty',
      title: `Курс ${course.title}`,
      course
...
```
Где: 
- `Course.findById()` - `req.params.id`
- `res.render('course', {})` -
- `course` -


⬆️ [К оглавлению раздела "Модели"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/> <br/>

## Модель "Order"
Полный блок кода:
```node
const {Schema, model} = require('mongoose')

const orderSchema = new Schema({
  courses: [
    {
      course: {
        type: Object,
        required: true
      },
      count: {
        type: Number,
        required: true
      }
    }
  ],
  user: {
    name: String,
    userId: {
      type: Schema.Types.ObjectId,
      ref: 'User',
      required: true
    }
  },
  date: {
    type: Date,
    default: Date.now
  }
})

module.exports = model('Order', orderSchema)
```
Где: 
- `{Schema, model}` -
- `new Schema({})` -
- `courses: [{}]` -
- `course: {}` -
  - `type` - `Object`
  - `required` - `true`
- `count: {}` -
  - `type` - `Number`
- `user: {}` -
  - `name` - `String`
  - `type` - `Schema.Types.ObjectId`
  - `ref` - `'User'`
- `date: {}` -
  - `type` - `Date`
  - `default` - `Date.now`


⬆️ [К оглавлению раздела "Модели"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/> <br/>

## Работа с моделью "Order"

Использоваине модели `Order` в контроллерах страницы "Заказы"

Использование типом запроса `get` маршрутизатора `/orders`
```node 
const Order = require('../models/order')
  ...
    const orders = await Order.find({'user.userId': req.user._id})
      .populate('user.userId')
  ...
  orders: orders.map(o => {
        return {
          ...o._doc,
          price: o.courses.reduce((total, c) => {
            return total += c.count * c.course.price
          }, 0)
        }
      })
  ...
```
Где:
- `Order.find({})` - `'user.userId': req.user._id`
- `populate()` - `'user.userId'`
- `orders` - ` orders.map()`


Использование типом запроса `post` маршрутизатора `/orders`
```node
const Order = require('../models/order')
  ...
  const order = new Order({
      user: {
        name: req.user.name,
        userId: req.user
      },
      courses: courses
    })
  ...
  await order.save()
...
```
Где:
- `new Order({})` -
- `user: {}` -
- `name` - `req.user.name`
- `userId` - `req.user`
- `courses` - `courses`
- `order.save()` - 


⬆️ [К оглавлению раздела "Модели"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/> <br/>

## Модель "User"
Полный блок кода:
```node
const {Schema, model} = require('mongoose')

const userSchema = new Schema({
  email: {
    type: String,
    required: true
  },
  name: String,
  password: {
    type: String,
    required: true
  },
  avatarUrl: String,
  resetToken: String,
  resetTokenExp: Date,
  cart: {
    items: [
      {
        count: {
          type: Number,
          required: true,
          default: 1
        },
        courseId: {
          type: Schema.Types.ObjectId,
          ref: 'Course',
          required: true
        }
      }
    ]
  }
})

module.exports = model('User', userSchema)
```
Где:
- `{Schema, model}` -
- `new Schema({})` -
- `email: {}` -
  - `type` - `String`
  - `required` - `true`
- `name` - `String`
- `password` -
- `avatarUrl` - ``
- `resetToken` - 
- `resetTokenExp` - 
- `cart:{}` - 
- `items: [{}]` - 
- `count: {}`
- `courseId: {}`


Блок кода функций 
```node
userSchema.methods.addToCart = function(course) {
  const items = [...this.cart.items]
  const idx = items.findIndex(c => {
    return c.courseId.toString() === course._id.toString()
  })

  if (idx >= 0) {
    items[idx].count = items[idx].count + 1
  } else {
    items.push({
      courseId: course._id,
      count: 1
    })
  }

  this.cart = {items}
  return this.save()
}
```
Где:
- `userSchema.methods.addToCart = function(course) {` -
- `[...this.cart.items]` -
- `items.findIndex` -
- `c.courseId.toString() === course._id.toString()` -
- `items[idx].count = items[idx].count + 1` -
- `items.push({})` -
- `courseId` - `course._id`
- `count: 1` -
- `this.cart` - `{items}`
- `this.save()` - 


```node
userSchema.methods.removeFromCart = function(id) {
  let items = [...this.cart.items]
  const idx = items.findIndex(c => c.courseId.toString() === id.toString())

  if (items[idx].count === 1) {
    items = items.filter(c => c.courseId.toString() !== id.toString())
  } else {
    items[idx].count--
  }

  this.cart = {items}
  return this.save()
}
```
Где:
- `userSchema.methods.removeFromCart` - `function(id) {}`
- `[...this.cart.items]` -
- `items.findIndex()` -
- `c => c.courseId.toString() === id.toString()` -
- `items[idx].count === 1` -
- `items.filter` -
- `c => c.courseId.toString() !== id.toString()` -
- `items[idx].count--` - 
- `this.cart` - `{items}`
- `this.save()` -


```node
userSchema.methods.clearCart = function() {
  this.cart = {items: []}
  return this.save()
}
```
Где:
- `userSchema.methods.clearCart` - `function() {}`
- `this.cart` - `{items: []}`
- `this.save()` -


⬆️ [К оглавлению раздела "Модели"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/> <br/>

## Работа с моделью "User"

Использование модели `User` в контроллерах страницы "Авторизации"

Использование типом запроса `post` маршрутизатора `/auth/login`
Блок кода:
```node
const User = require('../models/user')
...
    const candidate = await User.findOne({ email })

    if (candidate) {
          const areSame = await bcrypt.compare(password, candidate.password)
    if (areSame) {
        req.session.user = candidate
        req.session.isAuthenticated = true
        req.session.save(err => {
    ...
```
Где:
- `User.findOne({ email })` -
- `bcrypt.compare()` - `password`, `candidate.password`
- `req.session.user` - `candidate`
- `req.session.isAuthenticated` -  `true`
- `req.session.save(err => {})` -  

Использование типом запроса `post` маршрутизатора `/auth/register`
Блок кода:
```node
const User = require('../models/user')
...
    const user = new User({
        email, name, password: hashPassword, cart: {items: []}
      })
      await user.save()
    ...
```
Где:
- `new User({})` -
- `email, name, password: hashPassword, cart: {items: []}` -
- `await user.save()` -


Использование типом запроса `post` маршрутизатора `/auth/password/:token`
Блок кода:
```node
const User = require('../models/user')
...
  const user = await User.findOne({
      resetToken: req.params.token,
      resetTokenExp: {$gt: Date.now()}
    })
...
```
Где:
- `User.findOne({})` -
- `resetToken` - `req.params.token`
- `resetTokenExp` - `{$gt: Date.now()}`


Использование типом запроса `post` маршрутизатора `/auth/reset`
Блок кода:
```node
const User = require('../models/user')
...
const candidate = await User.findOne({email: req.body.email})
    if (candidate) {
        candidate.resetToken = token
        candidate.resetTokenExp = Date.now() + 60 * 60 * 1000
        await candidate.save()
    ...
```
Где:
- `User.findOne({})` - `email: req.body.email`
- `candidate` -
- `candidate.resetToken` - `token`
- `candidate.resetTokenExp` - `Date.now() + 60 * 60 * 1000`
- `candidate.save()` -

Использование типом запроса `post` маршрутизатора `/auth/password`
Блок кода:
```node
    const user = await User.findOne({
        _id: req.body.userId,
        resetToken: req.body.token,
        resetTokenExp: {$gt: Date.now()}
    ...
    if (user) {
        user.password = await bcrypt.hash(req.body.password, 10)
        user.resetToken = undefined
        user.resetTokenExp = undefined
        await user.save()
    ...
```
Где:
- ` User.findOne({})` -
- `_id` - `req.body.userId`
- `resetToken` - `req.body.token`
- `resetTokenExp` - `{$gt: Date.now()}`
- `user.password` - ` bcrypt.hash(req.body.password, 10)`
- `user.resetToken` - `undefined`
- `user.resetTokenExp` - `undefined`
- `user.save()` - 

Использование модели `User` в контроллерах страницы "Профиль"

Использование типом запроса `post` маршрутизатора `/profile`
Блок кода:
```node
const User = require('../models/user')
...
    const user = await User.findById(req.user._id)
    Object.assign(user, toChange)
    await user.save()
...
```
Где:
- `User.findById()` - `req.user._id`
- `Object.assign()` - `user, toChange`
- `user.save()` -

Использование модели `User` в промежуточном обработчике `registerValidators`
Блок кода:
```node
const User = require('../models/user')
...
    const user = await User.findOne({email: value})
        if (user) {
            ...
        }
...
```
Где:
- `User.findOne({})` - `email: value`
- `if (user) {}` -

Использование модели `User` в промежуточном обработчике `userMiddleware`

Блок кода:
```node
const User = require('../models/user')
...
    req.user = await User.findById(req.session.user._id)
    next()
...
```
Где:
- `User.findById()` - `req.session.user._id`




⬆️ [К оглавлению раздела "Модели"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/> <br/>
