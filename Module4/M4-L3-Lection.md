# Лекция по веб-разработке: Вёрстка таблиц и динамическое добавление данных

**Тема:** Таблицы в HTML5/CSS3, стилизация и работа с формой для добавления строк (JavaScript)

## 1. Введение: Зачем дизайнеру знать про таблицы?

На прошлом занятии мы разобрали, как верстать многостраничный сайт, оформлять меню и возиться с формами. Сегодня мы погружаемся в мир таблиц.

**Важный дисклеймер:** Верстать таблицы — не самое удобное занятие. Таблицы существуют с первых версий HTML (HTML 4 / CSS 2) и до сих пор живут по старым правилам. На них **не действуют** современные свойства CSS3, такие как `flexbox` и `grid`.

### Где в реальной жизни дизайнеру пригодятся таблицы?
1.  **Вывод табличных данных** (расписания, прайс-листы, спецификации) на сайте.
2.  **Email-рассылки** — это, пожалуй, главное применение. Почтовые клиенты (Gmail, Outlook, Mail.ru, приложения на телефонах) не поддерживают `flex` или `grid`. Единственный способ сверстать красивую сетку в письме — это таблицы. Существуют даже специальные сервисы для тестирования вёрстки писем, но самый надежный способ — завести ящики во всех клиентах и проверять вручную.

## 2. Структура таблицы (HTML)

Создадим стандартную структуру файлов:
*   `table.html`
*   `css/table.css`
*   `js/table.js`

### Основные теги таблицы

| Тег | Расшифровка | Назначение |
| :--- | :--- | :--- |
| `

Верстка таблицы идет **по строкам (строка за строкой)**.

### Пример: Сетка занятия (Модуль 4)
Допустим, у нас есть данные о занятиях (Неделя, Название, Предмет, Дата, Тип).

#### Логическая структура (рекомендуется для больших таблиц)
Чтобы не запутаться в коде, используем логические блоки:

```html
<table>
  <!-- Шапка таблицы -->
  <thead>
    <tr>
      <th>Неделя</th>
      <th>Название</th>
      <th>Предмет</th>
      <th>Дата</th>
      <th>Тип</th>
      <th></th> <!-- Пустой столбец для действий -->
    </tr>
  </thead>

  <!-- Тело таблицы (основной контент) -->
  <tbody>
    <tr>
      <td>4.1</td>
      <td>Верстаем форму</td>
      <td><span class="tag-red">Технологии</span></td>
      <td>02.04.2025</td>
      <td><span class="tag-green">Воркшоп</span></td>
      <td></td>
    </tr>
    <tr>
      <td>4.2</td>
      <td>Вводная лекция</td>
      <td><span class="tag-blue">Арт-практика</span></td>
      <td>09.04.2025</td>
      <td><span class="tag-gold">Лекция</span></td>
      <td></td>
    </tr>
    <!-- ... остальные строки ... -->
  </tbody>

  <!-- <tfoot> для итогов, но в нашем примере его не будет -->
</table>
```

> **Совет:** Используйте отступы в коде. Это делает структуру «воздушной» и легко читаемой, даже если строк много.

## 3. Стилизация таблицы (CSS)

«Из коробки» таблицы выглядят как привет из 90-х. Наша задача — привести их к современному виду.

### Базовый сброс и шрифты
```css
* {
  box-sizing: border-box;
}

body {
  font-family: system-ui, -apple-system, 'Segoe UI', Roboto, sans-serif;
  font-size: 14px;
}
```

### Оформление таблицы и ячеек
Ключевое свойство — `border-collapse: collapse;`. Оно убирает двойные границы между ячейками.

```css
table {
  width: 100%;          /* Резиновая ширина */
  border-collapse: collapse;
}

th, td {
  font-weight: 400;     /* Убираем жирность у заголовков */
  text-align: left;
  padding: 12px 16px;   /* Воздух внутри ячеек */
  height: 48px;
  border: 1px solid #eee; /* Легкая сетка */
}

/* Убираем внешние границы, чтобы таблица не была "заперта в рамку" */
tr th:first-child, tr td:first-child { border-left: none; }
tr th:last-child, tr td:last-child { border-right: none; }
```

### Настройка ширины колонок
Так как мы не используем сложные гриды, задаем ширину через псевдоклассы `:nth-of-type()`.

```css
/* 1-я колонка (Неделя) — 10% */
tr td:nth-of-type(1) { width: 10%; }
/* 2-я колонка (Название) — самая широкая, 35% */
tr td:nth-of-type(2) { width: 35%; }
/* 3-я (Предмет) — 15% */
tr td:nth-of-type(3) { width: 15%; }
/* 4-я (Дата) — 15% */
tr td:nth-of-type(4) { width: 15%; }
/* 5-я (Тип) — 15% */
tr td:nth-of-type(5) { width: 15%; }
/* 6-я (пустая) — фиксированная ширина */
tr td:nth-of-type(6) { width: 60px; }
```

### Стилизация тегов (спанов)
Внутри ячеек с типами занятий и предметами используем `<span>` с классами.

**HTML:**
```html
<td><span class="tag tag-green">Воркшоп</span></td>
```

**CSS:**
```css
td span {
  display: inline-block;
  padding: 4px 12px;
  border-radius: 16px;
  font-size: 13px;
  font-weight: 500;
}

.tag-red { background: #fdecea; color: #d32f2f; }
.tag-blue { background: #e3f2fd; color: #1976d2; }
.tag-green { background: #e8f5e9; color: #388e3c; }
.tag-gold { background: #fff8e1; color: #f57c00; }
.tag-purple { background: #f3e5f5; color: #7b1fa2; }
```

### Добавление иконок в заголовки через псевдоэлементы
Используем `::before` и `position: relative`, чтобы добавить иконки слева от текста.

```css
/* Родительский спан в заголовке должен быть relative */
thead th span {
  position: relative;
  padding-left: 28px;
}

/* Общие стили для иконки */
thead th span::before {
  content: '';
  position: absolute;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
  width: 18px;
  height: 18px;
  background-size: contain;
  background-repeat: no-repeat;
}

/* Конкретные иконки для каждого столбца */
.week::before { background-image: url('../images/icons/calendar.svg'); }
.name::before { background-image: url('../images/icons/topic.svg'); }
.subject::before { background-image: url('../images/icons/subject.svg'); }
.date::before { background-image: url('../images/icons/date.svg'); }
.type::before { background-image: url('../images/icons/type.svg'); }
```

## 4. Форма для добавления новых строк (HTML + CSS)

Под таблицей создадим форму, которая визуально продолжает таблицу.

```html
<form id="new-row-form" class="table-form">
  <input type="text" name="week" placeholder="Неделя (4.5)" value="">
  <input type="text" name="name" placeholder="Название занятия">
  <select name="subject">
    <option value="Технологии">Технологии</option>
    <option value="Арт-практика">Арт-практика</option>
  </select>
  <input type="text" name="date" placeholder="ДД.ММ.ГГГГ">
  <select name="type" id="type">
    <option value="Лекция">Лекция</option>
    <option value="Воркшоп">Воркшоп</option>
    <option value="Ревью">Ревью</option>
    <option value="Домашка">Домашка</option>
  </select>
  <button type="button" id="add-row-btn">+</button>
</form>
```

**Стилизация формы (чтобы она сливалась с таблицей):**
```css
.table-form {
  display: flex;
  width: 100%;
  border-top: none; /* Убираем верхнюю границу */
}

/* Все элементы формы растягиваются пропорционально колонкам таблицы */
.table-form input, .table-form select, .table-form button {
  padding: 12px 16px;
  border: 1px solid #eee;
  border-top: none;
  font-family: inherit;
  font-size: 14px;
}

/* Ширина элементов формы соответствует ширине ячеек таблицы */
.table-form input[name="week"] { width: 10%; }
.table-form input[name="name"] { width: 35%; }
.table-form select[name="subject"] { width: 15%; }
.table-form input[name="date"] { width: 15%; }
.table-form select[name="type"] { width: 15%; }
.table-form button { width: 60px; cursor: pointer; background: #f5f5f5; }
```

## 5. Добавление динамики (JavaScript)

Наша цель: при нажатии на «+» создавать новую строку в `<tbody>` и очищать форму.

### Логика работы скрипта

1.  Навесить обработчик клика на кнопку.
2.  Считать значения из полей формы.
3.  Создать новые `<td>` и наполнить их данными.
4.  Для тегов (Тип и Предмет) динамически добавить нужный CSS-класс.
5.  Собрать ячейки в строку `<tr>`.
6.  Добавить строку в конец `<tbody>`.
7.  Очистить поля формы.

### Код (файл `table.js`)

```javascript
document.addEventListener('DOMContentLoaded', function() {
  const addButton = document.getElementById('add-row-btn');
  const form = document.getElementById('new-row-form');
  const tbody = document.querySelector('tbody');

  // Функция для получения цвета тега (предмет)
  function getSubjectColor(subject) {
    return subject === 'Технологии' ? 'tag-red' : 'tag-blue';
  }

  // Функция для получения цвета тега (тип занятия)
  function getTypeColor(type) {
    switch(type) {
      case 'Лекция': return 'tag-gold';
      case 'Воркшоп': return 'tag-green';
      case 'Ревью': return 'tag-red';
      case 'Домашка': return 'tag-purple';
      default: return '';
    }
  }

  addButton.addEventListener('click', function() {
    // 1. Получаем значения из полей
    const week = form.querySelector('input[name="week"]').value;
    const name = form.querySelector('input[name="name"]').value;
    const subject = form.querySelector('select[name="subject"]').value;
    const date = form.querySelector('input[name="date"]').value;
    const type = form.querySelector('select[name="type"]').value;

    // Простая валидация: если поля пустые, не добавляем
    if (!week || !name || !date) return;

    // 2. Создаем ячейки (td)
    const tdWeek = document.createElement('td');
    tdWeek.textContent = week;

    const tdName = document.createElement('td');
    tdName.textContent = name;

    // Ячейка с предметом (внутри span)
    const tdSubject = document.createElement('td');
    const subjectSpan = document.createElement('span');
    subjectSpan.textContent = subject;
    subjectSpan.className = `tag ${getSubjectColor(subject)}`;
    tdSubject.appendChild(subjectSpan);

    const tdDate = document.createElement('td');
    tdDate.textContent = date;

    // Ячейка с типом (внутри span)
    const tdType = document.createElement('td');
    const typeSpan = document.createElement('span');
    typeSpan.textContent = type;
    typeSpan.className = `tag ${getTypeColor(type)}`;
    tdType.appendChild(typeSpan);

    // Пустая ячейка для действий (можно потом добавить кнопку удаления)
    const tdEmpty = document.createElement('td');
    tdEmpty.textContent = '';

    // 3. Собираем строку
    const newRow = document.createElement('tr');
    newRow.appendChild(tdWeek);
    newRow.appendChild(tdName);
    newRow.appendChild(tdSubject);
    newRow.appendChild(tdDate);
    newRow.appendChild(tdType);
    newRow.appendChild(tdEmpty);

    // 4. Добавляем в таблицу
    tbody.appendChild(newRow);

    // 5. Очищаем форму
    form.reset();
  });
});
```

## 6. Задание для самостоятельной работы (HW)

1.  **Доделать таблицу:** Доверстать все строки из макета до 4.4 занятия (про типографику и UI Kit).
2.  **Цвета:** Подобрать свои оттенки для тегов, используя систему дизайна (можно взять цвета из вашего UI Kit).
3.  **Иконки:** Найти или нарисовать SVG-иконки для заголовков таблицы.
4.  **Улучшение JS (по желанию):**
    *   Добавить кнопку «🗑️» в последнюю пустую ячейку, чтобы удалять строки.
    *   Реализовать сохранение данных в `localStorage`, чтобы таблица не исчезала после перезагрузки страницы.

## 7. Бонус: Что дальше? 3D на сайте

В следующем модуле мы можем разобрать, как добавить на сайт 3D-объекты, которые движутся при скролле. Это делается с помощью библиотеки **Three.js**. Плоскую картинку можно крутить через `transform: rotateY(…)`, но для настоящего 3D (кубы, сложные модели) нужна библиотека. Если будет интересно — посмотрим воркшоп по Three.js.
