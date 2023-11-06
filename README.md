<p align="center">
  <a href="https://github.com/shopot/naming-cheatsheet">
    <img src="./js-cheat-sheet.png" alt="Naming cheatsheet" />
  </a>
</p>

# Шпаргалка по именованию

* [Английский язык](#английский-язык)
* [Соглашение об именовании (Naming convention)](#соглашение-об-именовании-naming-convention)
* [S-I-D](#s-i-d)
* [Избегайте сокращений](#избегайте-сокращений)
* [Избегайте дублирования контекста](#избегайте-дублирования-контекста)
* [Отразить ожидаемый результат](#отразить-ожидаемый-результат)
* [Именование функций](#именование-функций)
  * [A/HC/LC Pattern](#ahclc-pattern)
  * [Actions](#actions)
  * [Context](#context)
  * [Prefixes](#prefixes)
* [Единственное и множественное число](#единственное-и-множественное-число)

Перевод [Naming cheatsheet](https://github.com/kettanaito/naming-cheatsheet)

Называть вещи сложно. Этот руководство должно помочь сделать это проще.

> Хотя эти предложения можно применить к любому языку программирования, я буду использовать JavaScript, чтобы проиллюстрировать их на практике.

## Английский язык

Используйте английский язык при именовании переменных и функций.

```js
/* ❌ Bad */
const primerNombre = 'Gustavo';
const amigos = ['Kate', 'John'];

/* ✅ Good */
const firstName = 'Gustavo';
const friends = ['Kate', 'John'];
```

> Нравится вам это или нет, но английский является доминирующим языком в программировании: на английском написан синтаксис всех языков программирования, а также бесчисленное множество документации и учебных материалов. Написав код на английском языке, вы значительно повысите его связность.


## Соглашение об именовании (Naming convention)

В JavaScript для именования переменных и функций используется `camelCase` нотация, для классов и функций-конструкторов `PascalCase` нотация.

```js
/* Bad */
const page_count = 5;
const shouldUpdate = true;
class appLogger {};

/*  Good */
const pageCount = 5;
const shouldUpdate = true;
class AppLogger {};
```

## S-I-D

Имя должно быть _коротким_, _интуитивным_ и _описательным_ (_short_, _intuitive_, _descriptive_):

- **Короткий**. Имя не должно занимать много времени, чтобы набираться и, следовательно, запоминаться;
- **Интуитивный**. Имя должно читаться естественно, как можно ближе к обычной речи;
- **Описательный**. Имя должно наиболее эффективно отражать то, что оно делает/чем обладает.

```js
/* Bad */
const a = 5; // "a" может означать что угодно
const isPaginatable = a > 10; // "Paginatable" звучит крайне неестественно
const shouldPaginatize = a > 10; // Придуманные глаголы — это так весело!

/* Good */
const postCount = 5;
const hasPagination = postCount > 10;
const shouldPaginate = postCount > 10; // альтернатива
```

## Избегайте сокращений

Не используйте сокращения. Они не способствуют ничему, кроме снижения читабельности кода. Найти короткое, описательное имя может быть сложно, но сокращение не является оправданием для отказа от этого.

```js
/* Bad */
const onItmClk = () => {};

/* Good */
const onItemClick = () => {};
```

## Избегайте дублирования контекста

Имя не должно дублировать контекст, в котором оно определено. Всегда удаляйте контекст из имени, если это не ухудшает его читабельность.

```js
class MenuItem {
  /* Bad: Имя метода дублирует контекст (который "MenuItem") */
  handleMenuItemClick = (event) => { ... };

  /* Good: Читается хорошо, как `MenuItem.handleClick()` */
  handleClick = (event) => { ... };
}
```

## Отразить ожидаемый результат

Имя должно отражать ожидаемый результат.

```jsx
/* Bad */
const isEnabled = itemCount > 3;
return <Button disabled={!isEnabled} />;

/* Good */
const isDisabled = itemCount <= 3;
return <Button disabled={isDisabled} />;
```

---

# Именование функций

## A/HC/LC Pattern

Существует полезный шаблон, которому следует следовать при именовании функций:

```
prefix? + action (A) + high context (HC) + low context? (LC)
```

Посмотрите, как можно применить этот шаблон, в таблице ниже.

| Name                   | Prefix   | Action (A) | High context (HC) | Low context (LC) |
| ---------------------- | -------- | ---------- | ----------------- | ---------------- |
| `getUser`              |          | `get`      | `User`            |                  |
| `getUserMessages`      |          | `get`      | `User`            | `Messages`       |
| `handleClickOutside`   |          | `handle`   | `Click`           | `Outside`        |
| `shouldDisplayMessage` | `should` | `Display`  | `Message`         |                  |

> **Примечание**. Порядок контекста влияет на значение переменной. Например, `shouldUpdateComponent` означает, что _вы_ собираетесь обновить компонент, а `shouldComponentUpdate` сообщает вам, что _comComponent_ обновит себя, и вы контролируете только _когда_ он должен обновиться.
> Другими словами, **High context (HC) подчеркивает значение переменной**.

## Actions

Глагольная часть имени вашей функции. Самая важная часть, отвечающая за описание того, что _делает_ функция.

### `get`

Немедленный доступ к данным (т. е. сокращенный метод получения внутренних данных).

```js
function getFruitCount() {
  return this.fruits.length;
}
```

> Смотрите также [compose](#compose).

Вы также можете использовать `get` при выполнении асинхронных операций:

```js
async function getUser(id) {
  return await fetch(`/api/user/${id}`);
}
```

### `set`

Устанавливает переменную декларативным способом со значением `A` в значение `B`.

```js
let fruits = 0;

function setFruits(nextFruits) {
  fruits = nextFruits;
}

setFruits(5);

console.log(fruits); // 5
```

### `reset`

Возвращает переменную в исходное значение или состояние.

```js
const initialFruits = 5;

let fruits = initialFruits;

setFruits(10);

console.log(fruits); // 10

function resetFruits() {
  fruits = initialFruits;
}

resetFruits();

console.log(fruits); // 5
```

### `remove`

Удаляет что-то _от_куда-то.

Например, если у вас есть коллекция выбранных фильтров на странице поиска, удаление одного из них из коллекции - это `removeFilter`, а не `deleteFilter` (и именно так вы, естественно, скажете это и на английском языке):

```js
function removeFilter(filterName, filters) {
  return filters.filter((name) => name !== filterName);
}

const selectedFilters = ['price', 'availability', 'size'];

removeFilter('price', selectedFilters);
```

> Смотрите также [delete](#delete).

### `delete`

Полностью стирает что-то из сфер существования.

Представьте, что вы редактор контента, и есть пресловутый пост, от которого вы хотите избавиться.
После того, как вы нажали блестящую кнопку «Удалить сообщение», CMS выполнила действие `deletePost`, а не `removePost`.

```js
function deletePost(id) {
  return database.find({ id }).delete();
}
```

> Смотрите также [remove](#remove).

> **`remove` or `delete`?**
> Если разница между `remove` и `delete` для вас не столь очевидна, я бы предложил взглянуть на их противоположные действия - `add` и `create`.
> Ключевое различие между `add` и `create` заключается в том, что `add` требует назначения, а `create` **не требует места назначения**. Вы `добавляете` элемент _куда-то_, но не "`создаете` его _куда-то_".
> Просто соедините `remove` с `add` и `delete` с `create`.
>
> Подробно объяснено [здесь (анг)](https://github.com/kettanaito/naming-cheatsheet/issues/74#issue-1174942962).

### `compose`

Создает новые данные из существующих. В основном применимо к строкам, объектам или функциям.

```js
function composePageUrl(pageName, pageId) {
  return pageName.toLowerCase() + '-' + pageId;
}
```

> Смотрите также [get](#get).

### `handle`

Обрабатывает действие. Часто используется при названии метода обратного вызова.

```js
function handleLinkClick() {
  console.log('Clicked a link!');
}

link.addEventListener('click', handleLinkClick)
```

---

## Context

Домен, в котором работает функция.

Функция часто представляет собой действие над чем-то. Важно указать, какова его рабочая область или, по крайней мере, ожидаемый тип данных.

```js
/* Чистая функция, работающая с примитивами */
function filter(list, predicate) {
  return list.filter(predicate);
}

/* Функция, работающая именно с постами */
function getRecentPosts(posts) {
  return filter(posts, (post) => post.date === Date.now());
}
```

> Некоторые предположения, специфичные для языка, могут позволить опустить контекст. Например, в JavaScript обычно `filter` работает с массивом. Добавление явного `filterArray` было бы ненужным.

---

## Prefixes

Префикс расширяет значение переменной. Он редко используется в именах функций (практически никогда).

### `is`

Описывает характеристику или состояние текущего контекста (обычно логическое значение `boolean`).

```js
const color = 'blue';
const isBlue = color === 'blue'; // характеристика
const isPresent = true; // состояние

if (isBlue && isPresent) {
  console.log('Blue is present!');
}
```

### `has`

Описывает, обладает ли текущий контекст определенным значением или состоянием (обычно логическое значение `boolean`).

```js
/* Bad */
const isProductsExist = productsCount > 0;
const areProductsPresent = productsCount > 0;

/* Good */
const hasProducts = productsCount > 0;
```

### `should`

Отражает положительное условное утверждение (обычно логическое `boolean`), связанное с определенным действием.

```js
function shouldUpdateUrl(url, expectedUrl) {
  return url !== expectedUrl;
}
```

### `min`/`max`

Представляет минимальное или максимальное значение. Используется при описании границ или пределов.

```js
/**
 * Отображает случайное количество сообщений внутри
 * заданных границы мин/макс.
 */
function renderPosts(posts, minPosts, maxPosts) {
  return posts.slice(0, randomBetween(minPosts, maxPosts));
}
```

### `prev`/`next`

Укажите предыдущее или следующее состояние переменной в текущем контексте. Используется при описании переходов состояний.

```jsx
async function getPosts() {
  const prevPosts = this.state.posts;

  const latestPosts = await fetch('...');

  const nextPosts = concat(prevPosts, latestPosts);

  this.setState({ posts: nextPosts });
}
```

## Единственное и множественное число

Как и префикс, имена переменных могут быть записаны в единственном или множественном числе в зависимости от того, содержат ли они одно или несколько значений.

```js
/* Bad */
const friends = 'Bob';
const friend = ['Bob', 'Tony', 'Tanya'];

/* Good */
const friend = 'Bob';
const friends = ['Bob', 'Tony', 'Tanya'];
```
