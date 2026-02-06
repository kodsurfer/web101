# Лекция-воркшоп: Дополнительные механики JavaScript

## Тема: Интерактивные элементы и анимации в веб-дизайне

**Цель:** Освоить практические техники создания интерактивных элементов и анимаций для современных веб-интерфейсов.

**Необходимые знания:** HTML, CSS, базовый JavaScript

---

## Часть 1: Введение

### Почему эти механики важны?
- Улучшение пользовательского опыта (UX)
- Визуальная обратная связь для пользователей
- Привлечение внимания к ключевым элементам
- Современные тренды в веб-дизайне

### Основные принципы:
- Производительность анимаций
- Доступность (accessibility)
- Соответствие брендингу
- Мера в использовании

---

## Часть 2: Практические модули

### Модуль 1: Эффекты переворота (Flip)

#### Теория:
- CSS-свойство `transform` с функцией `rotateY()`
- Сохранение 3D-пространства: `transform-style: preserve-3d`
- Перспектива: `perspective` свойство
- Обратная сторона элемента: `backface-visibility`

#### Пример кода:
```html
<div class="flip-card">
  <div class="flip-card-inner">
    <div class="flip-card-front">
      <img src="front.jpg" alt="Front">
    </div>
    <div class="flip-card-back">
      <h3>Заголовок</h3>
      <p>Описание</p>
    </div>
  </div>
</div>
```

```css
.flip-card {
  perspective: 1000px;
  width: 300px;
  height: 300px;
}

.flip-card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  transition: transform 0.8s;
  transform-style: preserve-3d;
}

.flip-card:hover .flip-card-inner {
  transform: rotateY(180deg);
}

.flip-card-front, .flip-card-back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
}

.flip-card-back {
  transform: rotateY(180deg);
}
```

**Практическое задание:** Создайте карточку товара с эффектом переворота, показывающую дополнительную информацию на обратной стороне.

---

### Модуль 2: Анимация встряхивания (Shake)

#### Теория:
- CSS-анимации с ключевыми кадрами (`@keyframes`)
- Использование `transform: translateX()` для создания эффекта дрожания
- Практическое применение: ошибки валидации форм, привлечение внимания

#### Пример кода:
```css
@keyframes shake {
  0%, 100% {transform: translateX(0);}
  10%, 30%, 50%, 70%, 90% {transform: translateX(-5px);}
  20%, 40%, 60%, 80% {transform: translateX(5px);}
}

.shake-element {
  animation: shake 0.8s ease-in-out;
}

/* JavaScript для активации */
const button = document.querySelector('.submit-btn');
const form = document.querySelector('form');

button.addEventListener('click', () => {
  if (!form.checkValidity()) {
    form.classList.add('shake-element');
    setTimeout(() => {
      form.classList.remove('shake-element');
    }, 800);
  }
});
```

**Практическое задание:** Добавьте эффект встряхивания для поля ввода при некорректном заполнении.

---

### Модуль 3: Переключатели (Toggle Switch)

#### Теория:
- Псевдоэлементы `::before` и `::after`
- Стилизация чекбоксов
- CSS-переходы для плавной анимации
- Доступные переключатели с метками

#### Пример кода:
```html
<label class="switch">
  <input type="checkbox">
  <span class="slider"></span>
  <span class="label-text">Темная тема</span>
</label>
```

```css
.switch {
  position: relative;
  display: inline-flex;
  align-items: center;
  gap: 10px;
}

.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: relative;
  display: inline-block;
  width: 60px;
  height: 34px;
  background-color: #ccc;
  transition: .4s;
  border-radius: 34px;
  cursor: pointer;
}

.slider:before {
  content: "";
  position: absolute;
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  transition: .4s;
  border-radius: 50%;
}

input:checked + .slider {
  background-color: #2196F3;
}

input:checked + .slider:before {
  transform: translateX(26px);
}
```

**Практическое задание:** Создайте переключатель темы (светлая/темная) с сохранением состояния через JavaScript.

---

### Модуль 4: Прогресс-бар (Progress Bar)

#### Теория:
- HTML-элемент `<progress>` и его стилизация
- Анимированные прогресс-бары с CSS
- Заполнение прогресс-бара с JavaScript
- Применение: загрузка, заполнение форм, этапы процесса

#### Пример кода:
```html
<div class="progress-container">
  <div class="progress-bar" id="myBar"></div>
</div>
<button onclick="move()">Начать загрузку</button>
```

```css
.progress-container {
  width: 100%;
  height: 20px;
  background-color: #f1f1f1;
  border-radius: 10px;
  overflow: hidden;
}

.progress-bar {
  height: 100%;
  width: 0%;
  background-color: #4CAF50;
  transition: width 0.3s ease;
}
```

```javascript
function move() {
  const bar = document.getElementById("myBar");
  let width = 0;
  const interval = setInterval(() => {
    if (width >= 100) {
      clearInterval(interval);
    } else {
      width++;
      bar.style.width = width + "%";
    }
  }, 20);
}
```

**Практическое задание:** Создайте прогресс-бар для многостраничной формы.

---

### Модуль 5: Всплывающие окна (Popup)

#### Теория:
- Модальные и немодальные окна
- Позиционирование с помощью `position: fixed`
- Анимация появления/исчезновения
- Доступность (фокус, клавиша Escape)

#### Пример кода:
```html
<button onclick="openPopup()">Открыть окно</button>

<div class="popup" id="myPopup">
  <div class="popup-content">
    <span class="close" onclick="closePopup()">&times;</span>
    <h2>Заголовок окна</h2>
    <p>Содержимое всплывающего окна...</p>
  </div>
</div>
```

```css
.popup {
  display: none;
  position: fixed;
  z-index: 1000;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0,0,0,0.5);
}

.popup-content {
  background-color: white;
  margin: 10% auto;
  padding: 20px;
  width: 80%;
  max-width: 500px;
  border-radius: 10px;
  animation: popupFade 0.3s;
}

@keyframes popupFade {
  from {opacity: 0; transform: translateY(-20px);}
  to {opacity: 1; transform: translateY(0);}
}

.close {
  float: right;
  font-size: 28px;
  cursor: pointer;
}
```

```javascript
function openPopup() {
  document.getElementById("myPopup").style.display = "block";
}

function closePopup() {
  document.getElementById("myPopup").style.display = "none";
}

// Закрытие при клике вне окна
window.onclick = function(event) {
  const popup = document.getElementById("myPopup");
  if (event.target === popup) {
    popup.style.display = "none";
  }
}
```

**Практическое задание:** Создайте всплывающее окно для формы подписки на рассылку.

---

### Модуль 6: Рисование при скролле (Scrolldrawing)

#### Теория:
- Событие прокрутки `scroll`
- Расчет позиции скролла
- Анимация SVG или других элементов в зависимости от прокрутки
- Применение: интерактивные истории, пошаговые инструкции

#### Пример кода:
```html
<svg id="mySVG" width="400" height="100">
  <path id="myPath" d="M10,50 L390,50" stroke="#ddd" stroke-width="2" fill="none"/>
  <circle id="myCircle" cx="10" cy="50" r="8" fill="#4CAF50"/>
</svg>

<div style="height: 2000px;">
  <!-- Контент для прокрутки -->
</div>
```

```javascript
window.addEventListener('scroll', function() {
  const scrollPercent = 
    (document.documentElement.scrollTop + document.body.scrollTop) / 
    (document.documentElement.scrollHeight - document.documentElement.clientHeight);
  
  const path = document.getElementById('myPath');
  const circle = document.getElementById('myCircle');
  const pathLength = path.getTotalLength();
  
  // Устанавливаем положение круга вдоль пути
  const point = path.getPointAtLength(scrollPercent * pathLength);
  circle.setAttribute('cx', point.x);
  circle.setAttribute('cy', point.y);
});
```

**Практическое задание:** Создайте индикатор прогресса чтения статьи в виде движущегося элемента.

---

### Модуль 7: Редактируемый контент (Contenteditable)

#### Теория:
- HTML-атрибут `contenteditable`
- Стилизация состояния редактирования
- Сохранение изменений
- Применение: inline-редакторы, заметки, комментарии

#### Пример кода:
```html
<div class="editable" contenteditable="true" 
     data-placeholder="Нажмите, чтобы редактировать...">
  Ваш редактируемый текст здесь
</div>

<button onclick="saveContent()">Сохранить</button>
```

```css
.editable {
  padding: 10px;
  border: 2px dashed #ddd;
  min-height: 100px;
  outline: none;
}

.editable:focus {
  border-color: #4CAF50;
}

.editable:empty:before {
  content: attr(data-placeholder);
  color: #999;
}
```

```javascript
function saveContent() {
  const content = document.querySelector('.editable').innerHTML;
  localStorage.setItem('editableContent', content);
  alert('Содержимое сохранено!');
}
```

**Практическое задание:** Создайте блок для быстрых заметок с автосохранением.

---

### Модуль 8: Слайд-шоу цитат (Quotes Slideshow)

#### Теория:
- Смена контента с анимацией
- Таймеры в JavaScript
- Плавные переходы
- Навигационные элементы

#### Пример кода:
```html
<div class="slideshow-container">
  <div class="quote-slide fade">
    <q>Первая цитата</q>
    <p class="author">- Автор 1</p>
  </div>
  
  <div class="quote-slide fade">
    <q>Вторая цитата</q>
    <p class="author">- Автор 2</p>
  </div>
  
  <a class="prev" onclick="plusSlides(-1)">&#10094;</a>
  <a class="next" onclick="plusSlides(1)">&#10095;</a>
</div>

<div class="dots">
  <span class="dot" onclick="currentSlide(1)"></span>
  <span class="dot" onclick="currentSlide(2)"></span>
</div>
```

```css
.quote-slide {
  display: none;
  padding: 20px;
  text-align: center;
}

.fade {
  animation-name: fade;
  animation-duration: 1.5s;
}

@keyframes fade {
  from {opacity: .4}
  to {opacity: 1}
}

.prev, .next {
  cursor: pointer;
  position: absolute;
  top: 50%;
  padding: 16px;
  color: black;
  font-weight: bold;
  font-size: 18px;
  transition: 0.6s ease;
  border-radius: 0 3px 3px 0;
}

.next {
  right: 0;
  border-radius: 3px 0 0 3px;
}

.dot {
  cursor: pointer;
  height: 15px;
  width: 15px;
  margin: 0 2px;
  background-color: #bbb;
  border-radius: 50%;
  display: inline-block;
  transition: background-color 0.6s ease;
}

.active, .dot:hover {
  background-color: #717171;
}
```

```javascript
let slideIndex = 1;
showSlides(slideIndex);

function plusSlides(n) {
  showSlides(slideIndex += n);
}

function currentSlide(n) {
  showSlides(slideIndex = n);
}

function showSlides(n) {
  let slides = document.querySelectorAll(".quote-slide");
  let dots = document.querySelectorAll(".dot");
  
  if (n > slides.length) {slideIndex = 1}
  if (n < 1) {slideIndex = slides.length}
  
  slides.forEach(slide => slide.style.display = "none");
  dots.forEach(dot => dot.className = dot.className.replace(" active", ""));
  
  slides[slideIndex-1].style.display = "block";
  dots[slideIndex-1].className += " active";
}

// Автоматическое переключение
setInterval(() => {
  plusSlides(1);
}, 5000);
```

**Практическое задание:** Создайте слайд-шоу отзывов клиентов.

---

### Модуль 9: Перетаскиваемые элементы (Draggable DIV)

#### Теория:
- События мыши: `mousedown`, `mousemove`, `mouseup`
- Расчет позиции элемента
- Ограничение области перемещения
- Сохранение позиции после перемещения

#### Пример кода:
```html
<div id="draggable" class="draggable-box">
  Перетащите меня
</div>
```

```css
.draggable-box {
  width: 200px;
  height: 200px;
  background-color: #4CAF50;
  color: white;
  text-align: center;
  line-height: 200px;
  cursor: move;
  position: absolute;
  user-select: none;
  border-radius: 10px;
}
```

```javascript
const draggable = document.getElementById('draggable');
let pos1 = 0, pos2 = 0, pos3 = 0, pos4 = 0;

draggable.onmousedown = dragMouseDown;

function dragMouseDown(e) {
  e.preventDefault();
  pos3 = e.clientX;
  pos4 = e.clientY;
  document.onmouseup = closeDragElement;
  document.onmousemove = elementDrag;
}

function elementDrag(e) {
  e.preventDefault();
  pos1 = pos3 - e.clientX;
  pos2 = pos4 - e.clientY;
  pos3 = e.clientX;
  pos4 = e.clientY;
  
  draggable.style.top = (draggable.offsetTop - pos2) + "px";
  draggable.style.left = (draggable.offsetLeft - pos1) + "px";
}

function closeDragElement() {
  document.onmouseup = null;
  document.onmousemove = null;
  
  // Сохраняем позицию
  localStorage.setItem('boxTop', draggable.style.top);
  localStorage.setItem('boxLeft', draggable.style.left);
}

// Восстанавливаем позицию при загрузке
window.onload = function() {
  const savedTop = localStorage.getItem('boxTop');
  const savedLeft = localStorage.getItem('boxLeft');
  
  if (savedTop) draggable.style.top = savedTop;
  if (savedLeft) draggable.style.left = savedLeft;
}
```

**Практическое задание:** Создайте систему виджетов, которые можно перетаскивать и располагать на рабочем столе.

---

### Модуль 10: Стилизация курсора

#### Теория:
- CSS-свойство `cursor`
- Пользовательские курсоры с `url()`
- Различные состояния курсора
- Производительность и размер файлов курсоров

#### Пример кода:
```css
/* Системные курсоры */
.button {
  cursor: pointer;
}

.waiting {
  cursor: wait;
}

.help {
  cursor: help;
}

.not-allowed {
  cursor: not-allowed;
}

.zoom-in {
  cursor: zoom-in;
}

/* Пользовательские курсоры */
.custom-cursor {
  cursor: url('cursor.png'), auto;
}

.custom-pointer {
  cursor: url('pointer.png'), pointer;
}

/* Пример из конспекта */
.box {
  width: 100px;
  height: 100px;
  background: black;
  display: inline-block;
  position: relative;
  border: 1px solid white;
  cursor: url("https://cdn.custom-cursor.com/db/7648/32/meme-pop-cat-cursor.png"), auto;
}

.box:active {
  cursor: url("https://cdn.custom-cursor.com/db/7647/32/meme-pop-cat-pointer.png"), auto;
}
```

**Практическое задание:** Создайте тематический курсор для своего проекта.

---

## Часть 3: Финальный проект

**Задача:** Создайте интерактивную карточку пользователя, которая включает:

1. **Flip-эффект** для переворота карточки
2. **Shake-анимацию** при неверном действии
3. **Toggle Switch** для включения/отключения уведомлений
4. **Progress Bar** для отображения заполненности профиля
5. **Contenteditable** поля для имени и описания
6. **Кастомный курсор** при наведении на карточку

**Критерии оценки:**
- Работоспособность всех элементов
- Качество и плавность анимаций
- Чистота кода
- Креативность реализации

---

## Ресурсы для дальнейшего изучения:

1. **Документация:**
   - [MDN Web Docs](https://developer.mozilla.org/)
   - [W3Schools How To](https://www.w3schools.com/howto/default.asp)
   - [CSS-Tricks](https://css-tricks.com/)

2. **Библиотеки для анимаций:**
   - [Animate.css](https://animate.style/)
   - [GSAP](https://greensock.com/gsap/)
   - [AOS (Animate On Scroll)](https://michalsnik.github.io/aos/)

3. **Инструменты:**
   - [Cubic Bezier Generator](https://cubic-bezier.com/)
   - [CSS Gradient Generator](https://cssgradient.io/)
   - [Keyframes Generator](https://keyframes.app/)

---

**Домашнее задание:** Выберите один из изученных элементов и создайте его улучшенную версию с дополнительной функциональностью.

---
