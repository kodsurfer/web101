# Лекция-воркшоп: Создаём интерактивный попап с анимацией на JavaScript

**Цель:** Познакомиться с основами интерактивности на веб-странице через JavaScript, научиться создавать динамические элементы, которые реагируют на движение мыши, и применить это для генерации визуальных эффектов.  
**Формат:** Теория + практика (шаг за шагом пишем код вместе).

---

## Введение

Дизайн — это не только статичная картинка. Сегодня пользователи ожидают от сайтов отклика, интерактивности, «живых» элементов. Один из популярных приёмов — попап (всплывающее окно), который меняет своё содержимое в зависимости от действий пользователя. Сегодня мы сделаем такой попап: квадрат в центре экрана, который будет менять цвет или изображение в зависимости от того, в какой четверти экрана находится курсор мыши.

Почему это полезно? Вы сможете использовать подобные приёмы для:
- интерактивных фонов;
- визуализации данных;
- игровых механик;
- привлечения внимания к элементам.

Весь код мы будем писать на JavaScript. К концу занятия у вас будет работающий интерактивный модуль, который можно встроить в свои проекты.

---

## Подготовка: HTML-структура

Нам понадобится простой HTML-файл. Создайте его и добавьте следующие элементы:

```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Интерактивный попап</title>
    <style>
        body {
            margin: 0;
            height: 100vh;
            font-family: sans-serif;
        }
        #box {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 300px;
            height: 300px;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
            overflow: hidden; /* для картинок */
        }
        /* стили для координат (можно оформить как подсказку) */
        #info {
            position: fixed;
            bottom: 10px;
            left: 10px;
            background: rgba(255,255,255,0.8);
            padding: 10px;
            border-radius: 5px;
            font-size: 14px;
        }
        /* для шага 6 */
        #box img {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: opacity 0.3s ease;
            opacity: 0;
        }
    </style>
</head>
<body>
    <div id="box"></div>
    <div id="info">
        <div id="left"></div>
        <div id="top"></div>
        <div id="body"></div>
    </div>

    <script>
        // здесь будет наш код
    </script>
</body>
</html>
```

Разберём структуру:
- `#box` — наш попап. Он позиционирован по центру.
- `#info` — панелька, где мы будем выводить координаты курсора и центр экрана (для отладки и красоты).
- Внутри `#info` три пустых `div` с id: `left`, `top`, `body`. В них будем писать текст через JavaScript.

---

## Шаг 1. Анимируем и генерируем попап (создание элементов)

В конспекте первая ссылка ведёт на MDN про `createElement`. Зачем это нужно? Иногда элементы удобнее создавать прямо в JavaScript, особенно если они должны появляться динамически. Но на старте мы уже создали `#box` в HTML. Поэтому первый шаг можно пропустить или просто объяснить, что `createElement` позволяет создавать любые теги на лету.

**Пример:**
```javascript
let newDiv = document.createElement('div');
newDiv.textContent = 'Я появился!';
document.body.appendChild(newDiv);
```

Этот метод нам пригодится позже, когда будем добавлять картинки внутрь `#box`.

---

## Шаг 2. Немного арифметики и подготовительной работы

Теперь научимся отслеживать движение мыши и получать её координаты. Напишем первую версию функции `makeBeautyBox()`.

```javascript
function makeBeautyBox() {
  let left = document.querySelector('#left')
  let top = document.querySelector('#top')
  let body = document.querySelector('#body')
  let box = document.querySelector('#box')

  document.addEventListener('mousemove', (event) => {
    let cursorX = event.pageX
    let cursorY = event.pageY

    left.innerHTML = `курсор X: ${cursorX}`
    top.innerHTML = `курсор Y: ${cursorY}`

    let bodySizeX = document.documentElement.clientWidth
    let bodySizeY = document.documentElement.clientHeight
    let halfBodySizeX = bodySizeX / 2
    let halfBodySizeY = bodySizeY / 2

    body.innerHTML = `центр экрана по X: ${halfBodySizeX}px, по Y: ${halfBodySizeY}px`
  })
}

makeBeautyBox();
```

**Объяснение кода:**
- `document.querySelector` находит элементы по id.
- `document.addEventListener('mousemove', ...)` — слушатель события движения мыши. При каждом движении выполняется функция.
- `event.pageX` и `pageY` дают координаты курсора относительно всей страницы (не окна).
- `document.documentElement.clientWidth / clientHeight` — ширина и высота видимой области (окна).
- Вычисляем половину — это центр экрана.
- Выводим всё в соответствующие `div`.

**Результат:** при движении мыши в панельке снизу появляются цифры. Полезно для отладки и понимания, где находится курсор.

---

## Шаг 3. Немного усложняем: меняем цвет по условиям

Теперь добавим логику: разделим экран на 4 квадранта относительно центра и в зависимости от положения курсора будем менять фон `#box`.

Дополним функцию:

```javascript
function makeBeautyBox() {
  // ... (предыдущие строки остаются)

  document.addEventListener('mousemove', (event) => {
    // ... получение координат и размеров

    if (cursorX < halfBodySizeX && cursorY < halfBodySizeY) {
      box.style.cssText = 'background-color: red;'
    } else if (cursorX > halfBodySizeX && cursorY < halfBodySizeY) {
      box.style.cssText = 'background-color: gold;'
    } else if (cursorX > halfBodySizeX && cursorY > halfBodySizeY) {
      box.style.cssText = 'background-color: green;'
    } else {
      box.style.cssText = 'background-color: navy;'
    }
  })
}
```

**Разбор условий:**
- `cursorX < halfBodySizeX` — курсор левее центра по X.
- `cursorY < halfBodySizeY` — курсор выше центра по Y.
- Комбинации дают 4 квадранта: левый верхний (красный), правый верхний (золотой), правый нижний (зелёный), и оставшийся левый нижний (синий).

**Важно:** условие `else` сработает, когда курсор либо точно по центру (маловероятно), либо в левом нижнем квадранте. Оно покрывает случай `cursorX < halfBodySizeX && cursorY > halfBodySizeY`.

**Задание студентам:** подвигать мышью и увидеть, как меняется цвет квадрата. Обсудить, что происходит при прохождении через центр.

---

## Шаг 4. Немного упрощаем: добавляем переменные

Код работает, но условия громоздкие. Введём переменные для булевых выражений и цветов — так легче читать и изменять.

```javascript
function makeBeautyBox() {
  // ... выбор элементов

  document.addEventListener('mousemove', (event) => {
    // ... координаты и размеры

    let eq1 = cursorX < halfBodySizeX   // слева от центра?
    let eq2 = cursorX > halfBodySizeX   // справа от центра?
    let eq3 = cursorY < halfBodySizeY   // выше центра?
    let eq4 = cursorY > halfBodySizeY   // ниже центра?

    let bg0 = 'background-color: red;'
    let bg1 = 'background-color: gold;'
    let bg2 = 'background-color: green;'
    let bg3 = 'background-color: navy;'

    if (eq1 && eq3) {
      box.style.cssText = bg0
    } else if (eq2 && eq3) {
      box.style.cssText = bg1
    } else if (eq2 && eq4) {
      box.style.cssText = bg2
    } else {
      box.style.cssText = bg3
    }
  })
}
```

Теперь видно, что eq1 и eq3 — это левый верхний квадрант. Если захотите поменять цвета или добавить новые условия, это делается легко.

**Рефакторинг** — важный навык даже для дизайнеров: чистый код легче поддерживать.

---

## Шаг 5. Добавим ✨красоты✨ (картинки)

Цвета — это скучно. Давайте вместо фона подставлять изображения. Для этого нам понадобятся четыре картинки (например, с названиями 0.jpg, 1.jpg, 2.jpg, 3.jpg в папке images/blick/). Заменим `background-color` на `background-image`.

```javascript
function makeBeautyBox() {
  // ... всё то же самое до условий

    let bg0 = 'background-image: url("images/blick/0.jpg")'
    let bg1 = 'background-image: url("images/blick/1.jpg")'
    let bg2 = 'background-image: url("images/blick/2.jpg")'
    let bg3 = 'background-image: url("images/blick/3.jpg")'

    if (eq1 && eq3) {
      box.style.cssText = bg0
    } else if (eq2 && eq3) {
      box.style.cssText = bg1
    } else if (eq2 && eq4) {
      box.style.cssText = bg2
    } else {
      box.style.cssText = bg3
    }
}
```

Убедитесь, что пути к картинкам правильные. Можно использовать любые изображения — абстракции, текстуры, фотографии.

**Результат:** квадрат показывает разные картинки в зависимости от положения мыши. Уже похоже на интерактивный арт-объект.

---

## Шаг 6. Настраиваем плавность анимации

Пока картинки переключаются мгновенно, резко. Добавим плавность через opacity и переходы. Идея: внутри `#box` разместить четыре изображения друг над другом (абсолютно спозиционированных) и менять их прозрачность. Тогда будет эффект перетекания.

Сначала добавим изображения в HTML (или создадим их через JavaScript, но проще вручную). Модифицируем `#box`:

```html
<div id="box">
  <img id="img0" src="images/blick/0.jpg" alt="">
  <img id="img1" src="images/blick/1.jpg" alt="">
  <img id="img2" src="images/blick/2.jpg" alt="">
  <img id="img3" src="images/blick/3.jpg" alt="">
</div>
```

В CSS мы уже добавили `transition: opacity 0.3s ease;` и `opacity: 0;` для всех `img`.

Теперь перепишем JavaScript: вместо изменения фона будем менять opacity конкретного изображения.

```javascript
function makeBeautyBox() {
  // ... получение left, top, body, box

  // находим изображения внутри box
  let img0 = box.querySelector('#img0')
  let img1 = box.querySelector('#img1')
  let img2 = box.querySelector('#img2')
  let img3 = box.querySelector('#img3')

  document.addEventListener('mousemove', (event) => {
    // ... координаты и размеры, eq1-eq4

    if (eq1 && eq3) {
      img0.style.cssText = 'opacity: 1;'
    } else {
      img0.style.cssText = 'opacity: 0;'
    }
    if (eq2 && eq3) {
      img1.style.cssText = 'opacity: 1;'
    } else {
      img1.style.cssText = 'opacity: 0;'
    }
    if (eq2 && eq4) {
      img2.style.cssText = 'opacity: 1;'
    } else {
      img2.style.cssText = 'opacity: 0;'
    }
    // левый нижний квадрант: eq1 && eq4
    if (eq1 && eq4) {
      img3.style.cssText = 'opacity: 1;'
    } else {
      img3.style.cssText = 'opacity: 0;'
    }
  })
}
```

Обратите внимание: теперь у нас четыре условия, и каждое включает полное ветвление (иначе изображения не будут скрываться). Также появилось новое условие `eq1 && eq4` для левого нижнего квадранта — раньше оно было в `else`, а теперь мы его явно прописали для img3.

**Результат:** картинки плавно сменяют друг друга при движении мыши. Красиво и современно.

---

## Заключение

Мы создали интерактивный попап, который:
- отслеживает движение мыши;
- вычисляет центр экрана;
- делит экран на квадранты;
- меняет изображения с плавным переходом.

Этот паттерн можно использовать для множества задач:
- фоновые эффекты на лендингах;
- визуализация данных (карты, графики);
- сторителлинг (изменение сцены в зависимости от внимания);
- игры.

**Домашнее задание / идеи для развития:**
1. Вместо квадрантов использовать расстояние от центра — чем дальше курсор, тем темнее изображение.
2. Добавить звук при смене картинки.
3. Сделать, чтобы изображения менялись не только по положению, но и по скорости движения мыши.
4. Использовать видео или анимированные GIF вместо статичных картинок.
5. Создать генеративную графику на Canvas, меняющуюся в реальном времени.

---
