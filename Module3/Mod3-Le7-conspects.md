# Лекция-воркшоп: Создание интерактивных анимаций с JavaScript

## Введение

Сегодня мы научимся оживлять наши макеты с помощью кода. Мы создадим три интерактивных элемента:

1. **Анимированный Popper** — всплывающие сердечки при клике на кнопку.
2. **Интерактивный блок** — квадрат, который меняет цвет или изображение в зависимости от положения курсора.
3. **Кастомный курсор** — анимированный элемент, следующий за мышью и реагирующий на клик.

Наш воркшоп построен так, чтобы вы не только повторили код, но и поняли логику его работы. В конце вы сможете использовать эти приёмы в своих проектах. Поехали!

---

## 1. Подготовка рабочего окружения

### 1.1 Создаём структуру проекта
- Откройте папку вашего проекта.
- Создайте новый HTML-файл с именем `app-remover.html`.
- В той же папке создайте две подпапки: `css` и `js`. В них будут лежать наши стили и скрипты.

### 1.2 Базовая разметка HTML
В файле `app-remover.html` наберите `!` и нажмите Tab — сгенерируется стандартная структура HTML.

Добавьте ссылки на CSS и JavaScript:
- В `<head>` вставьте `<link rel="stylesheet" href="css/style.css">`.
- Перед закрывающим тегом `</body>` вставьте `<script src="js/script.js"></script>`.

### 1.3 Проверка подключения
В CSS-файле измените фон тега `body`, например:
```css
body {
  background-color: #f0f0f0;
}
```

В JavaScript-файле напишите:
```js
console.log('Скрипт подключен!');
```

Откройте `app-remover.html` в браузере. Убедитесь, что фон изменился, а в консоли (F12) видно сообщение. Если всё ок — ставим плюс в чат!

---

## 2. Часть 1: Анимированный Popper (лайки при клике)

### 2.1 HTML-структура
Внутри `<body>` создадим отдельную секцию для нашего попапа:

```html
<section class="popup-section">
  <div class="popup"></div>
  <button class="popup-button">Нажми меня</button>
</section>
```

### 2.2 Базовые стили (CSS)
Начнём с обнуления отступов и установки шрифта:

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Arial, Helvetica, sans-serif;
}
```

Теперь стилизуем кнопку:

```css
.popup-button {
  width: 6vw;
  height: 4vw;
  font-size: 1.2vw;
  background-color: #ff6b6b;
  border: none;
  border-radius: 10px;
  color: white;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 20px auto;
}
```

### 2.3 Анимация для всплывающих элементов
Создадим класс `.popup-item` для сердечек, которые будут появляться при клике:

```css
.popup-item {
  position: absolute;
  width: 3vw;
  text-align: center;
  opacity: 0;
  animation: floatUp 1.5s ease-out forwards;
  color: red;
  font-size: 2vw;
}

@keyframes floatUp {
  0% {
    opacity: 0.5;
    transform: translateY(0) scale(1);
  }
  50% {
    opacity: 1;
    transform: translateY(-50px) scale(1.2);
  }
  100% {
    opacity: 0;
    transform: translateY(-100px) scale(0.8);
  }
}
```

### 2.4 JavaScript: создаём элементы при клике
Перейдём в файл `script.js`. Убедимся, что страница загружена, и найдём кнопку:

```js
document.addEventListener('DOMContentLoaded', () => {
  const button = document.querySelector('.popup-button');
  button.addEventListener('click', animatePopup);
});

function animatePopup(event) {
  // Создаём новый div
  const like = document.createElement('div');
  // Добавляем содержимое (сердечко)
  like.innerHTML = '❤️';
  // Добавляем класс для стилей
  like.classList.add('popup-item');
  
  // Позиционируем элемент относительно кнопки
  const buttonRect = event.target.getBoundingClientRect();
  like.style.left = buttonRect.left + buttonRect.width / 2 - 15 + 'px';
  like.style.top = buttonRect.top + 'px';
  
  // Добавляем на страницу
  document.body.appendChild(like);
  
  // Удаляем элемент после окончания анимации
  setTimeout(() => {
    like.remove();
  }, 1500);
}
```

**Пояснения:**
- `createElement` — создаёт элемент указанного типа.
- `innerHTML` — задаёт содержимое.
- `classList.add` — добавляет класс.
- `getBoundingClientRect()` — получает координаты кнопки, чтобы сердечко появлялось прямо над ней.
- `appendChild` — вставляет элемент в DOM.
- `setTimeout` — удаляет элемент после завершения анимации (длительность 1.5с).

### 2.5 Проверка
Сохраните файлы, обновите страницу, нажмите кнопку. Должны появляться анимированные сердечки. Если всё работает — ставьте плюс!

---

## 3. Часть 2: Интерактивный блок, реагирующий на курсор

### 3.1 HTML для блока
Добавим новую секцию ниже:

```html
<section class="interactive-section">
  <div class="box"></div>
</section>
```

### 3.2 CSS для блока
Сделаем так, чтобы блок всегда был по центру экрана:

```css
.interactive-section {
  height: 100vh;
  position: relative;
}

.box {
  position: absolute;
  width: 150px;
  height: 150px;
  background-color: #3498db;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  transition: background-color 0.3s ease;
}
```

### 3.3 JavaScript: отслеживание мыши
Напишем функцию `makeBeautyBox`, которая будет запускаться после загрузки DOM:

```js
function makeBeautyBox() {
  const box = document.querySelector('.box');
  const body = document.body;
  
  // Получаем размеры окна и центр
  const halfBodySizeX = window.innerWidth / 2;
  const halfBodySizeY = window.innerHeight / 2;
  
  body.addEventListener('mousemove', (event) => {
    const cursorX = event.clientX;
    const cursorY = event.clientY;
    
    // Условия для изменения цвета
    if (cursorX < halfBodySizeX) {
      box.style.backgroundColor = 'red'; // слева
    } else {
      box.style.backgroundColor = 'yellow'; // справа
    }
    
    // Добавим вертикаль
    if (cursorY < halfBodySizeY) {
      box.style.backgroundColor = 'green'; // сверху
    } else {
      box.style.backgroundColor = 'blue'; // снизу
    }
  });
}

document.addEventListener('DOMContentLoaded', makeBeautyBox);
```

**Проблема:** последнее условие перезаписывает предыдущее. Нам нужно комбинировать условия.

### 3.4 Комбинирование условий
Используем логические операторы, чтобы задать четыре зоны:

```js
if (cursorX < halfBodySizeX && cursorY < halfBodySizeY) {
  box.style.backgroundColor = 'red'; // левый верх
} else if (cursorX >= halfBodySizeX && cursorY < halfBodySizeY) {
  box.style.backgroundColor = 'yellow'; // правый верх
} else if (cursorX < halfBodySizeX && cursorY >= halfBodySizeY) {
  box.style.backgroundColor = 'green'; // левый низ
} else {
  box.style.backgroundColor = 'blue'; // правый низ
}
```

### 3.5 Упрощение кода
Вынесем проверки в отдельные переменные:

```js
const isLeft = cursorX < halfBodySizeX;
const isTop = cursorY < halfBodySizeY;

if (isLeft && isTop) box.style.backgroundColor = 'red';
else if (!isLeft && isTop) box.style.backgroundColor = 'yellow';
else if (isLeft && !isTop) box.style.backgroundColor = 'green';
else box.style.backgroundColor = 'blue';
```

### 3.6 Добавление изображений
Вместо сплошного цвета можно использовать картинки. Предположим, у вас есть архив с четырьмя изображениями. Распакуйте их в папку `images`. В CSS добавим фоновые картинки для каждого состояния, но проще сделать четыре слоя.

**Альтернативный подход:** Создадим четыре абсолютно позиционированных изображения внутри `.box` и будем менять их прозрачность.

HTML:
```html
<div class="box">
  <img src="images/img1.jpg" class="box-layer layer-1">
  <img src="images/img2.jpg" class="box-layer layer-2">
  <img src="images/img3.jpg" class="box-layer layer-3">
  <img src="images/img4.jpg" class="box-layer layer-4">
</div>
```

CSS:
```css
.box {
  position: relative;
  width: 300px;
  height: 300px;
}
.box-layer {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  opacity: 0;
  transition: opacity 0.3s ease;
}
```

JavaScript:
```js
const layers = document.querySelectorAll('.box-layer');
// внутри обработчика mousemove:
if (isLeft && isTop) {
  layers[0].style.opacity = 1;
  layers[1].style.opacity = 0;
  layers[2].style.opacity = 0;
  layers[3].style.opacity = 0;
} else if (!isLeft && isTop) {
  layers[0].style.opacity = 0;
  layers[1].style.opacity = 1;
  layers[2].style.opacity = 0;
  layers[3].style.opacity = 0;
} // и так далее...
```

Теперь блок плавно меняет изображения при движении мыши.

---

## 4. Часть 3: Анимированный кастомный курсор

### 4.1 HTML-элемент курсора
Добавим в конец `<body>`:

```html
<div id="cursor"></div>
```

### 4.2 Стили для курсора
```css
#cursor {
  position: absolute;
  width: 30px;
  height: 30px;
  border-radius: 50%;
  background-color: rgba(255, 255, 255, 0.7);
  mix-blend-mode: difference;
  pointer-events: none;
  transition: transform 0.2s ease;
  animation: glow 1.5s infinite alternate;
}

@keyframes glow {
  from {
    box-shadow: 0 0 5px cyan;
  }
  to {
    box-shadow: 0 0 20px blue;
  }
}

#cursor.active {
  transform: scale(2);
}
```

### 4.3 JavaScript: слежение за мышью
```js
function animateCursor() {
  const cursor = document.getElementById('cursor');
  
  document.addEventListener('mousemove', (e) => {
    cursor.style.left = e.clientX + 'px';
    cursor.style.top = e.clientY + 'px';
  });
  
  document.addEventListener('click', () => {
    cursor.classList.add('active');
    setTimeout(() => {
      cursor.classList.remove('active');
    }, 200);
  });
}

document.addEventListener('DOMContentLoaded', animateCursor);
```

**Объяснение:**
- `pointer-events: none` — чтобы курсор не мешал кликать по элементам.
- `mix-blend-mode: difference` — интересный визуальный эффект.
- При клике добавляем класс `active`, который увеличивает размер, и через 200 мс убираем его.

---

## 5. Заключение

Сегодня мы научились:
- Генерировать элементы на лету с помощью `createElement`.
- Анимировать их через CSS keyframes.
- Отслеживать положение курсора и реагировать на него.
- Создавать кастомный курсор с эффектами.

---

**Дополнительные материалы:**
- Документация по createElement: [MDN](https://developer.mozilla.org/ru/docs/Web/API/Document/createElement)
- Подробнее о событиях мыши: [MDN](https://developer.mozilla.org/ru/docs/Web/API/Element/mousemove_event)
