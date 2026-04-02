---

# Лекция: четвёртый модуль — формы, типографика, анимация и баннер

## 1. Организационные моменты: планы на модуль

Приветствую, коллеги! Четвёртый модуль будет немного необычным по ритму. У нас запланировано **четыре встречи в апреле**, затем перерыв на майские праздники, а после праздников мы продолжим работу над проектами. Не пугайтесь пауз — это время для самостоятельной доработки, а я буду проводить **консультации**. Финальная консультация перед сессией состоится **в июне**.

**Важно:** в модуле мы разберём базовые, но очень нужные темы — **формы, таблицы, типографику и выгрузку сайта в сеть**. Формы вам уже встречались в проектах третьего модуля, теперь разберём их профессионально. По запросу могу провести дополнительный воркшоп.

## 2. Задание на сегодня

Сегодня мы сверстаем **форму по техническому заданию** к четвёртому модулю. Вам пригодятся ссылки на пример формы и шрифт (всё есть в материалах).

## 3. Создаём проект для формы

1. Создайте новый файл `.html`, назовите его `form.html`.
2. Внутри тега `<title>` пропишите `form` — так вы легко найдёте страницу среди других.

## 4. Базовый тег `<form>` и его атрибуты

Форма начинается с тега `<form>`. Главные атрибуты:

- **`action`** — URL, куда отправятся данные формы (например, `/submit-form`).
- **`method`** — способ отправки:  
  - `get` (данные в адресной строке, не для паролей)  
  - `post` (данные в теле запроса, безопаснее)

Пока просто запомните их. В нашем проекте бэкэнда нет, но мы сделаем форму, которая при нажатии кнопки будет показывать собранные данные на странице — это полезно для прототипирования.

## 5. Тег `<input>` и его типы

Тег `<input>` — главный инструмент ввода. Типы (атрибут `type`):

| Тип | Что делает |
|-----|-------------|
| `text` | обычный текст |
| `number` | числа (со стрелками) |
| `tel` | телефон (клавиатура для цифр на мобильных) |
| `date` | календарик для выбора даты |
| `email` | почта (валидация, автозаполнение) |
| `password` | звёздочки вместо символов |
| `checkbox` | галочка (можно несколько) |
| `radio` | переключатель (только один в группе) |

### Примеры в коде:

```html
<input type="text" name="firstname">
<input type="email" name="email">
<input type="password" name="password">
```

### Строчные и блочные теги

`<input>` — строчный (inline), но его легко сделать блочным через CSS (`display: block`). Обычно мы оборачиваем каждый `input` в `<div>` или `<span>` для управления вёрсткой.

## 6. Атрибуты `name`, `value` и `placeholder`

- **`name`** — имя поля. Именно оно отправится на сервер: `name="username"`.
- **`value`** — значение поля. Если оставить пустым (`value=""`), пользователь сам его заполнит. Не рекомендуется прописывать значение по умолчанию, иначе можно случайно отправить пустые данные.
- **`placeholder`** — подсказка внутри поля. Исчезает при вводе. (В интерфейсах Apple иногда отказываются от placeholder, но мы его используем — потом сделаем анимацию получше.)

## 7. Элемент `<select>` — выпадающий список

Когда вариантов много, используем `<select>`. Внутри — `<option>`.

```html
<select name="country">
  <option value="ru">Россия</option>
  <option value="de">Германия</option>
  <option value="fr">Франция</option>
</select>
```

Атрибуты `<select>`: `name`, `id`, `autofocus`, `disabled`, а также `multiple` — позволяет выбрать несколько вариантов.

## 8. Практика: собираем форму регистрации

Давайте создадим форму, похожую на Apple ID. Она будет включать:

### 8.1. Блок «Имя и фамилия»

Два инпута внутри `div` с классом `names`:

```html
<div class="names">
  <input type="text" name="firstname" placeholder="Имя">
  <input type="text" name="lastname" placeholder="Фамилия">
</div>
```

### 8.2. Дата рождения

Три селекта — день, месяц, год. Располагаем их в одну линию с помощью flexbox (потом добавим стили).

```html
<div class="birthdate">
  <select name="day">
    <option value="1">1</option> <option value="2">2</option> ... <option value="31">31</option>
  </select>
  <select name="month">
    <option value="jan">Январь</option> ... <option value="dec">Декабрь</option>
  </select>
  <select name="year">
    <!-- генерируем годы, например от 1900 до 2025 -->
  </select>
</div>
```

*Подсказка: значения для 31 дня, 12 месяцев и годов можно сгенерировать вручную или скриптом. В учебных целях пропишите по 5-6 вариантов.*

### 8.3. Email и пароль

Три поля: email, пароль, подтверждение пароля. У всех `value=""`.

```html
<input type="email" name="email" placeholder="Электронная почта">
<input type="password" name="password" placeholder="Пароль">
<input type="password" name="confirm_password" placeholder="Подтверждение пароля">
```

### 8.4. Выбор страны и телефон

Селект с кодом страны + поле для номера.

```html
<select name="country_code">
  <option value="+7">Россия +7</option>
  <option value="+49">Германия +49</option>
  <option value="+33">Франция +33</option>
  <option value="+1">США +1</option>
</select>
<input type="tel" name="phone" placeholder="Номер телефона">
```

### 8.5. Радиокнопки — вопрос о списании денег

Добавим абзац с текстом и две радиокнопки с одинаковым `name` (чтобы работали как группа). Для подписей используем `<label>` — клик по тексту тоже активирует кнопку.

```html
<p>Разрешаете списывать деньги?</p>
<label><input type="radio" name="payment" value="yes"> Да</label>
<label><input type="radio" name="payment" value="no"> Нет</label>
```

### 8.6. Чекбоксы (например, согласие с условиями)

```html
<label><input type="checkbox" name="agree"> Я согласен с условиями</label>
```

### 8.7. Кнопка отправки

```html
<button type="submit">Продолжить</button>
```

## 9. Подключение стилей и шрифтов

Создайте файл `styles.css` и подключите его в `<head>`:

```html
<link rel="stylesheet" href="styles.css">
```

### Шрифт San Francisco Pro Display

Скачайте шрифт, удалите лишние начертания (оставьте только дисплейные). Подключите через `@font-face`:

```css
@font-face {
  font-family: 'SF Pro Display';
  src: url('fonts/SFProDisplay-Light.woff2') format('woff2');
  font-weight: 300;
  font-style: normal;
}
/* Повторить для 200, 400, 500, 600, 700, 800, 900 */
```

### Базовые сбросы и центрирование

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'SF Pro Display', -apple-system, BlinkMacSystemFont, 'Helvetica Neue', sans-serif;
  background: #f5f5f7;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

form {
  width: 460px;
  margin: 15px auto;
  background: white;
  padding: 30px;
  border-radius: 18px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}
```

### Стилизация полей

```css
input, select, button {
  font-family: inherit;
  font-size: 17px;
  font-weight: 400;
  width: 100%;
  padding: 14px 16px;
  margin-bottom: 16px;
  border: 1px solid #ccc;
  border-radius: 12px;
  background: #fff;
  transition: all 0.2s;
}

input:focus, select:focus {
  border-color: #0071e3;
  outline: none;
  box-shadow: 0 0 0 4px rgba(0,113,227,0.2);
}

button {
  background: #0071e3;
  color: white;
  border: none;
  font-weight: 600;
  cursor: pointer;
}

button:hover {
  background: #005bbf;
}
```

### Flex для горизонтальных блоков

```css
.names, .birthdate, .phone-group {
  display: flex;
  gap: 12px;
}
.names input, .birthdate select, .phone-group select {
  width: calc(50% - 6px);
}
```

### Радиокнопки и чекбоксы — делаем их красивее

```css
/* Вертикальная группа радиокнопок */
.radio-group {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-bottom: 16px;
}
.radio-group label, .checkbox-group label {
  display: flex;
  align-items: center;
  gap: 12px;
  font-size: 16px;
}
input[type="radio"], input[type="checkbox"] {
  width: 20px;
  height: 20px;
  margin: 0;
}
```

## 10. Анимация плейсхолдера (продвинутый приём)

Вместо стандартного `placeholder` сделаем анимированную подсказку, которая всплывает при фокусе. Это требует дополнительных обёрток.

### 10.1. Структура

Каждое поле оборачиваем в `<span class="placeholder-field">`:

```html
<span class="placeholder-field">
  <input type="text" name="firstname" required>
</span>
```

### 10.2. Псевдоэлемент `::before` для текста подсказки

```css
.placeholder-field {
  position: relative;
  display: block;
  margin-bottom: 16px;
}

.placeholder-field input {
  width: 100%;
  padding: 20px 12px 8px 12px;
  border: 1px solid #ccc;
  border-radius: 12px;
  font-size: 17px;
}

.placeholder-field::before {
  content: attr(data-placeholder);
  position: absolute;
  left: 12px;
  top: 50%;
  transform: translateY(-50%);
  color: #888;
  font-size: 17px;
  transition: 0.2s ease;
  pointer-events: none;
}
```

### 10.3. Анимация при фокусе или при заполненном поле

Используем псевдокласс `:focus` и `:not(:placeholder-shown)`, но так как мы не используем настоящий placeholder, проще добавить класс через JS. Однако можно обойтись и CSS с атрибутами.

Более простой вариант для демонстрации: используйте стандартный `placeholder` и анимируйте его через CSS:

```css
input:focus::placeholder {
  opacity: 0;
  transform: translateY(-10px);
  transition: 0.2s;
}
```

Это работает без обёрток.

## 11. Создаём баннер (как на странице Apple ID)

Добавим над формой красивый баннер с анимированными кружочками.

### 11.1. HTML

```html
<div class="banner">
  <div class="banner-text">
    <h1>Ваш Apple ID</h1>
    <a href="#">Войти в систему</a>
  </div>
  <div class="circles">
    <div class="circle circle-1"></div>
    <div class="circle circle-2"></div>
    <div class="circle circle-3"></div>
  </div>
</div>
```

### 11.2. CSS для баннера

```css
.banner {
  position: relative;
  width: 100%;
  height: 300px;
  background: #1d1d1f;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  overflow: hidden;
  border-radius: 24px;
  margin-bottom: 30px;
}

.banner-text h1 {
  color: white;
  font-weight: 800;
  font-size: 34px;
  letter-spacing: -0.5px;
  margin-bottom: 8px;
}
.banner-text a {
  color: #2997ff;
  text-decoration: none;
  font-size: 17px;
}

/* Кружочки */
.circles {
  position: absolute;
  width: 400px;
  height: 400px;
  filter: blur(150px);
  opacity: 0.6;
}
.circle {
  position: absolute;
  width: 200px;
  height: 200px;
  border-radius: 100px;
}
.circle-1 { background: #0071e3; top: -50px; left: -50px; }
.circle-2 { background: #ff9f0a; bottom: -50px; right: -50px; }
.circle-3 { background: #5e5ce0; top: 50%; left: 50%; transform: translate(-50%, -50%); }

/* Анимация вращения */
@keyframes rotateCircles {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
.circles {
  animation: rotateCircles 20s linear infinite;
}
```

## 12. Меню навигации (введение в многостраничность)

Добавим на страницу навигационное меню. В будущем это поможет создать многостраничный сайт.

```html
<nav class="main-nav">
  <a href="#">Store</a>
  <a href="#">Mac</a>
  <a href="#">iPad</a>
  <a href="#">iPhone</a>
  <a href="#">Watch</a>
  <a href="#">AirPods</a>
  <a href="#">TV & Home</a>
  <a href="#">Only on Apple</a>
  <a href="#">Accessories</a>
  <a href="#">Support</a>
</nav>
```

Стили:

```css
.main-nav {
  background: rgba(0,0,0,0.8);
  backdrop-filter: blur(10px);
  display: flex;
  justify-content: center;
  gap: 30px;
  padding: 12px 20px;
  border-radius: 40px;
  margin-bottom: 40px;
}
.main-nav a {
  color: #f5f5f7;
  text-decoration: none;
  font-size: 14px;
  transition: color 0.2s;
}
.main-nav a:hover {
  color: #fff;
}
```

**Как работают ссылки?**  
Атрибут `href` указывает путь:  
- `href="page2.html"` — относительная ссылка внутри проекта.  
- `href="https://apple.com"` — абсолютная, ведёт на другой сайт.  
В многостраничном проекте все страницы подключают один и тот же CSS-файл.

## 13. Формы и данные (без бэкэнда)

Поскольку бэкэнда у нас нет, мы можем научиться обрабатывать данные формы прямо в браузере. Добавим простой скрипт:

```javascript
document.querySelector('form').addEventListener('submit', function(e) {
  e.preventDefault(); // отменяем отправку на сервер
  const formData = new FormData(this);
  const data = Object.fromEntries(formData.entries());
  console.log('Данные формы:', data);
  alert('Форма собрана! Данные в консоли');
});
```

Это полезно для прототипирования — вы видите, какие поля и с какими значениями отправляются.

## 14. Подведение итогов и домашнее задание

Сегодня мы:
- Создали структуру формы с разными типами полей.
- Применили селекты, радиокнопки, чекбоксы.
- Подключили кастомные шрифты и стилизовали форму.
- Начали разбираться с анимацией плейсхолдера.
- Сверстали баннер с вращающимися кружочками.
- Сделали навигационное меню.

**Что нужно доделать:**  
1. Закончить вёрстку формы по ТЗ (добавить все поля, о которых говорили).
2. Подключить шрифты и добиться внешнего вида, близкого к макету.
3. По желанию — реализовать анимацию плейсхолдера (можно через стандартный CSS-трюк с `:focus`).
4. Принести готовую форму на следующее занятие.

**Требования к четвёртому модулю:**  
- Форма должна быть адаптивной (на мобильных экранах поля растягиваются).
- Все инпуты имеют правильные атрибуты `name`.
- При клике на кнопку данные выводятся в консоль.

Желаю вам хорошего вечера и продуктивных выходных! Вопросы пишите в чат, на следующей встрече разберём ошибки и продолжим работу с иконками для меню.

---
