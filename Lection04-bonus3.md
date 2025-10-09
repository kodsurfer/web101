Отлично! Продолжаем наш курс "Основы Web" с углублением в CSS, включая псевдоэлементы, позиционирование и тонкости вёрстки.

## 📚 Лекция 6: Продвинутый CSS - псевдоэлементы, позиционирование и типографика

### 🎭 Работа с псевдоэлементами

**Что такое псевдоэлементы?**
Псевдоэлементы позволяют стилизовать определенные части элемента без добавления лишнего HTML-кода.

**Базовый синтаксис:**
```css
.element::before {
    content: ""; /* ОБЯЗАТЕЛЬНОЕ свойство */
    display: block;
    width: 100px;
    height: 100px;
    position: absolute;
    top: 0;
    left: 0;
}
```

**Ключевые моменты:**
- `content: ""` - обязательно, даже для пустых псевдоэлементов
- `::before` - создает псевдоэлемент ПЕРЕД содержимым
- `::after` - создает псевдоэлемент ПОСЛЕ содержимого

### 🖼️ Работа с фоновыми изображениями

**Настройка фона через CSS:**
```css
.background-section {
    background-image: url('../images/background.jpg');
    background-position: center top;
    background-repeat: no-repeat;
    background-size: cover;
    /* Или сокращенная запись */
    background: url('../images/background.jpg') center top / cover no-repeat;
}
```

**Позиционирование фона:**
```css
.background-element {
    background-position: 20px 50px; /* left top */
    background-position: center center;
    background-position: 80% 30%;
}
```

### ⚡ Проблемы и решения в вёрстке

**Проблема перекрытия ссылок:**
```css
/* ПРОБЛЕМА: псевдоэлемент перекрывает кликабельную область */
.menu-item::before {
    position: absolute;
    width: 100%;
    height: 100%;
    content: "";
    /* Этот элемент может блокировать клики */
}

/* РЕШЕНИЕ: использовать pointer-events */
.menu-item::before {
    pointer-events: none; /* Позволяет кликам "проходить сквозь" псевдоэлемент */
}
```

**Или альтернативное решение:**
```css
/* Удаляем псевдоэлемент и используем фон */
.menu-item {
    background: rgba(255, 255, 255, 0.8); /* Полупрозрачный фон */
}
```

### 🎯 Тонкости Flexbox и выравнивания

**Создание адаптивных рядов:**
```css
.flex-container {
    display: flex;
    flex-wrap: wrap; /* Позволяет перенос на новую строку */
    gap: 20px; /* Отступы между элементами */
}

.flex-item {
    flex: 1 1 300px; /* grow, shrink, basis */
    min-width: 0; /* Решает проблемы с переполнением */
}
```

**Работа с переполнением:**
```css
.image-container {
    overflow: hidden; /* Обрезает всё, что выходит за границы */
    border-radius: 8px; /* Закругленные углы */
}

.image-container img {
    width: 100%;
    height: auto;
    display: block;
}
```

### 🔢 Селекторы по номеру элемента

**Использование `nth-of-type`:**
```css
/* Стили для первого div в контейнере */
.container div:nth-of-type(1) {
    background-color: #f0f0f0;
    width: 720px;
}

/* Стили для второго div */
.container div:nth-of-type(2) {
    background-color: #e0e0e0;
    width: 680px;
}

/* Четные элементы */
.container div:nth-of-type(even) {
    border-left: 1px solid #ccc;
}

/* Нечетные элементы */
.container div:nth-of-type(odd) {
    border-right: 1px solid #ccc;
}
```

### 📐 Точная настройка размеров

**Расчет ширины для макета 1440px:**
```css
.layout-container {
    max-width: 1440px;
    margin: 0 auto;
}

.column-left {
    width: 720px;  /* 50% от 1440px */
}

.column-right {
    width: 680px;  /* Оставшееся пространство */
    margin-left: 40px; /* Итого: 720 + 680 + 40 = 1440px */
}
```

### ✍️ Работа с типографикой

**Настройка текстовых стилей:**
```css
.text-content h1 {
    font-size: 30px;
    line-height: 38px; /* Оптимально: font-size * 1.2-1.5 */
    font-weight: 700;
    margin-bottom: 24px;
}

.text-content p {
    font-size: 18px;
    line-height: 24px; /* Для читаемости */
    margin-bottom: 16px;
}

/* Специальный класс для мелкого текста */
.text-small {
    font-size: 14px;
    line-height: 20px;
}
```

### 📏 Система отступов и выравнивания

**Вертикальные отступы:**
```css
.content-section {
    padding: 120px 80px 0 80px; /* top right bottom left */
    /* Или подробно */
    padding-top: 120px;
    padding-right: 80px;
    padding-bottom: 0;
    padding-left: 80px;
}
```

**Работа с вертикальными ритмами:**
```css
.text-block {
    margin-top: 24px; /* Создание воздушного пространства */
}

/* Решение проблемы висячих предлогов */
.no-break {
    white-space: nowrap;
}

/* Использование в HTML */
<p>Это предложение с <span class="no-break">висящим предлогом</span> которое может плохо переноситься.</p>
```

### 🖼️ Выравнивание изображений

**Подвижка изображения с сохранением потоковой модели:**
```css
.image-wrapper {
    padding-top: 80px;
    position: relative;
}

.image-wrapper img {
    display: block;
    max-width: 100%;
    height: auto;
}
```

### 🔧 Инструменты для отладки

**Использование браузерных инструментов:**
- **F12** - открыть инструменты разработчика
- **Ctrl+Shift+C** - инспектор элементов
- **Изменение стилей в реальном времени**
- **Просмотр box-model**

**Временные стили для отладки:**
```css
.debug * {
    outline: 1px solid red !important;
}

.debug-flex {
    outline: 2px dashed blue !important;
}
```

### 🛠️ Практическое задание: Создание сложного макета

**HTML структура:**
```html
<section class="feature-section">
    <div class="container">
        <div class="text-column">
            <h1>Наш проект</h1>
            <p>Описание проекта с правильными переносами и отступами.</p>
            <p>Второй абзац с <span class="no-break">правильным переносом</span> предлогов.</p>
        </div>
        <div class="image-column">
            <div class="image-wrapper">
                <img src="images/project.jpg" alt="Наш проект">
            </div>
        </div>
    </div>
</section>
```

**CSS стилизация:**
```css
.feature-section {
    background: rgba(255, 255, 255, 0.9); /* Полупрозрачный фон */
    position: relative;
}

.feature-section::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: url('../images/pattern.png');
    opacity: 0.1;
    pointer-events: none; /* Не мешает кликам */
}

.container {
    display: flex;
    max-width: 1440px;
    margin: 0 auto;
    position: relative; /* Для абсолютного позиционирования псевдоэлементов */
}

.text-column {
    width: 720px;
    padding: 120px 80px 0 80px;
}

.text-column h1 {
    font-size: 30px;
    line-height: 38px;
    margin-bottom: 24px;
}

.text-column p {
    font-size: 18px;
    line-height: 24px;
    margin-bottom: 16px;
}

.image-column {
    width: 680px;
    padding-top: 80px;
}

.image-wrapper {
    overflow: hidden;
    border-radius: 8px;
}

.no-break {
    white-space: nowrap;
}
```

### 💾 Завершение работы и GitHub

**Финальные шаги:**
```bash
# Добавление изменений
git add .

# Создание коммита
git commit -m "Завершение вёрстки страницы О проекте: добавлены псевдоэлементы, настроена типографика и отступы"

# Отправка на GitHub
git push origin main
```

**Чек-лист завершения:**
- [ ] Все стили применены корректно
- [ ] Изображения оптимизированы и правильно подключены
- [ ] Текст хорошо читается, правильные переносы
- [ ] Отступы соответствуют макету
- [ ] Ссылки работают корректно
- [ ] Код закоммичен и запушен в репозиторий

### 💡 Ключевые концепции этой лекции

| Концепция | Применение | Важность |
|-----------|------------|----------|
| **Псевдоэлементы** | Декоративные элементы без HTML | ★★★★★ |
| **Абсолютное позиционирование** | Точное размещение элементов | ★★★★☆ |
| **Nth-of-type** | Стилизация по порядковому номеру | ★★★★☆ |
| **Line-height** | Вертикальный ритм текста | ★★★★★ |
| **Padding/Margin** | Пространство между элементами | ★★★★★ |
| **Overflow** | Контроль переполнения | ★★★★☆ |

Эта лекция охватывает продвинутые техники CSS, которые превращают обычную вёрстку в профессиональную и качественную работу. Практикуйтесь и экспериментируйте! 🚀