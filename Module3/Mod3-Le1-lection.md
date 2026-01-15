# Основы JavaScript

## Часть 1: Введение (15 минут)

### Что мы изучим сегодня?
JavaScript — язык, который "оживляет" веб-страницы. Сегодня мы разберем:
1. Как JavaScript взаимодействует с HTML/CSS
2. Основы синтаксиса JavaScript
3. Работу с переменными и типами данных
4. Взаимодействие с пользователем

### Практическая значимость для веб-дизайнеров
- Создание интерактивных интерфейсов
- Динамическое изменение контента
- Валидация форм
- Анимации и переходы
- Работа с API

---

## Часть 2: Основы JavaScript (20 минут)

### Встраивание JavaScript в HTML
```html
<!-- Способ 1: Внутри тега <script> -->
<script>
  alert('Привет, мир!');
</script>

<!-- Способ 2: Внешний файл -->
<script src="script.js"></script>
```

### Инструкции и точки с запятой
**Практическое правило:** Всегда ставьте точку с запятой!
```javascript
// Хорошо
alert('Привет');
alert('Мир');

// Рискованно
alert('Привет')
alert('Мир')
```

### ⚠️ Пример опасного кода
```javascript
// Без точки с запятой может сломаться!
alert("Ошибка")

[1, 2].forEach(alert)  // Ошибка!

// Всегда так:
alert("Всё работает");
[1, 2].forEach(alert);
```

---

## Часть 3: Переменные и типы данных (25 минут)

### Создание переменных
```javascript
// Современный способ (используйте всегда!)
let message = 'Привет';
const PI = 3.14;  // Константа

// Устаревший (избегайте)
var oldWay = 'Не используйте';
```

### Правила именования
```javascript
// Можно
let userName;
let test123;
let $price;
let _private;

// Нельзя
let 123test;     // Начинается с цифры
let user-name;   // Дефис не разрешен
let let;         // Зарезервированное слово
```

### Типы данных в JavaScript
```javascript
// 1. Числа
let age = 25;
let price = 9.99;
let infinity = Infinity;
let notANumber = NaN;

// 2. BigInt (для очень больших чисел)
const bigNumber = 1234567890123456789012345678901234567890n;

// 3. Строки
let single = 'одинарные';
let double = "двойные";
let backticks = `Можно вставлять ${age} лет`;

// 4. Булевые значения
let isStudent = true;
let isWorking = false;

// 5. Null (пустое значение)
let emptyValue = null;

// 6. Undefined (не присвоено)
let notAssigned;
console.log(notAssigned); // undefined

// 7. Объекты (позже подробнее)
let user = { name: "Анна", age: 25 };

// 8. Символы (для уникальных идентификаторов)
let id = Symbol("id");
```

### Проверка типов
```javascript
typeof "Текст"     // "string"
typeof 42          // "number"
typeof true        // "boolean"
typeof undefined   // "undefined"
typeof null        // "object" (историческая ошибка!)
typeof alert       // "function"
```

---

## Часть 4: Взаимодействие с пользователем (20 минут)

### Три основных функции
```javascript
// 1. Alert - простое сообщение
alert("Добро пожаловать на наш сайт!");

// 2. Prompt - запрос ввода
let userName = prompt("Как вас зовут?", "Гость");
// Второй параметр - значение по умолчанию

// 3. Confirm - подтверждение
let isAdult = confirm("Вам есть 18 лет?");
// Возвращает true/false
```

### Практический пример формы
```javascript
// Симуляция регистрации
let name = prompt("Введите ваше имя:");
let age = prompt("Сколько вам лет?");
let isStudent = confirm("Вы студент?");

alert(`Привет, ${name}! 
Вам ${age} лет.
Статус студента: ${isStudent ? "да" : "нет"}`);
```

---

## Часть 5: Воркшоп - Практические задания (40 минут)

### Задание 1: Простой калькулятор возраста
```javascript
// Ваш код здесь:
let birthYear = prompt("В каком году вы родились?");
let currentYear = 2024; // Можно динамически получить
let age = currentYear - birthYear;
alert(`Вам примерно ${age} лет`);
```

### Задание 2: Изменение CSS через JavaScript
```html
<div id="myBox" style="width: 100px; height: 100px; background: blue;"></div>
<script>
  // Измените цвет и размер блока
  let box = document.getElementById('myBox');
  box.style.background = 'red';
  box.style.width = '200px';
</script>
```

### Задание 3: Обработка кликов
```html
<button id="myButton">Нажми меня</button>
<p id="message"></p>

<script>
  let button = document.getElementById('myButton');
  let message = document.getElementById('message');
  
  button.onclick = function() {
    message.textContent = "Кнопка нажата!";
    message.style.color = "green";
  };
</script>
```

### Задание 4: Валидация формы
```javascript
// Проверка пароля
let password = prompt("Придумайте пароль (минимум 6 символов):");

if (password.length < 6) {
  alert("Пароль слишком короткий!");
} else if (password === "123456") {
  alert("Слишком простой пароль!");
} else {
  alert("Отличный пароль!");
}
```

---

## Часть 6: Продвинутые возможности (10 минут)

### Что еще может JavaScript?

1. **Работа с DOM**:
```javascript
// Добавление нового элемента
let newDiv = document.createElement('div');
newDiv.innerHTML = 'Новый контент';
document.body.appendChild(newDiv);
```

2. **Обработка событий**:
```javascript
element.addEventListener('click', function() {
  // Действие при клике
});

// События: click, mouseover, keydown, scroll и др.
```

3. **Анимации**:
```javascript
// Плавное изменение
element.style.transition = 'all 0.3s';
element.style.transform = 'scale(1.2)';
```

4. **Работа с формами**:
```javascript
// Валидация, автозаполнение, маски ввода
```

---

## Домашнее задание

### Проект: "Интерактивная визитка"

Создайте HTML-страницу с:
1. Динамически изменяемым именем (через prompt)
2. Возможностью сменить цвет фона (через confirm)
3. Счетчиком кликов по кнопке
4. Простым калькулятором (сложение двух чисел)

### Требования:
- Использовать переменные с правильными именами
- Обрабатывать возможные ошибки ввода
- Комментировать код
- Использовать различные типы данных

---

## Ресурсы для дальнейшего изучения

1. [MDN JavaScript Guide](https://developer.mozilla.org/ru/docs/Web/JavaScript/Guide)
2. [Learn JavaScript](https://learn.javascript.ru/)
3. [JavaScript.info](https://javascript.info/)
4. [Codecademy JavaScript Course](https://www.codecademy.com/learn/introduction-to-javascript)

---

## Ключевые выводы

1. JavaScript делает веб-страницы интерактивными
2. Всегда используйте `let` и `const`, а не `var`
3. Не забывайте про точки с запятой
4. JavaScript имеет динамическую типизацию
5. `alert`, `prompt`, `confirm` — базовые инструменты взаимодействия
6. Практика — лучший способ обучения!

**Запомните:** JavaScript для веб-дизайнера — это не о сложных алгоритмах, а о создании удобного и интерактивного пользовательского опыта!
