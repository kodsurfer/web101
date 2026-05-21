## Лекция-воркшоп: Адаптивное меню-бургер и страница 404 для проекта «Музей огней Москвы»

---

## Введение и цели (10 мин)

Мы продолжаем работу над проектом **«Музей огней Москвы»** – это многостраничный сайт, который уже имеет главное меню и несколько разделов. Сегодня сделаем два важных улучшения:

1. **Добавим бургер-меню** – адаптивную навигацию для мобильных устройств (ширина экрана ≤600px). Меню будет плавно выезжать, а три полоски превращаться в крестик.
2. **Создадим собственную страницу 404** – вместо скучной стандартной ошибки сделаем стильную страницу с генерацией случайных изображений и кнопкой «Назад».

**План:**
- Обзор текущего проекта и настройка репозитория.
- Бургер-меню: семантика, медиа-запросы, анимация полосок и выезжающего меню.
- Дополнительно: анимированные стрелки для пунктов навигации.
- Страница 404: вёрстка, стилизация, кнопка «Назад», генерация случайных картинок из архива.
- Исправление типичных ошибок и финальная проверка.

---

## Часть 1. Создание бургер-меню (60 мин)

### 1.1. Добавляем разметку и идентификатор

Откройте файл `index.html` (или общий шаблон). Внутри `<header>` найдите элемент `<nav>` – это основной блок навигации. Добавим кнопку-бургер и присвоим навигации `id="burger"`.

```html
<button class="burger" id="burgerBtn">☰</button>
<nav id="burger">
  <ul>
    <li><a href="/">Главная</a></li>
    <li><a href="/expo.html">Выставки</a></li>
    <li><a href="/lights.html">Огни Москвы</a></li>
    <li><a href="/contacts.html">Контакты</a></li>
  </ul>
</nav>
```

**Важно:** кнопка будет видна только на мобильных устройствах, а на десктопе мы её скроем через CSS.

### 1.2. Базовые стили для бургера и медиа-запрос

Создайте (или дополните) файл `adaptive.css`. Подключите его в `<head>` **после** основного файла стилей (`layout.css`), иначе порядок правил нарушится – это частая ошибка.

```css
/* adaptive.css */
.burger {
  display: none; /* скрываем на широких экранах */
  width: 40px;
  height: 26px;
  margin: 16px;
  cursor: pointer;
  background: none;
  border: none;
  position: relative;
}

/* Адаптив до 600px */
@media (max-width: 600px) {
  .burger {
    display: block;
  }
  
  #burger {
    /* Стили для мобильной версии меню */
    position: fixed;
    top: 0;
    right: 0;
    width: 100%;
    height: 100vh;
    background: rgba(255, 255, 255, 0.95);
    backdrop-filter: blur(8px);
    transform: translateX(100%);
    transition: transform 0.3s ease-in-out;
    z-index: 1000;
  }
  
  #burger.active {
    transform: translateX(0);
  }
}
```

> **Объяснение семантики:** тег `<nav>` обозначает навигационную секцию, в отличие от `<section>`, который группирует произвольный контент. Скринридеры используют `<nav>` для быстрой навигации по сайту.

### 1.3. Стилизация пунктов меню для мобильных

Внутри медиа-запроса (max-width: 600px) добавим правила для списка:

```css
#burger ul {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100%;
  gap: 2rem;
  list-style: none;
  padding: 0;
}

#burger a {
  font-size: 3rem;
  line-height: 3rem;
  text-decoration: none;
  color: #1e2a3e;
  font-weight: 500;
}
```

**Почему `rem`?**  
`rem` (root em) отсчитывается от размера шрифта корневого элемента `<html>`. По умолчанию 1rem = 16px. При изменении базового размера (например, для accessibility) масштабируется всё меню. `em` же зависит от родителя, что приводит к неожиданным результатам. Всегда предпочитайте `rem` для глобальных величин.

### 1.4. Рисуем три полоски бургера без картинок

Мы создадим анимированный бургер, используя псевдоэлементы `::before` и `::after`, а третью полоску сделаем через `box-shadow`. Удалим стандартную надпись «☰» (можно убрать текст из кнопки, оставив пустую).

```css
.burger {
  /* уже задано */
  background: transparent;
}

.burger::before,
.burger::after {
  content: '';
  position: absolute;
  width: 100%;
  height: 2px;
  background-color: #1e2a3e;
  left: 0;
  transition: transform 0.3s ease, top 0.3s ease;
}

.burger::before {
  top: 0;
}

.burger::after {
  bottom: 0;
}

/* третья полоска через тень */
.burger {
  box-shadow: 0 12px 0 #1e2a3e; /* смещение по Y = 12px (половина высоты) */
}
```

**Проверьте:** высота кнопки 26px, полоски толщиной 2px. Чтобы тень оказалась по центру, вычисляем: (26 - 2)/2 = 12px.

### 1.5. Анимация превращения в крестик при клике

Добавим JavaScript. Создадим файл `burger.js` или впишем в общий скрипт.

```javascript
const burgerBtn = document.querySelector('.burger');
const navMenu = document.querySelector('#burger');

burgerBtn.addEventListener('click', () => {
  navMenu.classList.toggle('active');
  burgerBtn.classList.toggle('active');
});
```

Теперь стили для активного состояния бургера (крестик):

```css
.burger.active::before {
  transform: rotate(45deg);
  top: 12px; /* смещаем в центр */
}

.burger.active::after {
  transform: rotate(-45deg);
  bottom: 12px;
}

.burger.active {
  box-shadow: none; /* убираем третью полоску */
}
```

Анимацию `transform` сделаем плавной через `transition`, указанную ранее. Обратите внимание: длительность поворота 0.3 с – комфортная для пользователя.

### 1.6. Исправление порядка стилей – критично!

Если после всех манипуляций меню не работает, проверьте подключение CSS в `<head>`:

```html
<link rel="stylesheet" href="css/layout.css">
<link rel="stylesheet" href="css/adaptive.css"> <!-- адаптив должен быть ПОСЛЕ -->
```

Иначе специфичность селекторов может быть нарушена, и `transform` не переопределится.

### 1.7. Практическое задание (5 мин)

Добавьте на сайт плавное появление подложки меню, измените цвет фона на светло-серый (`#f8f9fa`) и убедитесь, что при открытом бургере скролл страницы блокируется (свойство `overflow: hidden` для `<body>`).

---

## Часть 2. Дополнительная стилизация: стрелки на ссылках (15 мин)

Создадим эффект, когда при наведении на пункт меню появляется стрелка слева.

```css
#burger li {
  position: relative;
}

#burger li a::before {
  content: '';
  position: absolute;
  left: -2rem;
  top: 0;
  width: 3rem;
  height: 3rem;
  background-image: url('../img/arrow.svg'); /* подготовьте иконку стрелки */
  background-size: contain;
  background-repeat: no-repeat;
  opacity: 0;
  transform: translateX(-15px);
  transition: opacity 0.2s ease, transform 0.2s ease;
}

#burger li a:hover::before {
  opacity: 1;
  transform: translateX(0);
}
```

**Убедитесь,** что у ссылки `position: relative` или `absolute` позиционируется относительно родительского `<li>`.

---

## Перерыв (10 мин)

---

## Часть 3. Создание кастомной страницы 404 (60 мин)

### 3.1. Структура HTML

Создайте файл `404.html`. В нём сделайте такую же шапку и бургер-меню, как на основном сайте (чтобы пользователь не терял навигацию). Основной контент:

```html
<div class="wrapper-404">
  <div class="content">
    <section id="text">
      <h1>404</h1>
      <h2>Страница не найдена</h2>
      <p>Возможно, вы ошиблись адресом, или огни Москвы погасли…</p>
      <button id="backBtn" class="back-button">← Назад на главную</button>
    </section>
    <section id="image">
      <!-- Сюда динамически будем вставлять случайное изображение и подпись -->
      <div class="random-objects"></div>
    </section>
  </div>
</div>
```

### 3.2. Стилизация страницы 404

Создайте `404.css` и `404.js`, подключите их.

```css
.wrapper-404 {
  max-width: 1024px;
  margin: 0 auto;
  padding: 2rem;
  display: flex;
  align-items: center;
  min-height: 70vh;
}

.content {
  display: flex;
  gap: 4rem;
  flex-wrap: wrap;
}

#text {
  flex: 1;
}

#image {
  flex: 1;
}

h1 {
  font-size: 6rem;
  margin: 0;
  color: #2c3e66;
}

h2 {
  font-size: 2rem;
  margin-top: 0;
}

.back-button {
  background: #1e2a3e;
  color: white;
  border: none;
  padding: 0.8rem 1.5rem;
  font-size: 1rem;
  cursor: pointer;
  transition: background 0.2s;
}

.back-button:hover {
  background: #2c3e66;
  text-decoration: underline;
}
```

### 3.3. Кнопка «Назад» с историей браузера

В `404.js` напишем:

```javascript
const backBtn = document.getElementById('backBtn');
backBtn.addEventListener('click', () => {
  window.history.back(); // возвращает на предыдущую страницу
  // Если истории нет, можно перенаправить на главную:
  // if (window.history.length === 1) window.location.href = '/';
});
```

### 3.4. Генерация случайного изображения и подписи

Используем архив из папки `img/random/` (преподаватель выдаёт архив с 10+ картинками и подписями). Создадим массив объектов:

```javascript
const elements = [
  { img: 'spasskaya.jpg', caption: 'Спасская башня' },
  { img: 'gum.jpg', caption: 'ГУМ в огнях' },
  { img: 'teatralnaya.jpg', caption: 'Театральная площадь' },
  // ... остальные
];

function getRandomElement() {
  const randomIndex = Math.floor(Math.random() * elements.length);
  return elements[randomIndex];
}

function renderRandomObject() {
  const container = document.querySelector('.random-objects');
  container.innerHTML = ''; // очищаем
  const item = getRandomElement();
  
  const img = document.createElement('img');
  img.src = `img/random/${item.img}`;
  img.alt = item.caption;
  img.style.maxWidth = '100%';
  img.style.borderRadius = '12px';
  
  const caption = document.createElement('p');
  caption.textContent = item.caption;
  caption.style.textAlign = 'center';
  
  container.appendChild(img);
  container.appendChild(caption);
}

renderRandomObject();
```

**Важно:** при каждом обновлении страницы будет показываться новый объект. Можно также добавить кнопку «Показать другой свет».

### 3.5. Исправление типичных ошибок

- Ошибка: `const txt вместо textContent` – переменная `txt` не определена; всегда используйте `.textContent`.
- Проблема: изображения не отображаются из-за неправильного пути. Проверьте папку `img/random` и регистр имён.
- Конфликт стилей: если заголовки H2 на 404 выглядят иначе, чем ожидалось – проверьте глобальные стили. Используйте специфичные селекторы для страницы 404 (например, `.wrapper-404 h2`).

### 3.6. Практическое задание (10 мин)

1. Добавьте на страницу 404 адаптивное бургер-меню (скопируйте код из основной части).
2. Сделайте так, чтобы при клике на «Назад» также закрывалось меню, если оно открыто.
3. Реализуйте анимацию появления случайной картинки (плавное появление).

---

## Завершение и подведение итогов (10 мин)

Проверьте работу:
- На ширине экрана менее 600px появляется бургер; при клике меню выезжает, полоски превращаются в крестик.
- На 404 странице генерируется случайная достопримечательность с огнями Москвы.
- Кнопка «Назад» возвращает пользователя на предыдущую страницу (или на главную).

**Что мы изучили:**
- Создание анимированного бургер-меню без использования JS-библиотек.
- Применение медиа-запросов и псевдоэлементов.
- Разницу между `rem` и `em`.
- Принцип работы кастомной страницы ошибки 404 с динамическим контентом.
- Генерацию случайных элементов и работу с DOM.

**Домашнее задание:** добавить на страницу 404 счётчик количества посещений (через `localStorage`) и кнопку «Показать ещё», которая меняет картинку без перезагрузки.
