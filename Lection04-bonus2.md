## 📚 Лекция 4: Продвинутая работа с Flexbox и Box-моделью

### Центрирование и выравнивание элементов

**Вертикальное центрирование с `align-items: center`**
- Свойство `align-items` работает в **flex-контейнере** и выравнивает элементы по **поперечной оси** (вертикально при `flex-direction: row`)
- Пример центрирования элемента "О проекте":
```css
.menu-bar {
    display: flex;
    align-items: center; /* Все дочерние элементы центрируются по вертикали */
    height: 60px; /* Изменение высоты контейнера демонстрирует эффект центрирования */
}
```

**Сравнение `justify-content` и `align-items`**
- `justify-content` → **горизонтальное** выравнивание (главная ось)
- `align-items` → **вертикальное** выравнивание (поперечная ось)

**Значения выравнивания:**
```css
.container {
    align-items: flex-start;    /* элементы у верха */
    align-items: center;        /* элементы по центру */
    align-items: flex-end;      /* элементы у низа */
    align-items: stretch;       /* элементы растянуты (по умолчанию) */
}
```

### 📦 Box-модель и отступы

**Свойство `padding`**
- Добавляет **внутренние отступы** внутри элемента
- Проблема: по умолчанию padding **добавляется** к ширине/высоте элемента

**Решения проблемы с padding:**
```css
/* ПРОБЛЕМА: элемент станет шириной 100px + 20px слева + 20px справа = 140px */
.element {
    width: 100px;
    padding: 0 20px;
}

/* РЕШЕНИЕ: используем border-box */
.element {
    box-sizing: border-box; /* padding и border включаются в ширину/высоту */
    width: 100px;
    padding: 0 20px; /* теперь общая ширина останется 100px */
}
```

**Важность `box-sizing: border-box`**
```css
/* Рекомендуется добавлять всем элементам */
* {
    box-sizing: border-box;
}

.menu-bar {
    width: 100%;
    padding: 0 20px;
    /* Ширина останется 100% даже с padding */
}
```

### 🌊 Резиновая вёрстка
- Элементы адаптируются к размеру окна браузера
- Использование **процентов** вместо фиксированных пикселей
```css
.container {
    width: 80%; /* Занимает 80% ширины родителя */
    max-width: 1200px; /* Но не больше 1200px */
}
```

### 🎨 Создание визуальных эффектов

**Добавление разделительной полоски**
```css
/* Вариант 1: border */
.menu-bar {
    border-bottom: 1px solid rgba(0, 0, 0, 0.1);
}

/* Вариант 2: box-shadow для более сложного эффекта */
.menu-bar {
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
```

**Параметры `box-shadow`:**
```css
.menu-bar {
    box-shadow: 
        0px    /* смещение по X */
        2px    /* смещение по Y */
        4px    /* радиус размытия (blur) */
        0px    /* радиус распространения */
        rgba(0, 0, 0, 0.1); /* цвет с прозрачностью */
}
```

**Использование генераторов теней:**
- Рекомендуемые инструменты: CSSmatic, Shadow Generator
- Позволяют визуально настраивать параметры и сразу видеть результат

### 🔗 Создание многостраничного сайта

**Структура проекта:**
```
project/
├── index.html
├── about.html
├── style.css
├── menu-bar.css
├── about.css
└── images/
    ├── logo.svg
    └── background.jpg
```

**Создание новой страницы:**
```html
<!-- about.html -->
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>О проекте - Вики Медиум</title>
    <link rel="stylesheet" href="style.css">
    <link rel="stylesheet" href="about.css">
</head>
<body>
    <nav class="menu-bar">
        <!-- Копируем то же меню из index.html -->
        <a href="index.html"><img src="images/logo.svg" alt="Логотип"></a>
        <a href="about.html">О проекте</a>
    </nav>
    
    <section class="content-section">
        <div class="text-column">
            <h1>Заголовок секции</h1>
            <p>Текст описания...</p>
        </div>
        <div class="image-column">
            <img src="images/section1.jpg" alt="Описание изображения">
        </div>
    </section>
</body>
</html>
```

### 🛠️ Практическое задание: Создание страницы "О проекте"

**1. Настройка меню с тенью и центрированием:**
```css
/* menu-bar.css */
.menu-bar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
    height: 60px;
    padding: 0 20px;
    box-sizing: border-box;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    background: white;
}
```

**2. Вёрстка секций по макету из Figma:**
```html
<section class="section">
    <div class="container">
        <div class="text-content">
            <h2>Заголовок</h2>
            <p>Текст описания проекта...</p>
        </div>
        <div class="image-content">
            <img src="images/about-image.jpg" alt="О проекте">
        </div>
    </div>
</section>
```

**3. Стилизация секций:**
```css
/* about.css */
.section {
    padding: 60px 0;
}

.container {
    display: flex;
    align-items: center;
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 20px;
}

.text-content {
    flex: 1;
    padding-right: 40px;
}

.image-content {
    flex: 1;
}

.image-content img {
    max-width: 100%;
    height: auto;
}
```

### ❌ Решение распространённых проблем

**Проблемы со ссылками:**
- Проверьте правильность путей: `href="about.html"` (не `эбаута.html`)
- Убедитесь, что файл существует в правильной директории
- Проверьте регистр символов в именах файлов

**Опечатки в коде:**
- Используйте только латинские символы в именах классов и файлов
- Проверяйте закрытие тегов и кавычек
- Используйте валидатор HTML/CSS

### 💡 Ключевые концепции этой лекции

| Концепция | Применение | Важность |
|-----------|------------|----------|
| **Flexbox выравнивание** | `align-items: center` для вертикального центрирования | Критически важно для современных макетов |
| **Box-sizing** | `border-box` для предсказуемых размеров | Обязательно для профессиональной вёрстки |
| **Box-shadow** | Создание теней и разделителей | Улучшает визуальную иерархию |
| **Резиновая вёрстка** | Проценты и max-width | Основа адаптивного дизайна |
| **Многостраничность** | Единое меню на всех страницах | Обеспечивает консистентность сайта |
