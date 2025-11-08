### **Воркшоп: «Мощь Flexbox: От основ до сложных адаптивных макетов»**

**Цель воркшопа:** Освоить технологию Flexbox на практике, научившись создавать как простые, так и сложные, адаптивные макеты веб-страниц.

**Формат:** Две практические сессии.
*   **Сессия 1:** Повторение и закрепление основ Flexbox.
*   **Сессия 2:** Создание сложного макета с изображениями и работа с адаптивностью.

---

### **Сессия 1: Основы Flexbox**

#### **Введение и Настройка проекта**

1.  **Откройте вашу рабочую папку** в редакторе кода (например, VS Code).
2.  **Создайте файлы:**
    *   `flexbox.html`
    *   `flexbox.css`
3.  **Организуйте базовую HTML-структуру:**

    ```html
    <!DOCTYPE html>
    <html lang="ru">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Мастерская Flexbox</title>
        <link rel="stylesheet" href="flexbox.css">
    </head>
    <body>
        <!-- Наш контейнер для экспериментов -->
        <div class="container"></div>
    </body>
    </html>
    ```

#### **Базовые стили и «Волшебный» `box-sizing`**

1.  **Подключите CSS и проверьте его,** задав временный фон body.
2.  **Сбросим отступы по умолчанию и применим `box-sizing`.** Это критически важный шаг для предсказуемой работы с размерами.

    ```css
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }
    ```

    **Объяснение `box-sizing: border-box`:**
    *   **Без него:** `width: 200px` + `padding: 20px` = элемент займет 240px.
    *   **С ним:** `width: 200px` + `padding: 20px` = элемент останется шириной 200px, а `padding` будет вычтен из внутреннего пространства. Это избавляет от головной боли при расчетах.

#### **Создаем и стилизуем полигон для экспериментов**

1.  **Стилизуем наш контейнер:**

    ```css
    .container {
        width: 100%;
        height: 500px;
        border: 2px dashed black; /* Пунктирная граница для наглядности */
    }
    ```

2.  **Добавим в контейнер три элемента-«атома».** В HTML внутри `<div class="container">`:

    ```html
    <div class="atom">1</div>
    <div class="atom">2</div>
    <div class="atom">3</div>
    ```

3.  **Стилизуем «атомы»:**

    ```css
    .atom {
        width: 100px;
        height: 100px;
        background-color: lightcoral;
        border: 1px solid darkred;
        font-size: 36px;
        color: white;
        text-align: center;
        line-height: 100px; /* Простой способ вертикального выравнивания для одной строки */
    }
    ```

    **Что мы видим?** Элементы расположены вертикально. Это **блочный поток документа** по умолчанию.

#### **Включаем Flexbox!**

1.  **Превращаем контейнер во Flex-контейнер:**

    ```css
    .container {
        display: flex; /* Включаем Flexbox! */
        /* ... остальные стили ... */
    }
    ```

    **Результат:** Элементы мгновенно выстроились в строку! Это значение `flex-direction: row` по умолчанию.

#### **Управление расположением по главной оси (`justify-content`)**

Главная ось по умолчанию — горизонтальная. Управляем распределением элементов по ней.

```css
.container {
    display: flex;
    justify-content: flex-start; /* Значение по умолчанию. Элементы у левого края */
    /* justify-content: flex-end;   Элементы у правого края */
    /* justify-content: center;     Элементы по центру */
    /* justify-content: space-between; Первый и последний элемент у краев, остальное пространство поровну между элементами */
    /* justify-content: space-around;  Пространство вокруг элементов (слева и справа у каждого) */
}
```

> **Задание:** Поменяйте значения `justify-content` в вашем CSS и посмотрите, как меняется布局.

#### **Управление расположением по поперечной оси (`align-items`)**

Поперечная ось по умолчанию — вертикальная. Управляем выравниванием элементов по высоте контейнера.

```css
.container {
    display: flex;
    justify-content: center;
    align-items: center; /* Элементы выровнены по центру по вертикали */
    /* align-items: flex-start; Элементы у верхнего края */
    /* align-items: flex-end;   Элементы у нижнего края */
}
```

> **Задание:** Поэкспериментируйте с комбинациями `justify-content` и `align-items`. Добейтесь полного центрирования всех элементов в контейнере. Это знаменитый «лайфхак» для центрирования!

#### **Меняем направление оси (`flex-direction`)**

Flexbox — гибкая система. Мы можем поменять направление главной оси.

```css
.container {
    display: flex;
    flex-direction: column; /* Главная ось теперь вертикальна! */
    justify-content: center; /* Теперь это свойство работает по вертикали */
    align-items: center;    /* А это — по горизонтали */
}
```

> **Важно:** При смене `flex-direction` свойства `justify-content` и `align-items` меняются осями.

#### **Перенос элементов (`flex-wrap`) и отступы (`gap`)**

1.  **Добавьте больше элементов `.atom` в HTML.**
2.  **Что произошло?** Элементы сжались, чтобы уместиться в одну строку.
3.  **Включим перенос:**

    ```css
    .container {
        display: flex;
        flex-wrap: wrap; /* Элементы теперь переносятся на новую строку */
    }
    ```

4.  **Добавим отступы между элементами:**

    ```css
    .container {
        display: flex;
        flex-wrap: wrap;
        gap: 20px; /* Пространство между элементами и по горизонтали, и по вертикали */
    }
    ```

---

### **Сессия 2: Сложные макеты и Адаптивность**

#### **Подготовка: Работа с изображениями**

1.  **Представьте, что у вас есть ZIP-архив с 36 SVG-иконками.**
2.  **Создадим новую структуру HTML (можно закомментировать старый `.container`):**

    ```html
    <header class="header">
        <div class="header-atom"></div>
        <div class="header-atom"></div>
        <div class="header-atom"></div>
        <!-- ... и так 10 раз ... -->
    </header>
    <main class="main">
        <aside class="left-main">
            <div class="left-main-atom"></div>
            <!-- ... несколько таких элементов ... -->
        </aside>
        <section class="right-main">
            <div class="right-main-atom">Hello</div>
            <div class="right-main-atom">World</div>
        </section>
    </main>
    ```

#### **Стилизация шапки (`header`)**

1.  **Базовые стили для шапки и ее элементов:**

    ```css
    .header {
        width: 100%;
        height: 15vh; /* Относительная единица, зависящая от высоты окна */
        display: flex;
        justify-content: space-around;
        align-items: center;
        border: 1px solid blue;
    }

    .header-atom {
        width: 10vh;
        height: 10vh;
        border: 1px solid green;
    }
    ```

2.  **Добавляем фоновые изображения для элементов.** Используем `:nth-of-type()` для разнообразия.

    ```css
    .header-atom:nth-of-type(1) {
        background-image: url('images/icon1.svg');
        background-repeat: no-repeat;
        background-size: contain; /* Важно: изображение впишется в элемент */
        background-position: center;
    }
    .header-atom:nth-of-type(2) {
        background-image: url('images/icon2.svg');
        background-repeat: no-repeat;
        background-size: contain;
        background-position: center;
    }
    /* И так далее... */
    ```

3.  **Лайфхак: Поворот изображений.**

    ```css
    .header-atom:nth-of-type(3) {
        /* ... background styles ... */
        transform: rotate(90deg); /* Поворачиваем иконку */
    }
    ```

#### **Создание двухколоночного макета (`main`)**

1.  **Стилизуем основную секцию и колонки:**

    ```css
    .main {
        width: 100%;
        display: flex; /* Делаем сам main flex-контейнером */
    }

    .left-main {
        width: 20%;
        height: 80vh;
        border: 1px solid orange;
        display: flex;
        flex-direction: column; /* Элементы в колонку */
        align-items: center;
        gap: 10px;
        padding-top: 10px;
    }

    .left-main-atom {
        width: 10vh;
        height: 10vh;
        background-image: url('images/icon5.svg');
        background-repeat: no-repeat;
        background-size: contain;
        background-position: center;
        border: 1px solid purple;
    }

    .right-main {
        width: 80%;
        height: 80vh;
        border: 1px solid red;
        display: flex;
        flex-direction: column;
    }

    .right-main-atom {
        width: 100%;
        height: 40vh; /* Два элемента по 40vh = 80vh родителя */
        display: flex;
        justify-content: center;
        align-items: center;
        font-family: Arial, sans-serif;
        font-size: 6vw; /* Относительный размер шрифта */
        border: 1px solid blue;
    }
    ```

#### **Делаем макет адаптивным с помощью Медиазапросов**

1.  **Добавим стили для мобильных устройств (ширина <= 768px):**

    ```css
    /* В конце вашего CSS-файла */
    @media (max-width: 768px) {
        .main {
            flex-direction: column; /* Меняем строку на колонку */
            height: auto;
        }

        .left-main, .right-main {
            width: 100%; /* Занимают всю ширину */
            height: auto; /* Высота определяется контентом */
        }

        .left-main {
            flex-direction: row; /* Элементы в левой колонке теперь в строку */
            flex-wrap: wrap; /* И переносятся */
            justify-content: center;
            order: 2; /* Меняем порядок: левая колонка уходит вниз */
        }

        .right-main {
            order: 1; /* Правая колонка поднимается наверх */
        }

        .header {
            flex-wrap: wrap; /* Элементы в шапке тоже переносятся */
            height: auto;
            padding: 10px 0;
        }

        .header-atom {
            width: 15vw;
            height: 15vw;
        }
    }
    ```

2.  **Продвинутый прием: Показ/скрытие элементов на разных версиях.**
    *   **Добавьте в HTML, внутри `main`, после `right-main` еще 4 квадрата с классом `.nan`.**
    *   **В CSS:**

    ```css
    .nan {
        display: none; /* По умолчанию скрыты на десктопе */
    }

    @media (max-width: 768px) {
        .nan {
            display: block; /* Показываем на мобильных */
            width: 20vw;
            height: 20vw;
            background-color: lightblue;
        }
    }
    ```

#### **Оптимизация и Рефакторинг**

*   **Вынесите повторяющиеся свойства** (например, `background-repeat: no-repeat; background-size: contain;`) в общий класс, чтобы избежать дублирования кода.
*   **Проверьте макет в браузере,** меняя размер окна. Исправляйте мелкие недочеты (например, высоты, отступы).

---

### **Итоги и Домашнее Задание**

**Что мы прошли:**
*   Основы Flexbox: `display: flex`, `justify-content`, `align-items`, `flex-direction`, `flex-wrap`, `gap`.
*   Создание сложного, семантически верного макета (header, main, aside, section).
*   Работу с фоновыми изображениями и их трансформацией.
*   Принципы адаптивного дизайна с помощью медиазапросов.
*   Методы управления порядком элементов (`order`).

**Домашнее задание:**
Создайте свою собственную галерею изображений, используя Flexbox.
1.  Шапка с логотипом (просто блок) и меню (ряд ссылок, выровненных по правому краю).
2.  Основная секция с сеткой изображений (3-4 колонки на десктопе).
3.  На мобильной версии меню должно превращаться в вертикальный список, а сетка изображений — в 2 колонки.
4.  *Задание со звездочкой:* Добавьте hover-эффект для изображений (например, увеличение масштаба с `transform: scale`).

**Следующая тема:** «Погружение в CSS Анимации и Плавные Переходы»!
