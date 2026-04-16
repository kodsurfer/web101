### Лекция по веб‑разработке для студентов‑дизайнеров: вёрстка таблиц


#### Введение

Сегодня мы разберём вёрстку таблиц в веб‑разработке. Несмотря на то, что таблицы — относительно старый инструмент (появились ещё в HTML 4 и CSS 2), они до сих пор актуальны:

* для отображения данных в табличном виде на сайте;
* для создания сетки в email‑рассылках (где флексбоксы и гриды не работают).


**Проблемы таблиц:**
* не поддерживают современные инструменты вёрстки (гриды, флексбоксы);
* требуют особого подхода для адаптивности;
* в email‑рассылках ведут себя непредсказуемо из‑за различий между почтовыми клиентами.

#### Шаг 1. Создание базовой структуры таблицы

Создаём три файла: `table.html`, `table.css`, `table.js`.

В `table.html` создаём базовую разметку:
```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Таблица</title>
    <link rel="stylesheet" href="table.css">
</head>
<body>
    <table>
        <!-- содержимое таблицы -->
    </table>
    <script src="table.js"></script>
</body>
</html>
```

**Основные теги для таблиц:**
* `<table>` — контейнер таблицы;
* `<thead>` — заголовок таблицы (содержит заголовки столбцов);
* `<tbody>` — тело таблицы (основное содержимое);
* `<tr>` — строка таблицы (`table row`);
* `<th>` — ячейка‑заголовок (`table header`);
* `<td>` — обычная ячейка (`table data`).

Пример структуры таблицы:
```html
<table>
    <thead>
        <tr>
            <th>Неделя</th>
            <th>Название</th>
            <th>Предмет</th>
            <th>Дата</th>
            <th>Тип</th>
            <th></th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>4.1</td>
            <td>Верстаем форму</td>
            <td>Технологии</td>
            <td>02.04.2025</td>
            <td>Workshop</td>
            <td></td>
        </tr>
        <!-- другие строки -->
    </tbody>
</table>
```

#### Шаг 2. Базовые стили для таблицы (`table.css`)

Сброс базовых стилей и настройка шрифтов:
```css
* {
    box-sizing: border-box;
}

body {
    font-family: system-ui, -apple-system, sans-serif;
    margin: 0;
    padding: 20px;
}
```

Стили для таблицы:
```css
table {
    width: 100%;
    font-size: 14px;
    border-collapse: collapse; /* убирает отступы между ячейками */
}

th, td {
    font-weight: 400;
    text-align: left;
    padding: 8px 12px;
    height: 34px;
    border: 1px solid #ddd;
}

/* Убираем границы у крайних ячеек */
tr th:first-of-type,
tr td:first-of-type {
    border-left: none;
}
tr th:last-of-type,
tr td:last-of-type {
    border-right: none;
}
```

Настройка ширины столбцов:
```css
th:nth-of-type(1),
td:nth-of-type(1) { width: 10%; }
th:nth-of-type(2),
td:nth-of-type(2) { width: 35%; }
th:nth-of-type(3),
td:nth-of-type(3) { width: 15%; }
th:nth-of-type(4),
td:nth-of-type(4) { width: 10%; }
th:nth-of-type(5),
td:nth-of-type(5) { width: 30%; }
th:nth-of-type(6),
td:nth-of-type(6) { width: 26px; }
```

#### Шаг 3. Стилизация ячеек с цветами

Добавляем классы для ячеек с разными цветами. В HTML оборачиваем содержимое ячеек в `<span>` с классом:
```html
<td><span class="span-red">Review</span></td>
<td><span class="span-blue">Арт-практика</span></td>
```

В CSS задаём стили:
```css
td span {
    padding: 4px 6px;
    border-radius: 2px;
}

.span-red {
    background-color: rgb(250, 214, 211);
    color: #d0312d;
}
.span-blue {
    background-color: rgb(200, 220, 255);
    color: #2c5aa0;
}
/* и т. д. для других цветов */
```

#### Шаг 4. Добавление иконок в заголовки

Для заголовков оборачиваем текст в `<span>` с классом и добавляем иконки через псевдоэлемент `::before`:
```html
<th><span class="week">Неделя</span></th>
```
```css
th span {
    position: relative;
    padding-left: 20px;
}

th .week::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 16px;
    height: 16px;
    background-image: url("../images/Form Icons/week.png");
}
/* аналогично для других классов */
```

#### Шаг 5. Создание формы для добавления строк

Добавляем форму под таблицей в `table.html`:
```html
<form id="new-row">
    <input type="text" name="week" value="">
    <input type="text" name="title" value="">
    <select name="subject">
        <option value="Технологии">Технологии</option>
        <option value="Арт-практика">Арт-практика</option>
    </select>
    <input type="text" name="date" value="">
    <select name="type">
        <option value="Лекция">Лекция</option>
        <option value="Workshop">Workshop</option>
        <option value="Review">Review</option>
        <option value="Домашка">Домашка</option>
    </select>
    <button type="button">+</button>
</form>
```

Стилизуем форму:
```css
#new-row {
    display: flex;
    gap: 10px;
    align-items: center;
    margin-top: 20px;
}

#new-row input,
#new-row select,
#new-row button {
    flex: 1;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
}

#new-row button {
    cursor: pointer;
    user-select: none;
    background-color: #007bff;
    color: white;
    border: none;
}
```

#### Шаг 6. Реализация функционала добавления строк (`table.js`)

Добавляем обработчик события для кнопки:
```javascript
document.addEventListener('DOMContentLoaded', function() {
    const button = document.querySelector('#new-row button');

    button.addEventListener('click', function() {
        // Получаем значения из формы
        const week = document.querySelector('input[name="week"]').value;
        const title = document.querySelector('input[name="title"]').value;
        const subject = document.querySelector('select[name="subject"]').value;
        const date = document.querySelector('input[name="date"]').value;
        const type = document.querySelector('select[name="type"]').value;

        // Определяем класс для ячейки в зависимости от значения
        let subjectClass = subject === 'Технологии' ? 'span-green' : 'span-blue';
        let typeClass = '';
        switch(type) {
            case 'Лекция': typeClass = 'span-gold'; break;
            case 'Workshop': typeClass = 'span-green'; break;
            case 'Review': typeClass = 'span-red'; break;
            case 'Домашка':
