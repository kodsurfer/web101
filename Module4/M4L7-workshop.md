# Лекция-воркшоп: Собираем корзину товаров + мета-теги для соцсетей

Сегодня мы превратим статичную страницу товаров в полноценный магазин с корзиной на `localStorage`. 
Разберём готовый код, исправим баги, добавим счётчик и научимся настраивать мета-теги для красивых превью в соцсетях (Open Graph).

---

## 1. Разбор кода

Ниже представлен полный код страницы товаров, стилей и логики корзины, который мы будем анализировать и улучшать.

### 1.1 HTML-разметка (`products.html`)

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Товары</title>
    <link rel="stylesheet" href="stylesheets/style.css" />
    <link rel="stylesheet" href="stylesheets/products.css" />
    <script src="javascripts/scripts.js"></script>
    <script src="javascripts/products.js"></script>
  </head>
  <body>
    <nav>
      <div id="burger"></div>
      <div class="main-menu">
        <a href="index.html">Главная</a>
        <div class="links">
          <a href="about.html">О музее</a>
          <a href="projects.html">Проекты</a>
          <a href="excursions.html">Экскурсии</a>
          <a href="contacts.html">Контакты</a>
          <a href="archive.html">Архив</a>
          <div class="cart-count">🧺</div>
        </div>
      </div>
    </nav>

    <section id="product-list"></section>
  </body>
</html>
```

**Что здесь важно:**
- В хедере подключаются два CSS-файла и два JS.
- Внутри навигации есть элемент `.cart-count` – сюда будет вставляться количество товаров.
- Основной контейнер для товаров – `<section id="product-list">`.

### 1.2 Стили (`products.css`)

```css
#product-list {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 20px;
}
.product {
  width: 30%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.product img {
  width: 100%;
}
.product-text {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
.buttons {
  display: flex;
  gap: 30px;
  align-items: center;
  justify-content: center;
  background-color: #eee;
  border-radius: 40px;
  width: 140px;
  padding: 14px 5px;
}
.buttons button {
  background-color: transparent;
  border: none;
  font-size: 20px;
  cursor: pointer;
  height: 100%;
  width: 20px;
}
.buttons p {
  user-select: none;
}
```

**Разбор стилей:**
- `#product-list` – гибкая сетка с отступами, карточки переносятся.
- `.product` – ширина 30% (3 карточки в ряд), внутренняя структура колонкой.
- `.buttons` – блок с кнопками «+» и «-» сделан в виде pill-элемента с серым фоном.

### 1.3 Логика корзины (`products.js`)

```javascript
document.addEventListener('DOMContentLoaded', () => {
  renderProducts()
  updateCartCount()
})

const products = [
  {
    id: 1,
    name: 'Футболка',
    price: 3500,
    image: 'https://static.insales-cdn.com/images/products/1/5982/769103710/DSCF0096.jpg'
  },
  {
    id: 2,
    name: 'Кружка',
    price: 2000,
    image: 'https://sublimagia.ru/image/cache/catalog/mugs/2397-600x600.jpg'
  },
  {
    id: 3,
    name: 'Пин',
    price: 1500,
    image: 'https://rockbunker.ru/upload/iblock/55f/9xbv232jog0ezdl846iq77swlw4rxhui.jpg'
  }
]

function renderProducts() {
  const productList = document.querySelector('#product-list')
  productList.innerHTML = ''

  products.forEach((product) => {
    const quantity = getProductCount(product.id)

    const productCard = document.createElement('div')
    productCard.classList.add('card')
    productCard.classList.add('product')

    productCard.innerHTML = `
        <img src="${product.image}" alt="${product.name}" />
        <div class="product-text">
          <h3>${product.name}</h3>
          <p>${product.price}</p>
          <div class="buttons">
            <button onclick='removeFromCart(${product.id})'>-</button>
            <p>${quantity}</p>
            <button onclick='addToCart(${product.id})'>+</button>
          </div>
        </div>
    `

    productList.appendChild(productCard)
  })
}

function getCart() {
  return JSON.parse(localStorage.getItem('cart') || '[]')
}

function getProductCount(productID) {
  let cart = getCart()
  const item = cart.find((p) => p.id === productID)
  return item ? item.quantity : 0
}

function removeFromCart(productID) {
  let cart = getCart()
  console.log(cart)

  const index = cart.findIndex((p) => p.id === productID)

  if (index != -1) {
    cart[index].quantity -= 1
  } else {
    cart.splice(index, 0)
  }

  setCart(cart)
}

function addToCart(productID) {
  let cart = getCart()

  const index = cart.findIndex((p) => p.id === productID)

  if (index != -1) {
    cart[index].quantity += 1
  } else {
    const item = products.find((p) => p.id === productID)

    if (item) {
      cart.push({ ...item, quantity: 1 })
    }
  }

  setCart(cart)
}

function setCart(cart) {
  localStorage.setItem('cart', JSON.stringify(cart))
  updateCartCount()
  renderProducts()
}

function updateCartCount() {
  let cart = getCart()

  const count = cart.reduce((sum, item) => sum + (item.quantity || 0), 0)

  if (count != 0) {
    document.querySelector('.cart-count').innerHTML = `🧺 ${count}`
  }
}
```

**Ключевые моменты:**
- `renderProducts()` – динамически создаёт карточки товаров, подставляя текущее количество из корзины.
- `getCart()` / `setCart()` – чтение и запись в `localStorage`.
- `addToCart()` – либо увеличивает `quantity`, либо добавляет новый товар (с копированием всех полей через `...item`).
- `updateCartCount()` – считает общее количество товаров (сумма всех `quantity`) и обновляет иконку корзины.

**Обнаруженный баг**:  
В `removeFromCart()` количество может уйти в минус, и нет удаления товара, если стало 0. Кроме того, странный `else` с `cart.splice(index, 0)`.

---

## 2. Исправляем баг в `removeFromCart`

Исходный код имел ошибку:  
- При уменьшении количества не проверялось, что количество не уходит в минус.
- Блок `else` (`cart.splice(index, 0)`) делал непонятные вещи.

**Исправленная версия:**

```javascript
function removeFromCart(productID) {
  let cart = getCart();
  const index = cart.findIndex(p => p.id === productID);

  if (index !== -1) {
    if (cart[index].quantity > 0) {
      cart[index].quantity -= 1;
    }
    // Если количество стало 0 – убираем товар из корзины полностью
    if (cart[index].quantity === 0) {
      cart.splice(index, 1);
    }
  }
  setCart(cart);
}
```

**Что изменилось?**  
- Количество не станет отрицательным.
- Когда количество доходит до 0, товар удаляется из массива корзины (иначе он бы висел с `quantity: 0`).

---

## 3. Дополнительные улучшения корзины

### 3.1 Очистка корзины (кнопка «Очистить всё»)

Добавим на страницу кнопку и функцию:

```javascript
function clearCart() {
  localStorage.removeItem('cart');
  updateCartCount();
  renderProducts();
}
```

В HTML где-нибудь: `<button onclick="clearCart()">Очистить корзину</button>`

### 3.2 Страница корзины с итоговой суммой

Создайте `cart.html`, где выводится список товаров из корзины с ценами и суммой.  
Это хорошее задание для студентов.

---

## 4. Оформляем мета-теги (Open Graph)

Когда вы делитесь ссылкой на сайт в соцсетях (Telegram, Facebook, VK), без мета-тегов будет просто ссылка. Чтобы появилась картинка, заголовок и описание, нужны **OG-теги**.

### 4.1 Генерация мета-тегов

Используйте сервисы:
- [metatags.io](https://metatags.io/) – удобный генератор.
- [ogp.me](https://ogp.me/) – спецификация.
- [Яндекс.Вебмастер](https://yandex.ru/support/webmaster/ru/open-graph/intro-open-graph.html) – руководство.

### 4.2 Пример мета-тегов для `products.html`

```html
<head>
  <!-- Стандартные метатеги -->
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Магазин мерча – Огни Москвы</title>
  <meta name="description" content="Футболки, кружки, пины с символикой музея «Огни Москвы». Поддержите музей!">

  <!-- Open Graph для соцсетей -->
  <meta property="og:title" content="Магазин мерча музея «Огни Москвы»">
  <meta property="og:description" content="Купить сувениры с историей: футболки, кружки, значки. Все средства идут на развитие музея.">
  <meta property="og:image" content="https://ваш-сайт.ru/images/og-preview.jpg">
  <meta property="og:url" content="https://ваш-сайт.ru/products.html">
  <meta property="og:type" content="website">
  <meta property="og:locale" content="ru_RU">

  <!-- Твиттер (опционально) -->
  <meta name="twitter:card" content="summary_large_image">
</head>
```

**Советы:**
- Картинка `og:image` должна быть не меньше 600×315 пикселей, желательно 1200×630.
- Используйте абсолютные URL (с `https://`).
- Обязательно указывайте `og:url`, чтобы соцсети правильно группировали лайки.

### 4.3 Почему это важно для интернет-магазина?

Когда пользователь кинет ссылку на товар другу в мессенджер, тот сразу увидит картинку и цену – вероятность перехода выше в 3-5 раз.

---

## 5. Задание для студентов (воркшоп)

Прямо сейчас сделайте следующее:

### 5.1 Исправьте код корзины
- Скопируйте приведённый выше корректный `removeFromCart`.
- Добавьте функцию `clearCart()` и кнопку для её вызова.

### 5.2 Добавьте подсчёт общей суммы корзины
Создайте на странице блок `<div id="cart-total"></div>` и напишите функцию:

```javascript
function updateCartTotal() {
  let cart = getCart();
  let total = cart.reduce((sum, item) => sum + item.price * item.quantity, 0);
  const totalEl = document.querySelector('#cart-total');
  if (totalEl) totalEl.innerHTML = `Общая сумма: ${total} ₽`;
}
```

Вызывайте её внутри `setCart()` после обновления счётчика.

### 5.3 Добавьте мета-теги для всех страниц (не только для товаров)
Для каждой страницы (`index.html`, `about.html`, `excursions.html` и т.д.) укажите свой `og:title` и `og:description`, а также общее `og:image` (логотип музея).

### 5.4 Проверьте результат через Facebook Sharing Debugger
Зайдите на [https://developers.facebook.com/tools/debug/](https://developers.facebook.com/tools/debug/), вставьте ссылку на свою страницу и убедитесь, что Open Graph работает.

### 5.5 Бонус: сделайте отдельную страницу корзины
Создайте `cart.html`, на которой отображаются:
- Список добавленных товаров с картинками, названиями, ценами, количеством.
- Кнопки «+» / «-» прямо на странице корзины.
- Итоговая сумма.
- Кнопка «Оформить заказ» (пока просто alert).

---

## 6. Частые ошибки и их решение

| Ошибка | Почему возникает | Исправление |
|--------|----------------|-------------|
| При перезагрузке товары исчезают из корзины | Не вызывается `renderProducts()` в `setCart()` | Уже есть – `setCart` вызывает `renderProducts()` |
| Счётчик корзины не обновляется | Забыли вызвать `updateCartCount()` после изменения | Мы вызываем внутри `setCart` |
| `localStorage` не сохраняет данные | Используете `sessionStorage` или забыли `JSON.stringify` | У нас `localStorage.setItem('cart', JSON.stringify(cart))` |
| Картинки не загружаются в корзине | Не сохранили поле `image` в объект корзины | В `addToCart` при добавлении нового товара: `cart.push({ ...item, quantity: 1 })` – оператор `...` копирует все поля, включая `image` |

---

## 7. Итоги и следующий шаг

Сегодня мы:
- Разобрали полный цикл корзины на `localStorage` (CRUD: create, read, update, delete).
- Починили баги с отрицательным количеством.
- Научились добавлять мета-теги Open Graph для соцсетей.
- Получили задание сделать страницу корзины и тотальный сумматор.

**Почему это важно?**  
Корзина – основа любого интернет-магазина. Понимание работы с `localStorage` и обновлением UI через рендер – ключевой навык фронтенд-разработчика.

**Домашнее задание:**  
Доработайте проект так, чтобы товары в корзине можно было удалять по одному (кнопка «Удалить» рядом с каждым товаром на странице корзины). Используйте `filter` для удаления.

**Жду ваши вопросы и готовые решения в общем чате!** Удачи с вёрсткой и мерчем музея «Огни Москвы» 🔥🛒
