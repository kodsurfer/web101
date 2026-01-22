# Лекция: Основы JavaScript - Операторы сравнения, условия, логика и объекты

## Часть 1: Введение

**Почему веб-дизайнеру нужно знать JavaScript?**

Как современный веб-дизайнер, вы не просто создаете статичные макеты. Вы проектируете интерактивные интерфейсы, анимации, формы с валидацией - все это требует понимания основ программирования. JavaScript - это язык, который "оживляет" ваши дизайны.

## Часть 2: Операторы сравнения

### Базовые операторы

В веб-разработке сравнения используются постоянно:
- Проверка заполнения полей формы
- Сравнение значений в фильтрах
- Валидация данных

```javascript
// Пример из реальной практики - проверка возраста
let userAge = parseInt(prompt("Введите ваш возраст", ""));

if (userAge >= 18) {
    showAdultContent(); // показываем контент для взрослых
} else {
    showChildContent(); // показываем детский контент
}
```

### Особенности сравнения в JavaScript

**Важное правило:** JavaScript часто пытается "угадать", что вы имели в виду, что может приводить к неожиданным результатам.

```javascript
// Потенциальная проблема в формах
let formValue = "0"; // строка из поля ввода
let expectedValue = 0; // число для сравнения

console.log(formValue == expectedValue); // true - может быть неочевидно!
console.log(formValue === expectedValue); // false - разные типы
```

### Сравнение строк в интерфейсах

```javascript
// Сортировка или фильтрация по алфавиту
let products = ["Яблоки", "Апельсины", "бананы"]; // Обратите внимание на регистр!
products.sort(); // ["Апельсины", "Яблоки", "бананы"] - строчные буквы "тяжелее"
```

**Совет:** При проектировании сортировок учитывайте регистр символов. Возможно, стоит приводить все к одному регистру перед сравнением.

## Часть 3: Строгое vs Нестрогое сравнение

### Почему строгое сравнение важно?

```javascript
// Типичный сценарий формы
function validateForm(inputValue) {
    // Плохо - пропустит пустую строку или 0
    if (inputValue == false) {
        return "Поле не заполнено";
    }
    
    // Хорошо - точная проверка
    if (inputValue === "" || inputValue === null || inputValue === undefined) {
        return "Поле не заполнено";
    }
}

// Для веб-дизайнера: проектируя формы, продумывайте все возможные состояния полей
```

## Часть 4: Условное ветвление в интерфейсах

### Практическое применение if/else

```javascript
// Изменение интерфейса в зависимости от условий
function updateUI(user) {
    if (user.isLoggedIn) {
        showUserMenu(user.name);
        if (user.isAdmin) {
            showAdminPanel();
        }
    } else {
        showLoginButton();
    }
}

// Тернарный оператор для кратких условий
let buttonColor = (user.subscription === "premium") ? "gold" : "blue";
```

### Цепочки условий для сложных сценариев

```javascript
// Определение типа контента для отображения
function getContentType(user) {
    if (user.age < 13) {
        return "child";
    } else if (user.age < 18) {
        return "teen";
    } else if (user.age < 65) {
        return "adult";
    } else {
        return "senior";
    }
}
```

## Часть 5: Логические операторы в UI/UX

### Использование ИЛИ (||) для значений по умолчанию

```javascript
// Установка значений по умолчанию в настройках
function getUserSettings(user) {
    return {
        theme: user.theme || "light", // если тема не задана, используем светлую
        fontSize: user.fontSize || 16,
        language: user.language || "ru"
    };
}

// В дизайне: всегда предусматривайте fallback-значения
```

### Использование И (&&) для условного отображения

```javascript
// Условный рендеринг элементов интерфейса
function renderDashboard(user) {
    return `
        <div class="dashboard">
            <h1>Добро пожаловать, ${user.name}</h1>
            ${user.hasNotifications && `<div class="notifications">У вас есть уведомления</div>`}
            ${user.isPremium && `<div class="premium-badge">Premium</div>`}
        </div>
    `;
}
```

## Часть 6: Объекты - организация данных в дизайне

### Объекты для хранения состояний UI

```javascript
// Состояние модального окна
let modalState = {
    isOpen: false,
    title: "Подтверждение",
    content: "Вы уверены?",
    buttons: [
        { text: "OK", action: confirmAction },
        { text: "Cancel", action: closeModal }
    ]
};

// Состояние пользователя
let currentUser = {
    id: 123,
    name: "Анна",
    email: "anna@example.com",
    preferences: {
        theme: "dark",
        notifications: true,
        language: "ru"
    },
    // Методы (функции) тоже могут быть в объекте
    getDisplayName: function() {
        return `${this.name} (${this.email})`;
    }
};
```

### Динамические свойства для адаптивных интерфейсов

```javascript
// Генерация классов CSS на основе состояния
function getButtonClasses(props) {
    let classes = {
        'btn': true,
        'btn-primary': props.variant === 'primary',
        'btn-large': props.size === 'large',
        'btn-disabled': props.disabled,
        'btn-loading': props.loading
    };
    
    // Преобразуем в строку классов
    return Object.keys(classes)
        .filter(key => classes[key])
        .join(' ');
}

// Использование
let buttonClass = getButtonClasses({
    variant: 'primary',
    size: 'large',
    disabled: false,
    loading: true
}); // "btn btn-primary btn-large btn-loading"
```

## Часть 7: Практические советы

### 1. Всегда используйте строгое сравнение (===)
```javascript
// Хорошо
if (inputValue === "") { /* ... */ }

// Плохо
if (inputValue == "") { /* ... */ }
```

### 2. Проверяйте существование свойств
```javascript
// Безопасный доступ к вложенным свойствам
let userTheme = (user && user.preferences && user.preferences.theme) || "light";

// Или с современным JavaScript (optional chaining)
let userTheme = user?.preferences?.theme || "light";
```

### 3. Используйте логические операторы для чистого кода
```javascript
// Вместо этого:
if (user) {
    if (user.isActive) {
        showProfile();
    }
}

// Пишите так:
user && user.isActive && showProfile();
```

### 4. Организуйте конфигурации в объекты
```javascript
// Конфиг стилей
const theme = {
    colors: {
        primary: '#3498db',
        secondary: '#2ecc71',
        danger: '#e74c3c'
    },
    spacing: {
        small: '8px',
        medium: '16px',
        large: '24px'
    }
};

// Использование
element.style.backgroundColor = theme.colors.primary;
```

## Практическое задание

**Создайте систему тем для веб-сайта:**

1. Создайте объект, содержащий две темы (light и dark) с цветами, шрифтами и размерами
2. Напишите функцию, которая переключает темы
3. Добавьте условие, которое сохраняет выбор пользователя в localStorage
4. При загрузке страницы проверяйте сохраненную тему

```javascript
// Пример структуры
const themes = {
    light: {
        backgroundColor: '#ffffff',
        textColor: '#333333',
        primaryColor: '#3498db'
    },
    dark: {
        backgroundColor: '#1a1a1a',
        textColor: '#ffffff',
        primaryColor: '#2980b9'
    }
};

function applyTheme(themeName) {
    const theme = themes[themeName];
    document.documentElement.style.setProperty('--bg-color', theme.backgroundColor);
    document.documentElement.style.setProperty('--text-color', theme.textColor);
    document.documentElement.style.setProperty('--primary-color', theme.primaryColor);
}
```

## Заключение

Понимание этих основ JavaScript поможет вам:
- Создавать более интерактивные и умные прототипы
- Эффективнее общаться с разработчиками
- Проектировать интерфейсы с учетом технических возможностей
- Быстрее тестировать свои дизайн-идеи

**Запомните:** Хороший веб-программист/дизайнер сегодня - это не просто художник, а проектировщик взаимодействий, который понимает, как его дизайн будет реализован технически.
