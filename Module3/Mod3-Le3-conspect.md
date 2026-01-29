### **Введение в JavaScript для веб-дизайнеров. От статики к интерактиву**

**Цель:** Превратить статичный HTML/CSS-макет в интерактивный, научившись управлять элементами страницы с помощью JavaScript.

---

#### **Часть 1: Настройка проекта (10 минут)**

**Теория:** JavaScript (JS) — язык программирования, который делает веб-страницы "живыми". Он выполняется в браузере пользователя.

**Задание 1: Создаем структуру папок и файлов.**
1.  Создайте папку для проекта `js_workshop`.
2.  Внутри нее создайте:
    *   Файл `index.html`
    *   Папку `styles`
    *   Папку `js`

**Задание 2: Наполняем файлы.**
1.  В `index.html` вставьте базовую структуру (в VS Code наберите `!` и Tab):
    ```html
    <!DOCTYPE html>
    <html lang="ru">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Мастерская: JS Функции</title>
        <link rel="stylesheet" href="styles/style.css">
    </head>
    <body>
        <h1>Наш интерактивный полигон</h1>

        <!-- Сюда будем добавлять элементы -->
        <div id="demo">Доброе утро</div>

        <script src="js/script.js"></script>
    </body>
    </html>
    ```
2.  В папке `styles` создайте файл `style.css` и задайте базовые стили:
    ```css
    * {
        box-sizing: border-box;
    }
    body {
        font-family: sans-serif;
        padding: 20px;
        line-height: 1.6;
    }
    #demo {
        padding: 20px;
        margin: 20px 0;
        background-color: #f0f0f0;
        border: 1px dashed #ccc;
        cursor: pointer;
        transition: all 0.3s ease;
    }
    ```
3.  В папке `js` создайте файл `script.js`. Пока что оставьте его пустым.

**Проверка:** Откройте `index.html` в браузере. Вы должны увидеть заголовок и серый блок с текстом.

---

#### **Часть 2: DOM и Поиск Элементов (20 минут)**

**Теория:** DOM (Document Object Model) — это дерево объектов, которое браузер создает из HTML-кода. JavaScript может "дотронуться" до любого элемента в этом дереве, чтобы изменить его.

**Ключевой момент:** Код должен выполняться **после** загрузки DOM.
В `script.js`:
```javascript
// Ожидаем полной загрузки DOM
document.addEventListener('DOMContentLoaded', function() {
    console.log('DOM загружен! Можно работать с элементами.');
    // Весь наш код будем писать внутри этой функции!
});
```

**Задание 3: Поиск элементов.**
1.  В `index.html` внутри `<body>` добавьте:
    ```html
    <h2>Обычный заголовок h2</h2>
    <h2 class="header">Заголовок с классом .header</h2>
    <h2 id="header">Заголовок с id #header</h2>
    ```
2.  В `script.js` внутри `DOMContentLoaded` найдите эти элементы и выведите их в консоль браузера (F12 -> Console):
    ```javascript
    // Находим ПЕРВЫЙ элемент h2
    const headerByTag = document.querySelector('h2');
    console.log('По тегу:', headerByTag);

    // Находим элемент по КЛАССУ .header
    const headerByClass = document.querySelector('.header');
    console.log('По классу:', headerByClass);

    // Находим элемент по ID #header
    const headerById = document.querySelector('#header');
    console.log('По ID:', headerById);

    // Практика: Найдите div с id="demo" и сохраните в переменную demoDiv
    const demoDiv = document.querySelector('#demo');
    console.log('Наш демо-блок:', demoDiv);
    ```

**Итог:** `querySelector` — ваш главный инструмент для поиска. Используйте CSS-селекторы (тег, `.класс`, `#id`).

---

#### **Часть 3: Манипуляции с контентом и простые события (25 минут)**

**Теория:** Найдя элемент, мы можем менять его содержимое и "слушать" действия пользователя.

**Задание 4: Меняем текст и реагируем на клик.**
1.  Работа с содержимым. Попробуйте в консоли (или в коде):
    ```javascript
    demoDiv.innerHTML = '<strong>Добрый день!</strong>'; // Вставляет HTML
    demoDiv.innerText = 'Добрый вечер!'; // Вставляет только текст
    ```
2.  Обработка события. Заставим блок менять текст по клику:
    ```javascript
    demoDiv.addEventListener('click', function() {
        console.log('По div кликнули!');
        // Меняем текст при каждом клике
        if (demoDiv.innerText === 'Доброе утро') {
            demoDiv.innerText = 'Добрый день';
        } else if (demoDiv.innerText === 'Добрый день') {
            demoDiv.innerText = 'Добрый вечер';
        } else {
            demoDiv.innerText = 'Доброе утро';
        }
    });
    ```
    **Объяснение:** `addEventListener` — "навешивает слушателя" на элемент. Первый аргумент — тип события ('click'), второй — функция, которая выполнится при событии.

**Задание 5: Работа с пользовательским вводом.**
1.  В `index.html` добавьте кнопку:
    ```html
    <button class="btn">Представиться</button>
    ```
2.  В `script.js` найдите кнопку и добавьте обработчик:
    ```javascript
    const greetButton = document.querySelector('.btn');
    greetButton.addEventListener('click', function() {
        // prompt показывает диалоговое окно с запросом
        const userName = prompt('Как вас зовут?', 'Иван');
        
        // Обрабатываем результат
        if (userName === null) {
            // Пользователь нажал "Отмена"
            demoDiv.innerText = 'Ну, как знаешь...';
        } else if (userName.trim() === '') {
            // Пользователь ввел пустую строку или пробелы
            demoDiv.innerText = 'Привет, человек без имени!';
        } else {
            // Пользователь ввел имя
            demoDiv.innerText = `Привет, ${userName}!`;
        }
    });
    ```

---

#### **Часть 4: Управление CSS-классами (25 минут)**

**Теория:** Прямое изменение стилей (`element.style.color='red'`) неудобно. Лучше менять классы, а стили описывать в CSS.

**Задание 6: Добавляем и удаляем классы.**
1.  В `style.css` добавьте стили для новых классов:
    ```css
    .red {
        background-color: #ff6b6b;
        color: white;
    }
    .big {
        width: 100%;
        height: 200px;
        font-size: 2em;
    }
    .circle {
        width: 100px;
        height: 100px;
        border-radius: 50%;
        background-color: #4ecdc4;
        margin: 10px;
        display: inline-block;
        transition: all 0.5s ease;
    }
    ```
2.  В `script.js` научим кнопку менять стиль `demoDiv`:
    ```javascript
    // Найдем новую кнопку (добавьте ее в HTML: <button id="styleBtn">Изменить стиль</button>)
    const styleButton = document.querySelector('#styleBtn');

    styleButton.addEventListener('click', function() {
        // Метод .toggle() - если класса нет, добавит, если есть - уберет.
        demoDiv.classList.toggle('red');
        demoDiv.classList.toggle('big');
        
        // Другие полезные методы:
        // demoDiv.classList.add('red'); // Добавить
        // demoDiv.classList.remove('big'); // Удалить
        // console.log(demoDiv.classList.contains('red')); // Проверить наличие (true/false)
    });
    ```

---

#### **Часть 5: Работа с множеством элементов (20 минут)**

**Теория:** `querySelectorAll` возвращает не один элемент, а коллекцию (похожую на массив). С ней нужно работать особым образом.

**Задание 7: Создаем и анимируем множество элементов.**
1.  В `index.html` добавьте контейнер для кружков:
    ```html
    <div id="circles-container"></div>
    ```
2.  В `script.js` сгенерируем круги динамически и добавим им интерактивность:
    ```javascript
    const container = document.querySelector('#circles-container');
    
    // Создаем 5 кругов
    for (let i = 1; i <= 5; i++) {
        const circle = document.createElement('div');
        circle.classList.add('circle');
        circle.innerText = i; // Номер внутри круга
        container.appendChild(circle);
    }
    
    // НЕПРАВИЛЬНО: circles.addEventListener('click', ...) - не сработает!
    // ПРАВИЛЬНО: Находим ВСЕ круги и каждому вешаем слушателя
    const allCircles = document.querySelectorAll('.circle');
    
    // Перебираем коллекцию методом .forEach()
    allCircles.forEach(function(oneCircle) {
        oneCircle.addEventListener('click', function() {
            // При клике на круг переключаем у НЕГО класс .red
            oneCircle.classList.toggle('red');
            // Меняем его размер
            oneCircle.classList.toggle('big');
        });
    });
    ```

**Суть:** `forEach` — это цикл для коллекций. Функция внутри него выполнится для **каждого** элемента.

---

#### **Финальное Практическое Задание (Самостоятельно)**

Создайте "интерактивную визитку":
1.  В `index.html` добавьте блок с вашим именем, фото (или аватаркой) и текстом "Нажми, чтобы узнать больше".
2.  С помощью JS:
    *   По клику на блок меняйте его фоновый цвет (используйте `classList.toggle`).
    *   По клику меняйте текст внутри на что-то о вас (используйте `innerText`).
    *   Добавьте кнопку "Показать/скрыть контакты". При ее нажатии должен появляться/исчезать дополнительный блок с контактами (используйте `classList.toggle` на скрытом блоке и, возможно, `display: none` в CSS).

---

#### **Итоги и Дальнейший Путь:**

Сегодня вы научились:
✅ Настраивать JS в проекте.
✅ Находить элементы в DOM с помощью `querySelector` и `querySelectorAll`.
✅ Менять содержимое элементов (`innerHTML`, `innerText`).
✅ Реагировать на действия пользователя (`addEventListener`).
✅ Управлять CSS-классами для анимации и изменения стилей (`classList`).
✅ Работать с группами элементов (цикл `forEach`).

**Следующий шаг:** Углубленное изучение **функций**, **объектов** и способов **хранения данных** в JavaScript.

**Домашнее задание:** Реализуйте финальное задание. Поэкспериментируйте: добавьте второй набор кругов, которые будут менять цвет не по клику, а при наведении мыши (событие `'mouseover'`).
