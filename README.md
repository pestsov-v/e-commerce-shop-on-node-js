# Интернет-магазин курсов

Проект реализущий приложение интернет-магазина курсов. Приложение разработано с учётом перечня всего функционала интернет магазина, в том числе корзина, заказы и организация профиля пользователя. README описан в формате, который по строково объясняет значение той или иной логики кода. Уровень описания проекта в README предполагает, что читающий его является или Junior-разработчиком или новичком. Для иных же читателей README упускал бы моменты разъяснение тех или иных методов, а лишь объяснял общую логику того или иного блока кода. 

## Оглавление документации 
  - [Оглавление документации](#оглавление-документации)
  - [Применение](#применение)
  - [Запустить проект у себя](#запустить-проект-у-себя)
    - [Краткая информация по разделам](#краткая-информация-по-разделам)
  - [Вводные данные](sections.md/inputData.md)
  - [Представления](sections.md/views.md)
  - [Маршуртизаторы](sections.md/routes.md)
  - [Контроллеры](sections.md/controllers.md)
  - [Промежуточные обработчики](sections.md/middleware.md)
  - [Модели](sections.md/models.md)
  - [Проект на Heroku](#проект-в-heroku)

## Применение

Проект показывает реализацию следующий промежуточных обработчиков и зависимостей MVC-паттерна:
- Вариант визуализации, за счёт применение шаблонизатора `Handlebars`.
- Создание контроллеров и маршрутизации.
- Применение `csurf`. `csurf` - промежуточный обработчик для валидации и сессийности.
- Применение `connect-flash`. `connect-flash` - промежуточный обработчик для валидации, который формирует подсказки исходя из действий пользователя.
- Применение `express-session`. `express-session` - промежуточный обработчик cессии.
- Применение `connect-mongodb-session`. `connect-mongodb-session` - промежуточный обработчик сессий для MongoDB.
- Применение `bcryptjs`. `bcryptjs` - промежуточный обработчик шифрующий пароли.
- Применение `express-validator`. `express-validator` - промежуточный обработчик для валидации.
- Применение `helmet`. `helmet` - промежуточный обработчик предоставляющий различные заголовки HTTPS протокола.
- Применение `multer`. `multer` - промежуточный обработчик использующийся для загружение данных.
- Применение `nodemailer`. `nodemailer` - промежуточный обработчик для електронной почты.
- Подключение и управление базой данной MongoDB с схемообразованием. 

---

Приложение разработано на серверной платформе `Node.js` с включением базы данных `MongoDB`, шаблонизатора `Handlebars` и различных `middlware` модулей. <br/>
Приложение выполнено по MVC-паттерну где: <br/>
- M - Model или модель. Модель представляет из себя схему, которая определяет перечень принимаемых и обрабатываемых данных. Также модель может включает в себя методы определяемые в конкретной модели. Такими методами могут быть, к примеру, перевод формата ID в базе данных MongoDB в формат ID принимаемый `Node.js` серверов. <br/>
- V - View или представления. Преставления визуализируют данные, тем самым позволяя разработчику более удобный взгляд и проверку реализации тех или иных функций и методов. <br/>
- С - Controller или контроллер. Контроллер это обработчик данных, который в зависимости от типа запроса и маршрута определяет функционал реализации. <br/>

---

Базовое понимание MVC-паттерна замыкается, на трёх блоках - модели, представления и контроллере. В свою очередь блоки имеют следующие между собой зависимости: <br/>
- Пользовательские взаимодействия с представлениями определяют работу контроллера. <br/>
- Контроллер определяет функционал обращения к модели. <br/>
- Модель предоставляет необходимые данные представлению. <br/>
Приложение же является более сложным, чем базовое понимание и поэтому включает в себя блок промежутоных обработчиков (middleware). Промежучтоные обработчики являются дополнительными обработчиками контроллеров к обращению к моделям. <br/>

Общая схема работы паттерна описана ниже: <br/>

[![1.png](https://i.postimg.cc/L8P1rNX5/1.png)](https://postimg.cc/XrVq5K96)

Запуск сервера осуществляется следующей логикой: <br/>
```node
const express = require('express')

const app = express()
const PORT = process.env.PORT || 3000

app.listen(PORT, () => {
    console.log(`Server is running on port ${PORT}`)
    })
```
Где: <br/>
- `app` - экземпляр функции `express()`. <br/>
- `PORT` - номер `localhost` для локального разворачивания приложения или `process.env.PORT` в случае запуска в режиме готового приложения. <br/>

---

Подробно о структуре приложения и описания логики кода смотрите ниже, выбирая нужный раздел.

## Запустить проект у себя

Чтобы развернуть приложение у себя необходимо.


## Краткая информация по разделам

### Представления

---

Представления визуализируют данные. Представления могут быть организованы различными шаблонизаторами, организацией API и front-end приложения на отдельном localhost или же с помощью HTML фреймворков, таких как pug. <br/>
В приложении представление реализовано через шаблонизатор `Handlebars`. Подробно о представлениях смотрите раздел [Представления](sections.md/views.md). <br/>

### Маршрутизаторы

---

Маршутизаторы определяют метод запроса и машрут визуализации данных. Машрутизаторы могут быть определены через различные пакеты или через объект `Router` модуля `express`. <br/>
В приложении маршрутизаторы представлены через объект `Router`. Подробно о маршрутизаторы смотрите раздел [Маршрутизаторы](sections.md/routes.md) <br/>

### Контроллеры

---

Контроллеры определяют обработку данных, которые отправляются, принимаются или удаляются исходя из маршрута и типа запроса. Подробно о контроллерах смотрите раздел [Контроллеры](sections.md/controllers.md) <br/>

### Промежуточные обработчики

---

Промежуточные обработчики представляют допольнительную обработку данных, которые отправляются, принимаются или удаляются исходя из маршрута и типа запроса. Подробно о промежуточных обработчиках смотрите раздел [Промежуточные обработчики](sections.md/middleware.md) <br/>

### Модели

---

Модели формируют схему оформления данных, которые заполняются, изменяются или извлекаются из базы данных. Подробно о моделях смотрите раздел [Модели](sections.md/models.md) <br/>

## Проект на Heroku
Проект запущен не PaaS сервисе Heroku. <br/> 
[Ссылка на проект](https://stark-wildwood-85340.herokuapp.com/)

