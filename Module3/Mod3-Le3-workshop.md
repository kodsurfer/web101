# JavaScript для веб-дизайнеров: Оживляем интерфейсы

**Цель воркшопа:** Научиться оживлять статичные макеты с помощью базовых возможностей JavaScript.

---

## Часть 1: Теория (40 минут)

### Введение: Почему веб-дизайнеру нужен JavaScript? (5 минут)

- **Статика vs Динамика:** CSS делает сайт красивым, JS делает его живым
- **UX через микровзаимодействия:** плавные переходы, реакция на действия пользователя
- **Карьерное преимущество:** дизайнер + базовый JS = более востребованный специалист

### Раздел 1: Когда начинать работать с элементами? (10 минут)

**Ключевая проблема:** JS пытается найти элементы, которых еще нет в DOM.

**Решение:** Событие `DOMContentLoaded`

```javascript
// Неправильно - элементы могут не успеть загрузиться
document.querySelector('h1').style.color = 'red';

// Правильно - ждем полной загрузки структуры
document.addEventListener('DOMContentLoaded', function() {
  document.querySelector('h1').style.color = 'red';
});
```

**Аналогия:** Вы не можете начать расставлять мебель в квартире, пока не построены стены. `DOMContentLoaded` - сигнал, что "стены построены".

### Раздел 2: Находим элементы для работы (10 минут)

**Селекторы - наш язык общения с браузером:**

```javascript
// Как в CSS - используем те же селекторы!
document.querySelector('h1')          // Первый h1 на странице
document.querySelector('.menu')       // Элемент с классом .menu
document.querySelector('#header')     // Элемент с id="header"

// Найти ВСЕ элементы
document.querySelectorAll('.card')    // Все карточки на странице
```

**Важно:** `querySelectorAll` возвращает массивоподобную коллекцию. Чтобы работать с каждым элементом:

```javascript
const buttons = document.querySelectorAll('.btn');
buttons.forEach(button => {
  button.addEventListener('click', function() {
    // Действие для каждой кнопки
  });
});
```

### Раздел 3: Меняем содержимое (5 минут)

**innerHTML - быстрый способ обновить контент:**

```javascript
document.querySelector('#greeting').innerHTML = 'Привет, пользователь!';

// Можно добавлять HTML-разметку
document.querySelector('#content').innerHTML = '<h2>Новый заголовок</h2><p>Новый текст</p>';
```

**Безопасность:** Будьте осторожны с пользовательским вводом через `innerHTML` - возможны XSS-атаки.

### Раздел 4: Работа со стилями (10 минут)

**Правило:** CSS для оформления, JS для динамических изменений.

**Два подхода:**

1. **Через классы (рекомендуется):**
```javascript
// Добавить класс
element.classList.add('active');

// Удалить класс
element.classList.remove('hidden');

// Переключить класс (если есть - удалить, если нет - добавить)
element.classList.toggle('dark-mode');

// Проверить наличие класса
if (element.classList.contains('error')) {
  // Действие
}
```

2. **Через style (для динамических значений):**
```javascript
// Изменить цвет фона
element.style.backgroundColor = 'blue';

// Изменять несколько свойств
element.style.cssText = `
  color: red;
  font-size: 20px;
  transform: rotate(5deg);
`;
```

---

## Часть 2: Практический воркшоп (80 минут)

### Упражнение 1: Интерактивная карточка (20 минут)

**Задача:** Создать карточку товара, которая реагирует на клики.

**HTML:**
```html
<div class="product-card">
  <h3 class="product-title">Название товара</h3>
  <p class="product-description">Описание товара...</p>
  <button class="favorite-btn">♡ В избранное</button>
  <div class="product-details hidden">
    <p>Дополнительная информация</p>
  </div>
</div>
```

**CSS:**
```css
.product-card {
  border: 1px solid #ddd;
  padding: 20px;
  max-width: 300px;
  transition: all 0.3s;
}

.hidden {
  display: none;
}

.active {
  border-color: #ff4081;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.favorited {
  color: #ff4081;
}
```

**JavaScript:**
```javascript
document.addEventListener('DOMContentLoaded', function() {
  const card = document.querySelector('.product-card');
  const title = document.querySelector('.product-title');
  const favoriteBtn = document.querySelector('.favorite-btn');
  const details = document.querySelector('.product-details');
  
  // 1. При клике на карточку добавляем/убираем активный стиль
  card.addEventListener('click', function() {
    card.classList.toggle('active');
  });
  
  // 2. При клике на кнопку избранного
  favoriteBtn.addEventListener('click', function(e) {
    e.stopPropagation(); // Останавливаем всплытие, чтобы не сработал клик по карточке
    favoriteBtn.classList.toggle('favorited');
    favoriteBtn.textContent = favoriteBtn.classList.contains('favorited') 
      ? '♥ В избранном' 
      : '♡ В избранное';
  });
  
  // 3. При двойном клике на заголовок показываем/скрываем детали
  title.addEventListener('dblclick', function() {
    details.classList.toggle('hidden');
  });
});
```

### Упражнение 2: Динамический переключатель темы (25 минут)

**Задача:** Создать переключатель светлой/темной темы.

**HTML:**
```html
<button id="theme-toggle">🌙 Темная тема</button>
<header class="site-header">
  <h1>Мой сайт</h1>
</header>
<main class="content">
  <p>Основное содержимое...</p>
</main>
```

**CSS:**
```css
:root {
  --bg-color: white;
  --text-color: black;
  --primary-color: #6200ee;
}

.dark-theme {
  --bg-color: #121212;
  --text-color: white;
  --primary-color: #bb86fc;
}

body {
  background-color: var(--bg-color);
  color: var(--text-color);
  transition: background-color 0.3s, color 0.3s;
}

.site-header {
  background-color: var(--primary-color);
  padding: 20px;
}

#theme-toggle {
  position: fixed;
  top: 20px;
  right: 20px;
  padding: 10px 20px;
  background-color: var(--primary-color);
  color: white;
  border: none;
  border-radius: 20px;
  cursor: pointer;
}
```

**JavaScript:**
```javascript
document.addEventListener('DOMContentLoaded', function() {
  const themeToggle = document.querySelector('#theme-toggle');
  const body = document.body;
  
  // Проверяем сохраненную тему
  const savedTheme = localStorage.getItem('theme');
  if (savedTheme === 'dark') {
    body.classList.add('dark-theme');
    themeToggle.textContent = '☀️ Светлая тема';
  }
  
  themeToggle.addEventListener('click', function() {
    // Переключаем тему
    body.classList.toggle('dark-theme');
    
    // Меняем текст кнопки
    const isDark = body.classList.contains('dark-theme');
    themeToggle.textContent = isDark ? '☀️ Светлая тема' : '🌙 Темная тема';
    
    // Сохраняем выбор пользователя
    localStorage.setItem('theme', isDark ? 'dark' : 'light');
  });
});
```

### Упражнение 3: Интерактивная галерея (35 минут)

**Задача:** Создать галерею изображений с предпросмотром.

**HTML:**
```html
<div class="gallery">
  <div class="main-image">
    <img id="main-img" src="image1.jpg" alt="Основное изображение">
  </div>
  
  <div class="thumbnails">
    <img src="image1.jpg" alt="Миниатюра 1" class="thumbnail active" data-full="image1.jpg">
    <img src="image2.jpg" alt="Миниатюра 2" class="thumbnail" data-full="image2.jpg">
    <img src="image3.jpg" alt="Миниатюра 3" class="thumbnail" data-full="image3.jpg">
    <img src="image4.jpg" alt="Миниатюра 4" class="thumbnail" data-full="image4.jpg">
  </div>
  
  <div class="gallery-controls">
    <button id="prev-btn">← Назад</button>
    <button id="next-btn">Вперед →</button>
    <button id="zoom-toggle">Увеличить</button>
  </div>
</div>
```

**JavaScript:**
```javascript
document.addEventListener('DOMContentLoaded', function() {
  const mainImg = document.querySelector('#main-img');
  const thumbnails = document.querySelectorAll('.thumbnail');
  const prevBtn = document.querySelector('#prev-btn');
  const nextBtn = document.querySelector('#next-btn');
  const zoomToggle = document.querySelector('#zoom-toggle');
  
  let currentIndex = 0;
  
  // Функция обновления главного изображения
  function updateMainImage(index) {
    const thumbnail = thumbnails[index];
    mainImg.src = thumbnail.dataset.full;
    mainImg.alt = thumbnail.alt;
    
    // Обновляем активный класс
    thumbnails.forEach(thumb => thumb.classList.remove('active'));
    thumbnail.classList.add('active');
    
    currentIndex = index;
  }
  
  // Клик по миниатюре
  thumbnails.forEach((thumbnail, index) => {
    thumbnail.addEventListener('click', function() {
      updateMainImage(index);
    });
  });
  
  // Кнопки навигации
  prevBtn.addEventListener('click', function() {
    let newIndex = currentIndex - 1;
    if (newIndex < 0) newIndex = thumbnails.length - 1;
    updateMainImage(newIndex);
  });
  
  nextBtn.addEventListener('click', function() {
    let newIndex = currentIndex + 1;
    if (newIndex >= thumbnails.length) newIndex = 0;
    updateMainImage(newIndex);
  });
  
  // Увеличение изображения
  zoomToggle.addEventListener('click', function() {
    mainImg.classList.toggle('zoomed');
    zoomToggle.textContent = mainImg.classList.contains('zoomed') 
      ? 'Уменьшить' 
      : 'Увеличить';
  });
  
  // Автопереключение каждые 5 секунд
  setInterval(() => {
    let newIndex = currentIndex + 1;
    if (newIndex >= thumbnails.length) newIndex = 0;
    updateMainImage(newIndex);
  }, 5000);
});
```

---

## Бонус: Советы для веб-дизайнеров

1. **Прототипируйте интерактивность:** Создавайте интерактивные прототипы в Figma + добавляйте JS для демонстрации поведения.

2. **Микровзаимодействия:** Используйте JS для:
   - Анимации при наведении
   - Плавного скролла к разделам
   - Динамической загрузки контента
   - Валидации форм

3. **Инструменты:**
   - [Codepen](https://codepen.io) для быстрого прототипирования
   - [JSFiddle](https://jsfiddle.net) для демонстрации клиентам
   - [CSS-Tricks Almanac](https://css-tricks.com/almanac/) для поиска свойств CSS

4. **Дальнейшее развитие:**
   - Изучите библиотеки анимаций (GSAP, Anime.js)
   - Освойте основы Vue.js или React для компонентного подхода
   - Изучите работу с API для динамического контента

---

## Домашнее задание

Создайте интерактивный компонент для вашего портфолио:
1. Выберите компонент (аккордеон, слайдер, табы)
2. Сверстайте его статически
3. Добавьте интерактивность с помощью JavaScript
4. Добавьте минимум 3 разных взаимодействия
5. Опубликуйте на Codepen и пришлите ссылку

---

## Ресурсы для самостоятельного изучения

1. [Learn JavaScript](https://learnjavascript.online) - интерактивный курс
2. [JavaScript 30](https://javascript30.com) - 30 проектов за 30 дней
3. [MDN Web Docs](https://developer.mozilla.org/ru/docs/Web/JavaScript) - документация
4. [Doka.guide](https://doka.guide/js/) - современный справочник

---

**Итог:** JavaScript позволяет превратить статичный дизайн в живой, отзывчивый интерфейс. Начните с малого - добавьте интерактивность к вашим текущим проектам, и вы увидите, как улучшится пользовательский опыт!
