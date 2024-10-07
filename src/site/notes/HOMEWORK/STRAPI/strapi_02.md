---
{"dg-publish":true,"permalink":"/homework/strapi/strapi-02/","tags":["gardenEntry"]}
---

- [x]  Есть скриншоты с основными ключевыми этапами;
- [x]  Скриншоты прокомментированы;
- [x]  К первому дедлайну реализована как минимум одна сущность;
- [x]  Ко второму дедлайну реализован весь проект;
- [x]  Противоречия в ТЗ (при наличии) прокомментированы;
- [x]  Отчет отформатирован.
# Общие положения и идея
«ComicSphere - приложение для любителей комиксов, где пользователи могут делиться своими любимыми комиксами, обсуждать их, оставлять отзывы и создавать клубы по интересам» - из описания все звучит вполне реализуемо, кроме пункта обсуждения. Обсуждения можно реализовать через модуль комментариев, где мы введем сущность Комментарий. Однако Strapi не предоставляет встроенных средств для обсуждения в реальном времени (на примере чата или форума), поэтому обсуждения будут работать как стандартные комментарии, отправляемые через API.
## Сущность "Пользователь" и трудности реализации
Strapi по умолчанию имеет систему управления пользователями через плагин "Users & Permissions". Однако, для некоторых полей потребуется кастомизация.

Также необходимо создать поле с паролем, иначе как проходить регистрацию и дальнейшую публикацию материалов?

Также есть роли «Администратор, гость, пользователь», а поле с ролью нет.

Также добавил бы email для более простого и удобного входа пользователя

Остальное в сущности «Пользователь» все реализуемо.
## Сущность "Рейтинг" и трудности реализации
Проблема, с которой я столкнулся при реализации сущности рейтинг – расчет среднего рейтинга, который требует дополнительной логики, так как Strapi не выполняет сложные вычисления из. Коробки. Придется настроить свою логику для расчета среднего значения на основе оценок пользователей или же убрать графу. Я выберу второй вариант.
## Сущность "Группа" и трудности реализации
Трудностей с реализацией этой сущности не возникло
## Сущность "Сообщение" и трудности реализации
Как я и писал выше сообщения между пользователями не поддерживаются, равно как и не поддерживается общение в реальном времени. Поэтому придется слегка изменить реализацию этой функции – а точнее изменить на комментарии и реализовывать полноценный функционал комментариев.
## Сущность "Подписка" и трудности реализации
Сущность подписка реализована

# Источники данных и их реализация 
Strapi по умолчанию поддерживает базу данных для хранения информации о пользователях, используя SQL (например, PostgreSQL, MySQL). Также можно кастомизировать профиль пользователя и хранить больше информации, чем стандартная система аутентификации.

Strapi идеально подходит для создания и управления базой данных комиксов. Можно создать контентный тип "Комикс", который будет содержать поля для хранения всех нужных данных.

Strapi предоставляет встроенный **плагин Upload**, который позволяет загружать и хранить медиафайлы, включая изображения. Медиа можно сохранять в локальном хранилище или интегрировать с облачными сервисами (например, Amazon S3, Cloudinary).

Проблема заключается с интеграцией с социальными сетями, приведенными в изначальном проекте, а именно внешние API (например, API Facebook*, Twitter* и Instagram*). Реализация возможна с иными соц сетями – Одноклассники, вКонтакте

*Meta Platforms Inc. признана экстремистской организацией на территории РФ

# Кастомная валидация
Реализуема в полном объеме согласно изначальному проекту. Корректировки не требуются
# Реализация
1. С помощью yarn (который я установил через BREW) и команды `yarn create strapi-app my-strapi-project --quickstart` начинаем скрипт установки Strapi
2. Регистрируемся с помощью GitHub
3. Переходим в аудиторию с папкой моего проекта
4. Вводим команду  `yarn develop`
![Pasted image 20240917113755.png](/img/user/Pasted%20image%2020240917113755.png)
5. Через Localhost через порт 1337 попадаем в StrapiDashboard ![Pasted image 20240917113824.png](/img/user/Pasted%20image%2020240917113824.png)
6. Переходим в Content-Type Builder. Создаем сущность User1 и создаем поля. Все скриншоты ниже прокомментированы 
![Pasted image 20240917113850.png](/img/user/Pasted%20image%2020240917113850.png)
Создаем сущности, с которыми связан User1, чтобы в поле можно было показать их взаимосвязь.
![Pasted image 20240917113920.png](/img/user/Pasted%20image%2020240917113920.png)
Создали поля, показали взаимосвязь
![Pasted image 20240917114046.png](/img/user/Pasted%20image%2020240917114046.png)
Для поля username указываем обязательно уникальность и обязательное заполнение
![Pasted image 20240917114454.png](/img/user/Pasted%20image%2020240917114454.png)
Аналогично для поля e-mail

## Реализация подписки 
![Снимок экрана 2024-09-25 в 13.16.58.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-25%20%D0%B2%2013.16.58.png)
Для сущности комикс создаем взаимоотношение с сущностью пользователь Многие-ко-многим 

![Снимок экрана 2024-09-25 в 13.17.57.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-25%20%D0%B2%2013.17.57.png)
Для пользователя создаем отношение к самому себе - многие ко многим 
![Снимок экрана 2024-09-25 в 13.23.39.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-25%20%D0%B2%2013.23.39.png)
Создаем три пользователя и 2 комикса. На второй комикс подписаны три пользователя. 
![Снимок экрана 2024-09-25 в 13.23.55.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-25%20%D0%B2%2013.23.55.png)На первый только Vasya2, аналогично смотрим в поле самого пользователя его любимые комиксы ( на которые он подписан)
![Снимок экрана 2024-09-25 в 13.24.12.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-25%20%D0%B2%2013.24.12.png)
К примеру Vasya2 он подписан и на первый, и на второй. Подписки реализованы. 
![Pasted image 20240925133634.png](/img/user/Pasted%20image%2020240925133634.png)
Также пользователи могут подписывать друг на друга и следить за новыми комментариями и публикациями новых комиксов. 

# REST 
Для сущности **Пользователь** можно создать следующие REST методы, обеспечивающие CRUD (Create, Read, Update, Delete) операции и дополнительные действия, связанные с активностью пользователя. Рассмотрим по порядку те, которые будут использоваться в моем проекте.

## POST /users/{user_id}

Создает нового пользователя.
Описание: Используется для регистрации нового пользователя в системе.
 ![Снимок экрана 2024-09-24 в 01.45.29 1.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-24%20%D0%B2%2001.45.29%201.png)

## DELETE /users/{user_id}

Удаление пользователя.
Описание: Удаляет пользователя с указанным user_id из системы.
 ![Снимок экрана 2024-09-24 в 01.46.50.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-24%20%D0%B2%2001.46.50.png)
## PUT /users/{user_id}

Обновление информации о пользователе.
Описание: Позволяет обновить данные пользователя, например, его никнейм или подписки
![Снимок экрана 2024-09-18 в 09.28.02.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-18%20%D0%B2%2009.28.02.png)

## GET /users/{user_id}

Получение информации о конкретном пользователе.
Описание: Возвращает информацию о пользователе с указанным user_id.
![Снимок экрана 2024-09-18 в 09.27.26.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-18%20%D0%B2%2009.27.26.png)

Валидация сущности пользователь - не допускать двух одинаковых имен. реализуется не только через панель администратора, но и через код следующим образом: по пути my-project/src/api/user1/controllers/user1.js меняем содержимое файла на 
``` json 

'use strict';

/**
 * user1 controller
 */

const { createCoreController } = require('@strapi/strapi').factories;

module.exports = createCoreController('api::user1.user1', ({ strapi }) => ({
  async create(ctx) {
    const { username } = ctx.request.body;

    // Проверяем, существует ли пользователь с таким именем
    const existingUser = await strapi.db.query('api::user1.user1').findOne({
      where: { username },
    });
	// Если имя не уникально, выводим ошибку
    if (existingUser) {
      return ctx.badRequest('Username already exists');
    }

    // Если имя уникально, продолжаем создание пользователя
    const entity = await super.create(ctx);
    return entity;
  },
}));
```
Сохраняем, обновляем и проверяем 
Аналогично создаем другие сущности. ![Снимок экрана 2024-09-23 в 12.58.10.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-23%20%D0%B2%2012.58.10.png)

![Снимок экрана 2024-09-24 в 01.45.40.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-24%20%D0%B2%2001.45.40.png)
Валидация реализована 
## Сущность "Комикс"
![Снимок экрана 2024-09-23 в 11.35.16.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-23%20%D0%B2%2011.35.16.png)
Обязательно добавляем Медиа, так как будем использовать обложки комиксов. 
![Снимок экрана 2024-09-23 в 11.38.09.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-23%20%D0%B2%2011.38.09.png)
![Снимок экрана 2024-09-24 в 10.42.49.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-24%20%D0%B2%2010.42.49.png)
Добавили медиа
Дата публикации 
![Снимок экрана 2024-09-23 в 11.38.47.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-23%20%D0%B2%2011.38.47.png)
У количества страниц указываем формат integer
![Снимок экрана 2024-09-23 в 11.39.20.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-23%20%D0%B2%2011.39.20.png)
Рейтинг будет float, так как будем брать среднее арифметическое оценок пользователей 

![Снимок экрана 2024-09-23 в 23.17.14.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-23%20%D0%B2%2023.17.14.png)
![Снимок экрана 2024-09-23 в 23.17.34.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-23%20%D0%B2%2023.17.34.png)
У пользователя может быть множество комиксов и у комикса может быть множество оценок


Итоговый набор полей у сущности "Комикс"
![Снимок экрана 2024-09-23 в 23.13.35.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-23%20%D0%B2%2023.13.35.png)

## Rest методы для сущности комикс
![Снимок экрана 2024-09-24 в 12.07.04.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-24%20%D0%B2%2012.07.04.png)
POST - опубликовать
![Снимок экрана 2024-09-24 в 12.08.02.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-24%20%D0%B2%2012.08.02.png)
GET- данные о всех комиксах
![Снимок экрана 2024-09-24 в 12.09.14.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-24%20%D0%B2%2012.09.14.png)
PUT - обновить данные любого поля ( если права пользователя позволяют подобное)
![Снимок экрана 2024-09-24 в 12.09.31.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-24%20%D0%B2%2012.09.31.png)
DELETE - удалить. Скриншот выполнен после повторной команды DELETE, 
## Users & Permissions
![Снимок экрана 2024-09-23 в 12.00.04.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-23%20%D0%B2%2012.00.04.png)
Также нам необходимо установить плагин users&permissions, чтобы выделить 3 основных типа пользователей (гость, зарегистрированный пользователь, админ) и наделить их правами и полномочиями

![Снимок экрана 2024-09-23 в 12.05.23.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-23%20%D0%B2%2012.05.23.png)

 Создаем роль "Зарегистрированный пользователь" и наделяем полномочиями по сущности "Комментарий" - создать и удалить
 по сущности "Рейтинг" - создать, удалить, изменить
 по сущности "Группа" - создать 
 по сущности "Комикс" - создать, удалить, изменить

Создаем роль администратора и наделяем его всеми полномочиями во всех аспектах 
![Снимок экрана 2024-09-23 в 12.19.56.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-23%20%D0%B2%2012.19.56.png)

## Сущность "Комментарий"
![Снимок экрана 2024-09-23 в 12.08.58.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-23%20%D0%B2%2012.08.58.png)
Создаем поле время с указанным на скриншоте типом даты
![Pasted image 20240923234059.png](/img/user/Pasted%20image%2020240923234059.png)
Связь: У одного пользователя может быть несколько комментариев 
![Pasted image 20240923234137.png](/img/user/Pasted%20image%2020240923234137.png)
Итоговый набор полей у сущности. 

Проблема, с которой я столкнулся при реализации валидации этой сущности. 
Как я писал выше - валидацию на конкретную сущность можно сделать только через путь проект/src/api, там выбрать сущность, поменять код json в content type и в контроллере. Или же выставить валидацию через обычную валидацию сущности в админке. Интересный парадокс - если писать код на валидацию - он ровно с таким же успехом не работает, как и при выставлении условий валидации через админку. Итог - валидация в поле text - не работает и проблема конкретно в strapi. 

![Снимок экрана 2024-09-24 в 12.12.50.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-24%20%D0%B2%2012.12.50.png)
Проблема #2 , с которой я столкнулся, точнее баг системы, что через REST запросы ни одна подпись в поле "text" не проходит валидацию - это, конечно, очень забавно))).


## Решение проблемы валидации количества символов
Мною был написан JavaScript код и добавлен в content-types, после этого валидация заработала. 
``` Javascript
module.exports = {

async beforeCreate(event) {

const { data } = event.params;

const { ValidationError } = require('@strapi/utils').errors;

  

// Логируем все данные для отладки

console.log('Event data:', data);

  

// Проверка наличия и типа поля `text`

if (typeof data.text !== 'string') {

throw new ValidationError('Text field must be a string');

}

  

// Логируем текст для отладки

console.log('Extracted text:', data.text);

  

// Проверяем минимальную длину текста

if (data.text.length < 5) {

throw new ValidationError('Review text must be at least 5 characters long');

}

  

// Проверяем максимальную длину текста

if (data.text.length > 500) {

throw new ValidationError('Review text cannot exceed 500 characters');

}

},

  

async beforeUpdate(event) {

const { data } = event.params;

const { ValidationError } = require('@strapi/utils').errors;

  

// Логируем все данные для отладки

console.log('Event data:', data);

  

// Проверка наличия и типа поля `text`

if (typeof data.text !== 'string') {

throw new ValidationError('Text field must be a string');

}

  

// Логируем текст для отладки

console.log('Extracted text:', data.text);

  

// Проверяем минимальную длину текста

if (data.text.length < 5) {

throw new ValidationError('Review text must be at least 5 characters long');

}

  

// Проверяем максимальную длину текста

if (data.text.length > 500) {

throw new ValidationError('Review text cannot exceed 500 characters');

}

},

};
``` 
![Pasted image 20240925133257.png](/img/user/Pasted%20image%2020240925133257.png)
![Pasted image 20240925133312.png](/img/user/Pasted%20image%2020240925133312.png)


## Сущность "Рейтинг"
![Снимок экрана 2024-09-23 в 23.49.45.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-23%20%D0%B2%2023.49.45.png)
Связь один комикс может иметь несколько оценок 
![Снимок экрана 2024-09-23 в 23.49.58.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-23%20%D0%B2%2023.49.58.png)
Связь  пользователь может ставить несколько оценок 
![Снимок экрана 2024-09-23 в 23.51.27.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-23%20%D0%B2%2023.51.27.png)
Поле с оценкой имеет тип integer и валидацию от 1 до 5. Проблема в  изначальном  проекте - валидация по рейтингу не указана. Один пользователь может поставить 5 в надежде, что это лучший результат, второй поставит 100. Итоговый результат будет недостоверен. 
![Pasted image 20240923235422.png](/img/user/Pasted%20image%2020240923235422.png)
Проверка заданной валидации - работает

![Снимок экрана 2024-09-23 в 23.49.31.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-23%20%D0%B2%2023.49.31.png)
Итоговый набор полей у рейтинга

## Реализация среднего рейтинга 
Я попытался по аналогии с валидацией добавить lifecycles.js в content-type рейтинга, но система не заработала, средний рейтинг выставлен по нулям 
``` Javascript
'use strict';

module.exports = {

async afterCreate(event) {

const { result } = event;

await updateAverageRating(result.comic_id);

},

  

async afterUpdate(event) {

const { result } = event;

await updateAverageRating(result.comic_id);

},

  

async afterDelete(event) {

const { result } = event;

await updateAverageRating(result.comic_id);

},

};

  

async function updateAverageRating(comicId) {

try {

// Проверим, что comicId существует

if (!comicId) {

console.error('Comic ID is missing');

return;

}

  

console.log('Fetching ratings for comic ID:', comicId);

const ratings = await strapi.entityService.findMany('api::rating.rating', {

filters: { comic_id: comicId },

fields: ['score'],

});

  

// Логируем полученные рейтинги

console.log('Retrieved ratings:', ratings);

  

// Проверим, если у нас есть рейтинги

if (ratings.length > 0) {

const totalScore = ratings.reduce((sum, rating) => sum + rating.score, 0);

const averageRating = totalScore / ratings.length;

  

// Логируем рассчитанный средний рейтинг

console.log('Calculated average rating:', averageRating);

  

// Обновляем средний рейтинг для комикса

await strapi.entityService.update('api::comic.comic', {

where: { id: comicId }, // Обновляем комикс по ID

data: {

average_rating: averageRating,

},

});

} else {

// Если нет рейтингов, устанавливаем средний рейтинг в 0

console.log('No ratings found, setting average rating to 0');

await strapi.entityService.update('api::comic.comic', {

where: { id: comicId }, // Обновляем комикс по ID

data: {

average_rating: 0,

},

});

}

} catch (error) {

console.error('Error updating average rating:', error);

}

}
``` 

## Rest методы у сущности Рейтинг

![Снимок экрана 2024-09-24 в 12.22.26.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-24%20%D0%B2%2012.22.26.png)
POST - создание 
![Снимок экрана 2024-09-24 в 12.30.15.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-24%20%D0%B2%2012.30.15.png)
GET - доступные рейтинги
![Снимок экрана 2024-09-24 в 12.30.37.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-24%20%D0%B2%2012.30.37.png)DELETE - удаление

## Сущность "Группа"![Снимок экрана 2024-09-24 в 00.06.33.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-24%20%D0%B2%2000.06.33.png)
Связь множество групп - множество пользователей

z

Связь множество администраторов - множество групп
![Снимок экрана 2024-09-24 в 00.07.35.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-24%20%D0%B2%2000.07.35.png)
Итоговый набор полей у сущности "Группа"

## REST методы у сущности Группа 
![Снимок экрана 2024-09-24 в 12.10.34.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-24%20%D0%B2%2012.10.34.png)
POST создание
![Снимок экрана 2024-09-24 в 12.10.55.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-24%20%D0%B2%2012.10.55.png)
PUT - изменение данных 
![Снимок экрана 2024-09-24 в 12.11.16.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-24%20%D0%B2%2012.11.16.png)
GET - информация об актуальных группах 
![Снимок экрана 2024-09-24 в 12.11.29.png](/img/user/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-24%20%D0%B2%2012.11.29.png)
DELETE- удаление 


## Полноценный пользовательский сценарий для ComicSphere

**Сценарий: Регистрация и взаимодействие пользователя**

**Шаг 1: Регистрация нового пользователя**

1.             **Пользователь открывает приложение ComicSphere.**

На главной странице есть кнопка "Регистрация".

2.             **Пользователь нажимает на кнопку "Регистрация".**

Появляется форма регистрации, где нужно ввести:

Никнейм (username)

Пароль

Email (если предусмотрен)

3.             **Пользователь заполняет форму и нажимает "Создать аккаунт".**

Если введенные данные корректны, система создает нового пользователя и перенаправляет его на страницу профиля.

**Шаг 2: Просмотр профиля и подписка на других пользователей**

1.             **Пользователь попадает на свою страницу профиля.**

На странице отображается информация о пользователе (никнейм, количество подписчиков, подписки).

2.             **Пользователь нажимает на "Найти пользователей".**

Открывается список всех зарегистрированных пользователей с кнопками "Подписаться" рядом с каждым.

3.             **Пользователь решает подписаться на другого пользователя.**

Нажимает "Подписаться" рядом с никнеймом другого пользователя.

Система обновляет информацию, и теперь пользователь видит, что он подписан на этого пользователя.

**Шаг 3: Добавление комиксов**

1.             **Пользователь возвращается на свою страницу профиля.**

На странице есть кнопка "Добавить комикс".

2.             **Пользователь нажимает на "Добавить комикс".**

Открывается форма для добавления комикса, где нужно ввести:

Название (title)

Автор (author)

Жанр (genre)

Описание (description)

Дата выхода (release_date)

Количество страниц (page_count)

Обложка (cover_image)

3.             **Пользователь заполняет форму и нажимает "Сохранить".**

Комикс добавляется в базу данных, и пользователь перенаправляется на страницу с комиксами.

**Шаг 4: Оценка комиксов**

1.             **Пользователь просматривает список комиксов.**

На странице отображаются все комиксы, которые были добавлены.

2.             **Пользователь выбирает комикс для оценки.**

Нажимает на комикс и открывается страница с подробной информацией о нем.

3.             **На странице комикса есть возможность оценить его.**

Пользователь выбирает оценку от 1 до 5 и нажимает "Оценить".

4.             **Система обновляет рейтинг комикса.**

После оценки пользователю показывается сообщение об успешном добавлении оценки, и обновляется средний рейтинг комикса.

**Шаг 5: Обсуждение и взаимодействие с сообществом**

1.             **Пользователь может просматривать отзывы других пользователей.**

На странице комикса отображается список отзывов и оценок от других пользователей.

2.             **Пользователь может оставить свой отзыв.**

Ниже списка отзывов есть форма для добавления нового отзыва.

Пользователь заполняет форму и нажимает "Отправить".

3.             **После отправки отзыва пользователь получает подтверждение.**

Отзыв появляется в списке отзывов на странице комикса.

**Шаг 6: Уведомления о новых подписках и активности**

1.             **Пользователь получает уведомления о новых подписках.**

В разделе уведомлений на странице профиля пользователь видит, кто на него подписался.

2.             **Пользователь может видеть активность своих подписчиков.**

На странице профиля есть раздел, который отображает последние действия подписчиков, например, новые комиксы, которые они добавили или оценили.