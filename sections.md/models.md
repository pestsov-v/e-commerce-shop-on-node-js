# Модели

Модели формируют схему данных, которые заполняются, изменяются или извлекаются из базы данных. Согласно паттерну MVC доступ к моделям имеют или простые контроллеры или контроллеры с подключением промежуточных обработчиков. 

⬅️ **Подробную информацию обо всех контроллерах смотреть в разделе [Контроллеры](routes.md)** <br/>
⬅️ **Подробную информацию обо всех промежуточных обработчиках смотреть в разделе [Промежуточные обработчики](middleware.md))** <br/>
↩️ [К оглавлению документации](../README.md) <br/> 

## Оглавление раздела
- [Модели](#модели)
  - [Оглавление раздела](#оглавление-раздела)
  - [Структура моделей](#структура-моделей)
  - [Таблица всех моделей](#таблица-всех-моделей)
  - [Модель "Course"](#модель-course)
  - [Реализация модели курса "Course" в контроллерах приложения](#реализация-модели-курса-course-в-контроллерах-приложения)
    - [Контроллер "Добавить курс" с типом запроса post маршрутизатора '/add'](#контроллер-добавить-курс-с-типом-запроса-post-маршрутизатора-add)
    - [Контроллер "Корзина" с типом запроса post маршрутизатора '/card/add'](#контроллер-корзина-с-типом-запроса-post-маршрутизатора-cardadd)
    - [Контроллер "Курсы" с типом запроса get маршрутизатора '/courses'](#контроллер-курсы-с-типом-запроса-get-маршрутизатора-courses)
    - [Контроллер "Курсы" с типом запроса get маршрутизатора '/courses/:id/edit'](#контроллер-курсы-с-типом-запроса-get-маршрутизатора-coursesidedit)
    - [Контроллер "Курсы" с типом запроса post маршрутизатора '/courses/edit'](#контроллер-курсы-с-типом-запроса-post-маршрутизатора-coursesedit)
    - [Контроллер "Курсы" с типом запроса post маршрутизатора '/courses/delete'](#контроллер-курсы-с-типом-запроса-post-маршрутизатора-coursesdelete)
    - [Контроллер "Курсы" с типом запроса get маршрутизатора '/courses/:id'](#контроллер-курсы-с-типом-запроса-get-маршрутизатора-coursesid)
  - [Модель "Order"](#модель-order)
  - [Реализация модели заказа "Order" в контроллерах приложения](#реализация-модели-заказа-order-в-контроллерах-приложения)
    - [Контроллер "Заказы" с типом запроса get маршрутизатора /orders](#контроллер-заказы-с-типом-запроса-get-маршрутизатора-orders)
    - [Контроллер "Заказы" с типом запроса post маршрутизатора '/orders'](#контроллер-заказы-с-типом-запроса-post-маршрутизатора-orders)
  - [Модель "User"](#модель-user)
  - [Реализация модели заказа "User" в контроллерах приложения](#реализация-модели-заказа-user-в-контроллерах-приложения)
    - [Контроллер "Профиль" с типом запроса post маршрутизатора '/profile'](#контроллер-профиль-с-типом-запроса-post-маршрутизатора-profile)
    - [Контроллер "Авторизация" с типом запроса post маршрутизатора '/auth/login'](#контроллер-авторизация-с-типом-запроса-post-маршрутизатора-authlogin)
    - [Контроллер "Авторизация" с типом запроса post маршрутизатора '/auth/register'](#контроллер-авторизация-с-типом-запроса-post-маршрутизатора-authregister)
    - [Контроллер "Авторизация" с типом запроса post маршрутизатора '/auth/password/:token'](#контроллер-авторизация-с-типом-запроса-post-маршрутизатора-authpasswordtoken)
    - [Контроллер "Авторизация" с типом запроса post маршрутизатора '/auth/reset'](#контроллер-авторизация-с-типом-запроса-post-маршрутизатора-authreset)
    - [Контроллер "Авторизация" с типом запроса post маршрутизатора '/auth/password'](#контроллер-авторизация-с-типом-запроса-post-маршрутизатора-authpassword)
  - [Реализация модели заказа "User" в промежуточных обработчиках приложения](#реализация-модели-заказа-user-в-промежуточных-обработчиках-приложения)
    - [Промежуточный обработчик registerValidators](#промежуточный-обработчик-registervalidators)
    - [Промежуточный обработчик userMiddleware](#промежуточный-обработчик-usermiddleware)

## Структура моделей

Каждая модель интегрируется в соответствующие контроллеры, связанные с запросами к конкретной модели. Каждая модель при этом - это экземпляр объекта Schema библиотеки mongoose, которая описывает все параметры сущностей, такие как пользователи или товары. Исходя из этих параметров, определяются и строятся зависимости между моделями, а так же перечень свойств самой модели. 

⬆️ [К оглавлению раздела "Модели"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/>

## Таблица всех моделей

Ниже представлены все модели которые применяются в приложении:

| Модель |                   Предназначение                   |
| :----: | :------------------------------------------------: |
| course |           Структура сущности одного курса          |
|  order |           Структура сущности одного заказа         |
|  user  | Структура сущности одного уникального пользователя |

⬆️ [К оглавлению раздела "Модели"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/>

## Модель "Course" 

Модель курса Сourse используется для создания, редактирования, обновления или удаление одного курса. Поскольку в дальнейшем права на редактирование курса сможет лишь только тот пользователь, который его создал, то модель курса Course предполагает связывание с моделью пользователя User для привязки конкретного курса к определенному пользователю.  

*Схема курса Course:*  <br/>
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

module.exports = model('Course', courseSchema)
```
Где: <br/>
- `Schema, model` - получение с помощью деструктуризации объекта схемы Schema и объект модели model из библиотеки mongoose. <br/>
- `new Schema({})` - экземпляр схемы Schema.  <br/>
- `title` - поле заголовка, тип которого определён как строка`type: String, с атрибутом определяющим обязательность наличия заполнености этого поля required: true. <br/>
- `price` - поле цены, тип которого определено как объект type: Number, с атрибутом определяющим обязательность наличия заполнености этого поля required: true. <br/>
- `userId` - тип поля ID пользователя,тип которого определён как реферальный ключ type: Schema.Types.ObjectId, а сам ключ ссылается на модель ref: 'User'. <br/>

Также модель курса Course наличивает метод, для успешной работы с объектом одного курса. Метод меняет значение уникального ID пользователя _id из базы данных MongoDB на значение ID принимающее cреда разработки Node.js: <br/>
```node
courseSchema.method('toClient', function() {
  const course = this.toObject()

  course.id = course._id
  delete course._id

  return course
})
```
Где: <br/>
- `courseSchema.method('toClient', function() {})` - функция-обработчик, изменяющая формат ID _id приходящий из MongoDB в формат ID определяющимся в среде разработки node.js - id. <br/>
- `const course = this.toObject()` - присваем переменной `course` объект приходящего курса. <br/>
- `course.id` `course._id` - присванием значение ID `_id` из базы данных - ID `id` курса. <br/>
- `delete course._id` - удаляем ID `_id` курса приходящего из базы данных как ликвидация мусора. <br/>

⬆️ [К оглавлению раздела "Модели"](#оглавление-раздела). <br/>
↩️ [К оглавлению документации](../README.md). <br/>

## Реализация модели курса "Course" в контроллерах приложения

---

### Контроллер "Добавить курс" с типом запроса post маршрутизатора '/add'

Контроллер с типом запроса post маршрутизатора '/add' реализовывает создание нового курса. Часть кода контроллера работающий с моделью: <br/>
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
Где: <br/>
- `new Course({})` - новый экземпляр модели Course. <br/>
- `title: req.body.title` - ключ заголовка, который принимает значение заголовка, который приходит в теле объекта запроса req.body. <br/>
- `price: req.body.price` - ключ цены, который принимает значение цены, который приходит в теле объекта запроса req.body. <br/>
- `img: req.body.img` - ключ картинки, который принимает значение картинки, который приходит в теле объекта запроса req.body. <br/>
- `userId: req.user` - ключ пользователя, который принимает значение ID пользователя создавшего курс. <br/>

⬅️ Подробнее о данном контролере смотрите [Метод запроса post по маршруту '/add'](controllers.md#метод-запроса-post-по-маршруту-add). <br/>

---

### Контроллер "Корзина" с типом запроса post маршрутизатора '/card/add'

Контроллер с типом запроса post маршрутизатора '/card/add' реализовывает добавление одного курса в корзину пользователя. Часть кода контроллера работающий с моделью: <br/>
```node
const Course = require('../models/course')
...
  const course = await Course.findById(req.body.id)
  await req.user.addToCart(course)
  res.redirect('/card')
})
```
Где: <br/>
- `req.user.addToCart()` - функция модели User - addToCart принимающая аргумент - course. <br/>
- `res.redirect()` - метод redirect объекта ответа response, который перенаправляет пользователя на страницу с маршрутом '/card'. <br/>

⬅️ Подробнee о функции addToCart смотрите в разделе [Модель "User"](#модель-user). <br/>
⬅️ Подробнее о данном контролере смотрите [Метод запроса post по маршруту '/card/add'](controllers.md#метод-запроса-post-по-маршруту-cardadd). <br/>

---

### Контроллер "Курсы" с типом запроса get маршрутизатора '/courses'

Контроллер с типом запроса get маршрутизатора '/courses' реализовывает получение всех курсов на странице "Курсы". Часть кода контроллера работающий с моделью: <br/>
```node
const Course = require('../models/course')

...
    const courses = await Course.find()
    .populate('userId', 'email name')
    .select('price title img')
  ...
```
Где: <br/>
- `Course.find()` - метод find модели Course, который ищет все курсы из коллекции Course и возвращает эти данные. <br/>
- `populate` - метод проверяющий, включают ли данные userId и email name после чего возвращает только те документы, в которых наявны userId и email name. <br/>
- `select` - метод возвращающий только значения ключей: price, title и img. <br/>

⬅️ Подробнее о данном контролере смотрите [Метод запроса get по маршруту '/courses'](controllers.md#метод-запроса-get-по-маршруту-courses). <br/>

---

### Контроллер "Курсы" с типом запроса get маршрутизатора '/courses/:id/edit'

Контроллер с типом запроса get маршрутизатора '/courses/:id/edit', реализовывает редактирование одного курса. Часть кода контроллера работающий с моделью: <br/>
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
Где: <br/>
- `Course.findById()` - метод findById модели Course, который ищет курс по входящему запросу с ID req.params.id из коллекции Course и возвращает эти данные. <br/>
- `course.userId.toString() !== req.user._id.toString()` - проверка, которая сравнивает ID курса с ID пользователя, который создал этот курс.<br/>
- `redirect()` - метод `redirect` объекта ответа response перенаправление на страницу с маршрутом '/courses'. <br/>
- `res.render('course-edit', {})` - метод render объекта response, который визуализирует данные приходящие из файла course-edit.hbs. <br/>
- `course` - ключ принимающий одноименное значение, в котором хранится данные одного курса определенного по ID. <br/>

⬅️ Подробнее о данном контролере смотрите [Метод запроса get по маршруту '/courses/:id/edit'](controllers.md#метод-запроса-get-по-маршруту-coursesidedit). <br/>

---

### Контроллер "Курсы" с типом запроса post маршрутизатора '/courses/edit'

Контроллер с типом запроса post маршрутизатора '/courses/edit' реализовывет отправку отредактируемых данных одного курса. Часть кода контроллера работающий с моделью: <br/>
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
Где: <br/>
- `Course.findById()` - метод findById модели Course, который ищет курс по входящему запросу с ID req.params.id из коллекции Course и возвращает эти данные. <br/>
- `course.userId.toString() !== req.user._id.toString()` - проверка, которая сравнивает ID курса с ID пользователя, который создал этот курс. <br/>
- `redirect()` - метод redirect объекта ответа response перенаправление на страницу с маршрутом '/courses'. <br/>
- `Object.assign()` - метод assign глобального объекта Object, который принимает объект курса course с опредённым ID и коппирует все перечислимые свойста course в целевой объект - req.body. <br/>
- `course.save()` - метод save объекта course сохраняющий данные в базе данных MongoDB. <br/>

⬅️ Подробнее о данном контролере смотрите [Метод запроса get по маршруту '/courses/:id/edit'](controllers.md#метод-запроса-get-по-маршруту-coursesidedit). <br/>

---

### Контроллер "Курсы" с типом запроса post маршрутизатора '/courses/delete'

Контроллер с типом запроса post маршрутизатора '/courses/delete' реализовывет удаление одного курса. Часть кода контроллера работающий с моделью: <br/>
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
Где: <br/>
- `Course.deleteOne({})` - метод deleteOne модели Course, который ищет курс по ID у определенного пользователя. Метод принимает 2 аргумента: <br/>
  - `_id` - значение ID курса сравнивающее значение ID курса в получаемом запросе - req.body.id. <br/> 
  - `userId` - значение ID пользователя сравнивающее значение ID пользователя в получаемом запросе - req.user._id. <br/>
- `res.redirect()` - метод redirect объекта ответа response перенаправление на страницу с маршрутом '/courses'. <br/>

⬅️ Подробнее о данном контролере смотрите [Метод запроса post по маршруту '/courses/remove'](controllers.md#метод-запроса-post-по-маршруту-coursesremove). <br/>

---

### Контроллер "Курсы" с типом запроса get маршрутизатора '/courses/:id'

Контроллер с типом запроса post маршрутизатора '/courses/:id' реализовывет получение одного курса. Часть кода контроллера работающий с моделью: <br/>
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
Где: <br/>
- `Course.findById(req.params.id)` - метод findById модели Course, который ищет курс по входящему запросу с ID req.params.id из коллекции Course и возвращает эти данные. <br/> 
- `res.render('course', {})` - метод render объекта response, который визуализирует данные приходящие из файла course.hbs. <br/>
- `course` - ключ принимающий одноименное значение, в котором хранится данные одного курса определённого по ID. <br/>

⬅️ Подробнее о данном контролере смотрите [Метод запроса post по маршруту '/courses/remove'](controllers.md#метод-запроса-post-по-маршруту-coursesremove). <br/>
⬆️ [К оглавлению раздела "Модели"](#оглавление-раздела). <br/>
↩️ [К оглавлению документации](../README.md). <br/>

## Модель "Order"

Модель заказа Order используется для формивание и отправки заказов пользователя, которые формируются из товаров, выбранные пользователям в объект корзины. <br/>

*Схема заказа Order:* <br/>
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
Где: <br/>
- `{Schema, model}` - получение с помощью деструктуризации объекта схемы Schema и объект модели model из библиотеки mongoose. <br/>
- `new Schema({})` - экземпляр класса Schema. <br/>
- `courses: [{}]` - поле массива курсов. <br/>
- `course ` - поле курса, тип которого определено как объект type: Object, с атрибутом определяющим обязательность наличия заполнености этого поля required: true. <br/>
- `count` - поле количества едениц одного курса, тип которого определено как объект type: Number, с атрибутом определяющим обязательность наличия заполнености этого поля required: true. <br/>
- `user` - тип поля ID пользователя,тип которого определён как реферальный ключ type: Schema.Types.ObjectId, а сам ключ ссылается на модель ref: 'User'. <br/>
- `date` - поле времени, тип которого определён как специальный объект type: Date, а время определения типа данных date является действенное время на момент создания данных default: Date.now.  <br/>

⬆️ [К оглавлению раздела "Модели"](#оглавление-раздела). <br/>
↩️ [К оглавлению документации](../README.md). <br/>

## Реализация модели заказа "Order" в контроллерах приложения

---

### Контроллер "Заказы" с типом запроса get маршрутизатора /orders

Контроллер с типом запроса get маршрутизатора /orders реализовывает получение всех заказов пользователя. Часть кода контроллера работающий с моделью: <br/>
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
Где: <br/>
- `Order.find({})` - метод find модели Order, который находит данные, в которых ID 'user.userId' является таким же как ID запроса req.user._id. <br/>
    - `user.userId` - ID пользователя, который есть в базе данных MongoDB. <br/>
    - `req.user._id` - ID запроса, который приходит с объектом запроса request. <br/>
- `populate()` - метод проверяющий, включение user.userId в результат. <br/>
- `orders: orders.map(o => {})` - ключ принимающий значение результата метода map выражения orders.map(o => {}). Метод возвращает результат разворачивание массива o объекта заказов orders, после чего выполняет тело метода. <br/>
- `...o._doc` - метод spread ... результатом которого является развёртывание элемента o в виде простого объекта. <br/>
- `o.courses.reduce((total, c) => {})` - метод reduce есть у каждого массива. Метод позволяет свернуть сумму каждого элемента массива в общую сумму. <br/>
- `return total += c.count * c.course.price` - выражение вычисляющее итоговую сумму с расчёта количество определенного курса умноженное на стоимость этого курса. <br/>
  - `c.price` - цена курса. <br/>
  - `c.count` - количество курсов покупаемых пользователем. <br/>

⬅️ Подробнее о данном контролере смотрите [Метод запроса get по маршруту '/orders'](controllers.md#метод-запроса-get-по-маршруту-orders). <br/>

---

### Контроллер "Заказы" с типом запроса post маршрутизатора '/orders'

Контроллер с типом запроса post маршрутизатора /orders реализовывает формирование и отправку одного заказа, с массивом курсов, пользователю. Часть кода контроллера работающий с моделью: <br/>
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
Где: <br/>
- `const order = new Order({})` - order экземпляр объекта Order, в тело которого включены все свойства объекта Order. <br/>
- `user: {}` - объект пользователя входящий в объект заказа Order. <br/>
  - `name` - ключ принимающий значение запроса имени пользователя req.user.name. <br/>  
  - `userId` - ключ принимающий значение запроса ID пользователя req.user. <br/>
- `courses` - ключ принимающий одноименный массив курсов courses. <br/>
- `order.save()` - метод save объекта order сохраняющий данные в базе данных MongoDB. <br/>

⬅️ Подробнее о данном контролере смотрите [Метод запроса post по маршруту '/orders'](controllers.md#метод-запроса-post-по-маршруту-orders). <br/>
⬆️ [К оглавлению раздела "Модели"](#оглавление-раздела). <br/>
↩️ [К оглавлению документации](../README.md). <br/> 

## Модель "User"

Модель пользователя User используется для обработки прав доступа к курсам, формирования, привязки и сохраненние определёных данных, таких как история заказов, курсы покупаемые пользователем или же сформированый блок корзины за конкретным пользователям. <br/>

*Схема пользователя User:* <br/>
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
Где: <br/>
- `{Schema, model}` - получение с помощью деструктуризации объекта схемы Schema и объект модели model из библиотеки mongoose. <br/>
- `new Schema({})` - экземпляр класса Schema. <br/>
- `email: {}` - поле email, тип которого определён как строка type: String, с атрибутом определяющим обязательность наличия заполнености этого поля required: true. <br/>
- `name` - поле имени, тип которого определён как строка type: String. <br/>
- `password` - поле пароля, тип которого определён как строка type: String, с атрибутом определяющим обязательность наличия заполнености этого поля required: true. <br/>
- `avatarUrl` - поле пути аватара пользователя, тип которого определён как строка type: String. <br/>
- `resetToken` - поле токена, для сброса пароля, тип которого определён как строка type: String. <br/>
- `resetTokenExp` - поле определяющий срок годности токена, тип которого определён как специальный объект type: Date. <br/>
- `cart:{}` - поле объекта с массивом курсов. <br/>
- `items: [{}]` - поле с массивом курсов. <br/>
- `count: {}` - поле количества едениц одного курса, тип которого определено как число type: Number, с атрибутом определяющим обязательность наличия заполнености этого поля required: true, и который по умолчанию имеет значение default: 1`. <br/>
- `courseId: {}` - тип поля ID курса, тип которого определён как реферальный ключ type: Schema.Types.ObjectId, а сам ключ ссылается на модель ref: 'Course'. <br/>

---

Также модель пользователя User наличивает методы, для различных задач работы с моделью User. Модель наличивает следующие методы: <br/>
- `addToCart` - метод добавляющий курсы в массив корзины пользователя. <br/>
- `removeFromCart` - метод удаляющий курс из массива корзины. <br/>
- `clearCart` - метод очищающий корзину пользователя после сформирование и отправки заказа пользователя. <br/>

*Функция addToCart:* <br/>
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
Где: <br/>
- `userSchema.methods.addToCart` - метод methods, который есть у каждой схемы Schema в том числе и userSchema позволяющий создать метод для конкретной схемы. Название метода указывается после точки: .addToCart. <br/>
- `function(course) {}` - функция принимающая объект курса course. <br/>
- `const items = [...this.cart.items]` - метод spread ... результатом которого является развёртывание всех данных из массива items вложенного в объект cart, который передаётся контекстом this, где this это получаемый функцией курс course. <br/>
- `const idx = items.findIndex(c => {})` - метод findIndex возвращает индексированый элемент массива после чего обрабатывает его в теле функции. <br/>
- `return c.courseId.toString() === course._id.toString()` - возвращает значение ID приведенного к строке course._id.toString() и приводит строгое сравнение с ID курсом запроса c.courseId.toString(). <br/>
- `if (idx >= 0) {}` - условие, которое выполняется, если элемент больше или равен 0. <br/>
- `items[idx].count = items[idx].count + 1` - элемент по индексу idx, который увеличивается на 1 количество определенного курса в массиве items. <br/>
- `items.push({})` -  метод `push` добавляющий вконец списка тело объекта, состоящие из 2 ключей: courseId и count. <br/>
- `courseId` - ключ принимающий значение ID course._id. ID с нижним подчёркиванием _id является уникальными идентификаторами в базе данных MongoDB. <br/>
- `count` - ключ принимающий значение количества одного курса в количестве - 1. <br/>
- `this.cart` - ключ переопределяющий значение объекта {items}, как новый объект контекста корзины, где this.cart является равным записи course.cart. <br/>
- `return this.save()` -  метод save сохроняет состояние корзины в базе данных у модели User, а именно оновленный массив в поле cart = [] модели User после чего return возвращает сохранённое значение как результат выполнения функции. <br/>

*Функция removeFromCart:* <br/>
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
Где: <br/>
- `userSchema.methods.removeFromCart` - метод methods, который есть у каждой схемы Schema в том числе и userSchema позволяющий создать метод для конкретной схемы. Название метода указывается после точки: .removeFromCart. <br/>
- `function()` - функция принимающая ID запроса req.user.id, который принимается аргументов id функции. <br/>
- `let items = [...this.cart.items]` - метод spread ... результатом которого является развёртывание всех данных из массива items вложенного в объект cart, который передаётся контекстом this, где this это получаемый функцией курс course. <br/>
- `const idx = items.findIndex(c => c.courseId.toString() === id.toString())` - возвращает индексируемый єлемент массива после чего сравнивает приведенный к строке ID объекта с и сравнивает строгим сравнением с ID запроса request, который также приводится к строке. Уникальный ID является и так строкой, но во избежания ошибок, принимается явное преобразование ID в строку. <br/>
- `if (items[idx].count === 1) {}` - условие при котором индексированный элемент idx в массиве items находится в количестве 1, то выполнится тело условия. <br/>
- `items = items.filter(c => c.courseId.toString() !== id.toString())` - метод filter перебирает элементы массива c и создаёт новый массив в который отбирает те элементы c, где ID курса не равняется ID курса находиящийся в запросе req.user.id. <br/>
- `items[idx].count--` - производит декремент (уменьшает значение на 1) у свойства count элемента idx массива items. <br/>
- `this.cart` - ключ переопределяющий значение объекта {items}, как новый объект контекста корзины, где this.cart является аналогичным записи course.cart. <br/>
- `return this.save()` -  метод save сохроняет состояние корзины в базе данных у модели User, а именно оновленный массив в поле cart = [] модели User после чего return возвращает сохранённое значение как результат выполнения функции. <br/>


*Функции clearCart:* <br/>
```node
userSchema.methods.clearCart = function() {
  this.cart = {items: []}
  return this.save()
}
```
Где: <br/>
- `userSchema.methods.clearCart` - метод methods, который есть у каждой схемы Schema в том числе и userSchema позволяющий создать метод для конкретной схемы. Название метода указывается после точки: .clearCart. <br/>
- `function() {}` - функция не принимающая никаких аргументов. <br/>
- `this.cart` - {items: []} переопределение массива items пустого значение массива в объекте this.cart, где this.cart аналогично user.cart. <br/>
- `return this.save()` - возврат сохранённого результата в базе данных MongoDB массива cart модели user. <br/>

⬆️ [К оглавлению раздела "Модели"](#оглавление-раздела). <br/>
↩️ [К оглавлению документации](../README.md). <br/>

## Реализация модели заказа "User" в контроллерах приложения

---

### Контроллер "Профиль" с типом запроса post маршрутизатора '/profile'

Контроллер с типом запроса post маршрутизатора '/profile' реализовывает сохранение новых данных пользователя. Часть кода контроллера работающий с моделью: <br/>
```node
const User = require('../models/user')
...
    const user = await User.findById(req.user._id)
    Object.assign(user, toChange)
    await user.save()
...
```
Где: <br/>
- `User.findById()` - метод findById модели User, который ищет пользователя по входящему запросу с ID req.user._id из коллекции User и возвращает эти данные. <br/>
- `Object.assign()` - метод assign глобального объекта Object, который включает данные документа user, которые подставляются в целевой объект toChange и который принимает два аргумента:
  - `user` - объект пользователя найденого в базе данных по ID пользователя. <br/>
  - `toChange` - объект, внутри которого определны ряд клюей пользователя в том числе, name, который принимает значение req.body.name. <br/>
- `user.save()` - метод save объекта user сохраняющий данные в базе данных MongoDB. <br/>

⬅️ Подробнее о данном контролере смотрите [Метод запроса post по маршруту '/auth/password'](controllers.md#метод-запроса-post-по-маршруту-authpassword). <br/>

---

### Контроллер "Авторизация" с типом запроса post маршрутизатора '/auth/login'

Контроллер с типом запроса post маршрутизатора '/auth/login' реализовывает получение доступа, если пользователь зарегестрирован. Часть кода контроллера работающий с моделью: <br/>
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
Где: <br/>
- `User.findOne({ email })` - метод findOne модели User, который ищет первый объект по значению одноименного ключа email. <br/>
- `bcrypt.compare(password, candidate.password)` - метод compare библиотеки bcrypt, который проверяет пароль пользователя с паролем пользователя полученного запросом от базы данных MongoDB. <br/>
- `req.session.user` - свойство пользователя user в обЪекте сессии session, которой присваиваются данные авторизированного пользователя candidate. <br/>
- `req.session.isAuthenticated` - свойство авторизации isAuthenticated, объекта сессии session, которое принимает boolean значение true, что определяет активность сессии. <br/>
- `req.session.save(err => {})` - функция save сохраняющая состояние сессии session. <br/>

⬅️ Подробнее о данном контролере смотрите [Метод запроса post по маршруту '/auth/login'](controllers.md#метод-запроса-post-по-маршруту-authlogin). <br/>

---

### Контроллер "Авторизация" с типом запроса post маршрутизатора '/auth/register'

Контроллер с типом запроса post маршрутизатора '/auth/register' реализовывает регистрацию пользователя. Часть кода контроллера работающий с моделью: <br/>
```node
const User = require('../models/user')
...
    const user = new User({
        email, name, password: hashPassword, cart: {items: []}
      })
      await user.save()
    ...
```
Где: <br/>
- `const user = new User({})` - создание нового экземпляра объекта User, которому присваиваются свойства указаны в теле экземпляра объекта. <br/>
- `email, name, password: hashPassword, cart: {items: []}` - перечень как одноименных свойств ключ-значение(к примеру email) так и разноименных password: hashPassword. <br/>
- `user.save()` - метод save объекта course сохраняющий данные в базе данных MongoDB. <br/>

⬅️ Подробнее о данном контролере смотрите [Метод запроса post по маршруту '/auth/register'](controllers.md#метод-запроса-post-по-маршруту-authregister). <br/>

---

### Контроллер "Авторизация" с типом запроса post маршрутизатора '/auth/password/:token'

Контроллер с типом запроса post маршрутизатора '/auth/password/:token' реализовывает сброс пароля пользователя. Часть кода контроллера работающий с моделью: <br/>
```node
const User = require('../models/user')
...
  const user = await User.findOne({
      resetToken: req.params.token,
      resetTokenExp: {$gt: Date.now()}
    })
...
```
Где:  <br/>
- `User.findOne({})` - метод findOne модели User, который ищет первый объект который подпадает под выполнение условий, заложених в тело метода. <br/>
- `resetToken` - ключ-условие, которое заложено в тело метода и которое сравнивает resetToken с токеном req.params.token, который приходит с телом запроса request.  <br/>
- `resetTokenExp` - ключ-условие, которое заложено в тело метода и которое сравнивает с помощью оператора $gt не привышает ли срок токена больше, чем время в которое выполняется запрос {Date.now()}. <br/>

⬅️ Подробнее о данном контролере смотрите [Метод запроса get по маршруту '/auth/password/:token'](controllers.md#метод-запроса-get-по-маршруту-authpasswordtoken). <br/>

---

### Контроллер "Авторизация" с типом запроса post маршрутизатора '/auth/reset'

Контроллер с типом запроса post маршрутизатора '/auth/reset' реализовывает смену пароля. Часть кода контроллера работающий с моделью: <br/>
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
Где:  <br/>
- `User.findOne({email: req.body.email})` - метод findOne модели User, который сравнивает почту пользвателя email с почтой отправленой в теле запроса req.body.email. <br/>
- `candidate` - объект пользователя найденный методом findOne из модели User. <br/> 
- `candidate.resetToken` - присвание свойству resetTokent объекта candidate значение временного токена token. <br/>
- `candidate.resetTokenExp` - присвание свойству resetTokenExp объекта candidate значение срока жизни токена - Date.now() + 60 * 60 * 1000. <br/>
- `candidate.save()` - метод save сохраняющий объект пользователя candidate в базе данных. <br/>

⬅️ Подробнее о данном контролере смотрите [Метод запроса post по маршруту '/auth/reset'](controllers.md#метод-запроса-post-по-маршруту-authreset). <br/>

---

### Контроллер "Авторизация" с типом запроса post маршрутизатора '/auth/password'

Контроллер с типом запроса post маршрутизатора '/auth/password' реализовывает сохранение нового пароля пользователя. Часть кода контроллера работающий с моделью: <br/>
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
Где:  <br/>
- ` User.findOne({})` - метод findOne модели User, который ищет первый объект который подпадает под выполнение условий, заложених в тело метода. <br/>
- `_id` - ключ принимающий значение ID пользователя приходящий в теле запроса req.body.userId. <br/>
- `resetToken` - ключ-условие, которое заложено в тело метода и которое сравнивает resetToken с токеном req.body.token, который приходит с телом запроса request. <br/>
- `resetTokenExp` - ключ-условие, которое заложено в тело метода и которое сравнивает с помощью оператора $gt не привышает ли срок токена больше, чем время в которое выполняется запрос {Date.now()}. <br/>
- `bcrypt.hash(req.body.password, 10)` - метод hash объекта bcrypt, который шифрует пароля, приходящий в запросе req.body.password в количестве - 10 символов. <br/>
- `user.resetToken` - переопределение свойства resetToken объекта user на значение - undefined. <br/>
- `user.resetTokenExp` - переопределение свойства resetTokenExp объекта user на значение - undefined. <br/>
- `user.save()` - метод save сохраняющий объект пользователя user в базе данных. <br/>

⬅️ Подробнее о данном контролере смотрите [Метод запроса post по маршруту '/auth/password'](controllers.md#метод-запроса-post-по-маршруту-authpassword). <br/>

---

## Реализация модели заказа "User" в промежуточных обработчиках приложения

---

### Промежуточный обработчик registerValidators

Промежуточный обработчик registerValidators валидирует данные при регистрации нового пользователя. Часть кода промежуточного обработчика работающий с моделью: <br/>
```node
const User = require('../models/user')
...
    const user = await User.findOne({email: value})
        if (user) {
            ...
        }
...
```
Где:  <br/>
- ` User.findOne({})` - метод findOne модели User, который ищет первый объект который подпадает под выполнение условия email: value. <br/>
- `if (user) {}` - условие, которое срабатывает, если объёкт user был найден.

### Промежуточный обработчик userMiddleware

Промежуточный обработчик userMiddleware проверяет наличие сессии авторизированного пользователя. Часть кода промежуточного обработчика работающий с моделью: <br/>
```node
const User = require('../models/user')
...
    req.user = await User.findById(req.session.user._id)
    next()
...
```
Где: <br/>
- `User.findById()` - метод findById модели User, который выдаёт результат найденного ID пользователя req.session.user._id в теле запроса req.session. <br/>

⬆️ [К оглавлению раздела "Модели"](#оглавление-раздела) <br/>
↩️ [К оглавлению документации](../README.md) <br/>  
