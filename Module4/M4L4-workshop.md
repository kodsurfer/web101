# Лекция-воркшоп: Перевёрстка многостраничного сайта на примере музея «Огни Москвы»

**Добро пожаловать!**  

Сегодня мы не просто поговорим о теории — мы возьмём реальный проект, который «застрял» в 2005 году, и превратим его в современный, удобный и красивый многостраничный сайт. Наш подопытный — сайт музея «Огни Москвы». Почему он? Потому что его вёрстка до сих пор держится на вложенных таблицах, стилях внутри HTML и полном отсутствии flexbox/grid. Знакомая ситуация по legacy-проектам? 😉

К концу воркшопа вы:
- организуете структуру многостраничного сайта;
- создадите переиспользуемые блоки (шапка, меню, карточки);
- научитесь генерировать контент через JSON и JavaScript;
- стилизуете всё так, чтобы было приятно глазу и удобно на разных экранах.

**Формат:** я объясняю — вы сразу пробуете на своих компьютерах. Готовы? Поехали!

---

## 1. Анализ текущей вёрстки. Почему всё плохо (и хорошо, что мы это исправим)

Откроем «Огни Москвы» в архивах (или представим). Что мы видим?
- Дизайн 2005 года — никаких отзывчивых сеток.
- Вёрстка на таблицах: `<table><tr><td>` — это ад для поддержки.
- Стили прямо в HTML: `bgcolor="#cccccc"`, `font face="Arial"`.
- Нет ни flex, ни grid, ни нормальной семантики.

> **Задание для студентов:** за 1 минуту напишите в чат, какие проблемы это создаёт для пользователей и разработчиков (мобильные устройства, скорость загрузки, доступность).

**Вывод:** мы перевёрстываем сайт с нуля, сохраняя контент, но полностью меняя подход.

---

## 2. Подготовка к перевёрстке. Структура проекта

Создаём на рабочем столе папку `ogni-moskvy` (или как вам удобно). Внутри неё стандартный скелет:

```
ogni-moskvy/
├── index.html
├── about.html
├── projects.html
├── excursions.html
├── contacts.html
├── archive/
│   └── (внутренние страницы архива)
├── styles/
│   ├── reset.css
│   ├── fonts.css
│   ├── layout.css
│   ├── adaptive.css
│   └── animation.css
├── scripts/
│   └── main.js
├── images/
├── fonts/
└── (опционально) media/
```

**Почему именно так?**
- `reset.css` – обнуляем браузерные отступы.
- `fonts.css` – подключаем шрифты (System, Roboto, Open Sans и т.д.).
- `layout.css` – глобальные сетки, контейнеры, шапка, подвал.
- `adaptive.css` – медиа-запросы для мобилок.
- `animation.css` – ховеры, переходы.
- `main.js` – общий JavaScript для всего сайта.

> **Важно:** все страницы будут использовать одни и те же CSS-файлы, чтобы меню и базовые стили не дублировались.

---

## 3. Создание базового HTML и навигации

### 3.1 Разметка всех страниц

Создаём в корне:
- `index.html` (главная)
- `about.html` (о музее)
- `projects.html` (проекты)
- `excursions.html` (экскурсии)
- `contacts.html` (контакты)

Внутри `archive/` создаём, например, `archive/exhibition-2023.html` – это страница второго уровня.

### 3.2 Пишем семантическое меню

В каждом файле между `<body>` и `<main>` размещаем:

```html
<nav class="main-nav">
    <div class="container nav-container">
        <a href="index.html" class="logo">Музей «Огни Москвы»</a>
        <ul class="nav-menu">
            <li><a href="about.html">О музее</a></li>
            <li><a href="projects.html">Проекты</a></li>
            <li><a href="excursions.html">Экскурсии</a></li>
            <li><a href="contacts.html">Контакты</a></li>
            <li><a href="archive/">Архив</a></li>
        </ul>
    </div>
</nav>
```

**Обратите внимание на ссылку на главную:** пишем `href="index.html"`, а не `"/"` или `"."`.  
Почему? На GitHub Pages слэш может вести в корень репозитория, а не на вашу главную страницу. `index.html` – надёжно.

Ссылка на архив: `href="archive/"` – это ведёт на `archive/index.html`, которую мы тоже создадим позже (содержит список архивных материалов).

### 3.3 Что нужно поменять на каждой странице?

- Заголовок `<title>` – разный для каждой страницы.
- `<h1>` – соответствующий разделу.
- Копируем структуру `<header>` + `<nav>` на все страницы, чтобы меню работало везде.

> **Практика:** откройте `index.html` в браузере, кликайте по ссылкам. Убедитесь, что страницы переключаются и заголовки вкладок меняются.

---

## 4. Базовая стилизация. Шрифты, отступы, меню

### 4.1 Подключаем файлы стилей

В `<head>` каждой страницы:

```html
<link rel="stylesheet" href="styles/reset.css">
<link rel="stylesheet" href="styles/fonts.css">
<link rel="stylesheet" href="styles/layout.css">
<link rel="stylesheet" href="styles/adaptive.css">
<link rel="stylesheet" href="styles/animation.css">
```

### 4.2 Настройка шрифтов (fonts.css)

```css
body {
    font-family: system-ui, -apple-system, 'Roboto', 'Open Sans', sans-serif;
    font-weight: 400;
    line-height: 1.5;
    color: #1e1e1e;
}

h1 { font-size: 48px; line-height: 58px; font-weight: 700; }
h2 { font-size: 36px; line-height: 46px; font-weight: 500; }
h3 { font-size: 24px; line-height: 32px; font-weight: 400; }
```

### 4.3 Стилизация меню (layout.css)

```css
.container {
    max-width: 1280px;
    margin: 0 auto;
    padding: 0 20px;
}

.main-nav {
    width: 100%;
    background: rgba(255,255,255,0.9);
    backdrop-filter: blur(8px);
    position: sticky;
    top: 0;
    z-index: 100;
}

.nav-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 16px 20px;
}

.nav-menu {
    display: flex;
    gap: 32px;
    list-style: none;
}

.nav-menu a {
    color: #333;
    text-decoration: none;
    border-bottom: 1px solid transparent;
    transition: color 0.3s, border-color 0.3s;
}

.nav-menu a:hover {
    color: #000;
    border-bottom-color: #e0a800;
}
```

> **Обратите внимание:** `gap` на флексах даёт одинаковые отступы между пунктами меню. Это лучше, чем `margin-right` у каждого `li`.

---

## 5. Динамические карточки на JavaScript (афиша музея)

Представьте, что музей проводит несколько мероприятий в разные дни. Мы не хотим вручную вбивать html-карточки, а хотим один раз описать данные и пусть JavaScript сам их нарисует. Это масштабируемо — можно подключить даже базу данных.

### 5.1 Где на странице будут карточки?

На главной добавим секцию `section id="poster"`. Внутри пустой контейнер `div class="cards-container"`.

### 5.2 Данные в формате JSON (массив объектов)

В файле `scripts/main.js` пишем:

```javascript
const posterData = [
    {
        date: "15 марта, пятница",
        events: [
            { time: "18:30", description: "Лекция «Свечи и фонари Москвы»" },
            { time: "20:00", description: "Ночная экскурсия с фонарщиком" }
        ]
    },
    {
        date: "16 марта, суббота",
        events: [
            { time: "12:00", description: "Детский мастер-класс «Огонёк в лампе»" },
            { time: "15:00", description: "Пешеходная экскурсия «Огни Китай-города»" }
        ]
    },
    {
        date: "17 марта, воскресенье",
        events: [
            { time: "14:00", description: "Семейный квест «Тайны старого фонаря»" }
        ]
    }
];
```

### 5.3 Функция отрисовки карточек

Всё та же `main.js`:

```javascript
function renderPoster() {
    const container = document.querySelector('.cards-container');
    if (!container) return;

    container.innerHTML = ''; // очищаем

    posterData.forEach(day => {
        const card = document.createElement('div');
        card.className = 'card';

        const dateHeading = document.createElement('h3');
        dateHeading.textContent = day.date;
        card.appendChild(dateHeading);

        const eventsList = document.createElement('div');
        eventsList.className = 'events-list';

        day.events.forEach(event => {
            const eventItem = document.createElement('div');
            eventItem.className = 'event-item';
            eventItem.innerHTML = `<span class="event-time">${event.time}</span> — <span class="event-desc">${event.description}</span>`;
            eventsList.appendChild(eventItem);
        });

        card.appendChild(eventsList);
        container.appendChild(card);
    });
}

// Ждём полной загрузки DOM
document.addEventListener('DOMContentLoaded', renderPoster);
```

### 5.4 Стилизация карточек (layout.css)

```css
.cards-container {
    display: flex;
    flex-wrap: wrap;
    gap: 30px;
    margin-top: 40px;
}

.card {
    flex: 1 1 300px;
    background: #faf9f8;
    border-radius: 16px;
    padding: 24px;
    transition: transform 0.3s, box-shadow 0.3s;
}

.card:hover {
    transform: translateY(-5px);
    box-shadow: 0 12px 24px rgba(0,0,0,0.1);
}

.event-item {
    display: flex;
    gap: 12px;
    margin-top: 12px;
    border-bottom: 1px dashed #e2e2e2;
    padding-bottom: 8px;
}

.event-time {
    font-weight: 700;
    color: #b45f06;
}
```

> **Почему это круто?** Теперь, когда музей добавит новую дату, мы просто допишем объект в `posterData`, и карточка появится сама. Никакого копипаста HTML.

---

## 6. Страница «О музее» + изображения и партнёры

### 6.1 Контент и адаптивное изображение

В `about.html` размещаем:

```html
<main class="container">
    <h1>О музее</h1>
    <div class="about-grid">
        <div class="about-text">
            <p>Музей «Огни Москвы» рассказывает об истории городского освещения...</p>
        </div>
        <div class="about-image">
            <img src="images/museum-panorama.jpg" alt="Панорама музея Огни Москвы">
        </div>
    </div>
</main>
```

**Важно:** изображение должно масштабироваться, чтобы не вылезать за границы.

```css
.about-image img {
    width: 100%;
    height: auto;
    border-radius: 20px;
}

.about-grid {
    display: flex;
    gap: 40px;
    align-items: center;
    flex-wrap: wrap;
}
```

### 6.2 Блок партнёров (карточки-ссылки)

Добавим после описания:

```html
<section class="partners">
    <h2>Партнёры музея</h2>
    <div class="partners-grid">
        <a href="https://example.com/rest" class="partner-card">Проект «Отдых с детьми»</a>
        <a href="https://potanin.foundation" class="partner-card">Фонд Потанина</a>
        <a href="https://ricard31.ru" class="partner-card">Рекламная кампания Рикард 31</a>
    </div>
</section>
```

Стили для `partners-grid`:

```css
.partners-grid {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
    margin: 40px 0;
}

.partner-card {
    flex: 1 1 180px;
    background: #f0f0f0;
    padding: 20px;
    text-align: center;
    border-radius: 40px;
    text-decoration: none;
    color: #2c2c2c;
    transition: all 0.4s ease;
}

.partner-card:hover {
    background: #e0a800;
    color: white;
    transform: scale(1.02);
}
```

---

## 7. Улучшаем меню и борёмся с вложенными страницами

### 7.1 Проблема: как ссылаться на главную из папки `archive/`?

Если мы находимся на странице `archive/exhibition-2023.html`, то ссылка `<a href="index.html">` не сработает, потому что файла `index.html` нет в папке `archive/`.

**Решение:**  
- Всегда используйте абсолютные относительно корня пути начинающиеся со слэша `href="/index.html"` – но на GitHub Pages это может вести в корень репозитория, а не в вашу папку проекта.  
- Более надёжный способ – выйти на уровень выше: `href="../index.html"`. Две точки + слеш = подняться на одну папку назад.

**Правило:** если вложенность меняется, используйте относительные пути в зависимости от глубины. Для удобства можно написать универсальную навигацию через JavaScript, но для учебного проекта тренируемся с `../`.

### 7.2 Сделаем меню фиксированным (sticky) и с полупрозрачным фоном

Уже сделали выше, но добавим `backdrop-filter: blur(8px)` для современного вида.

Если хотите полностью зафиксировать меню при скролле:

```css
.main-nav {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    background: rgba(30,30,30,0.85);
    backdrop-filter: blur(12px);
}

body {
    padding-top: 80px; /* чтобы контент не прятался под меню */
}
```

Однако на макетах с маленькими экранами нужно продумать отступ. Оставим `position: sticky`.

---

## 8. Финальные штрихи. Адаптивность и анимации

### 8.1 Медиа-запросы (adaptive.css)

```css
@media (max-width: 768px) {
    .nav-container {
        flex-direction: column;
        gap: 16px;
    }
    .nav-menu {
        flex-wrap: wrap;
        justify-content: center;
        gap: 16px;
    }
    h1 { font-size: 36px; line-height: 44px; }
    .cards-container {
        gap: 16px;
    }
}
```

### 8.2 Анимация появления карточек

```css
.card {
    opacity: 0;
    transform: translateY(20px);
    animation: fadeInUp 0.6s forwards;
}

@keyframes fadeInUp {
    to {
        opacity: 1;
        transform: translateY(0);
    }
}
```

Добавим небольшую задержку для каждой карточки через `:nth-child` или прямо в JS (при создании задаём `style.animationDelay`).

---

## Заключение. Что мы сделали и куда двигаться дальше

**Итоги воркшопа:**
1. Проанализировали устаревший табличный сайт.
2. Построили правильную файловую структуру для многостраничного проекта.
3. Создали общее меню и подключили его ко всем страницам.
4. Научились генерировать повторяющиеся блоки (карточки афиши) через JSON + JavaScript.
5. Оформили страницу с изображениями и партнёрами.
6. Починили ссылки для вложенных страниц и добавили анимации.

**Домашнее задание (чтобы закрепить):**
- Добавьте на страницу `contacts.html` форму обратной связи с полями: имя, email, сообщение. Используйте флексы для её расположения.
- Сделайте подвал (footer) с тремя колонками: навигация, контакты, соцсети.
- Поэкспериментируйте с тёмной темой для сайта (медиа-запрос `prefers-color-scheme`).

> **Совет:** называйте файлы осмысленно, не `page1.html`, а `excursions.html`. Это поможет и вам, и поисковым системам.

Если на каком-то этапе споткнулись – пересмотрите код, сверьтесь с конспектом. Я остаюсь на связи в чате. Удачи в перевёрстке реальных проектов! 🚀