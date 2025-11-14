## **Воркшоп: "Оживляем интерфейс: Практика веб-анимации"**

### **Подготовка рабочего пространства**

**1. Создаём структуру проекта:**
```
animation-workshop/
├── index.html
├── styles/
│   └── style.css
├── scripts/
│   └── script.js
└── images/
    └── (будут добавлены позже)
```

**2. Базовый HTML (index.html):**
```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Воркшоп: Веб-анимации</title>
    <link rel="stylesheet" href="styles/style.css">
</head>
<body>
    <header class="workshop-header">
        <h1>🎨 Воркшоп по веб-анимациям</h1>
        <p>Превращаем статику в динамику</p>
    </header>

    <main class="workshop-container">
        <!-- Задания будут добавляться сюда -->
    </main>

    <script src="scripts/script.js"></script>
</body>
</html>
```

**3. Базовые стили (styles/style.css):**
```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Arial', sans-serif;
    line-height: 1.6;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    min-height: 100vh;
}

.workshop-header {
    text-align: center;
    padding: 2rem;
    color: white;
    text-shadow: 0 2px 4px rgba(0,0,0,0.3);
}

.workshop-header h1 {
    font-size: 2.5rem;
    margin-bottom: 0.5rem;
}

.workshop-container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 2rem;
    display: grid;
    gap: 2rem;
}
```

---

## **Задание 1: "Танцующие квадраты" - Осваиваем transform**

**Добавляем в workshop-container:**
```html
<section class="exercise" id="exercise-1">
    <h2>🔄 Задание 1: Трансформации</h2>
    <div class="exercise-description">
        <p>Создайте анимации для квадратов используя свойства transform</p>
    </div>
    
    <div class="transform-playground">
        <div class="square base-square">Базовый</div>
        <div class="square translate-square">Перемещение</div>
        <div class="square scale-square">Масштаб</div>
        <div class="square rotate-square">Вращение</div>
        <div class="square skew-square">Наклон</div>
    </div>

    <div class="code-template">
        <h4>Подсказка:</h4>
        <pre><code>.translate-square:hover {
    transform: translateX(100px) translateY(-20px);
}

.scale-square:hover {
    transform: scale(1.2);
}

.rotate-square:hover {
    transform: rotate(45deg);
}

.skew-square:hover {
    transform: skew(15deg, 5deg);
}</code></pre>
    </div>
</section>
```

**Стили для задания 1:**
```css
.exercise {
    background: white;
    padding: 2rem;
    border-radius: 12px;
    box-shadow: 0 8px 32px rgba(0,0,0,0.1);
}

.transform-playground {
    display: flex;
    gap: 1rem;
    flex-wrap: wrap;
    margin: 2rem 0;
}

.square {
    width: 100px;
    height: 100px;
    background: #3498db;
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 8px;
    font-weight: bold;
    transition: transform 0.3s ease;
}

/* Студенты будут добавлять стили ниже */

.translate-square:hover {
    /* Задание: переместить на 50px вправо и 20px вверх */
}

.scale-square:hover {
    /* Задание: увеличить в 1.5 раза */
}

.rotate-square:hover {
    /* Задание: повернуть на 180 градусов */
}

.skew-square:hover {
    /* Задание: наклонить на 15 градусов по X и Y */
}
```

---

## **Задание 2: "Плавные переходы" - Изучаем transition**

**Добавляем секцию:**
```html
<section class="exercise" id="exercise-2">
    <h2>⏱️ Задание 2: Плавные переходы</h2>
    
    <div class="transition-playground">
        <div class="card transition-card">
            <h3>Карточка с hover-эффектом</h3>
            <p>Наведи на меня!</p>
        </div>
        
        <button class="btn animated-btn">Анимированная кнопка</button>
        
        <div class="loading-bar">
            <div class="progress"></div>
        </div>
    </div>

    <div class="code-template">
        <h4>Подсказка для карточки:</h4>
        <pre><code>.transition-card {
    transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.transition-card:hover {
    transform: translateY(-10px) scale(1.02);
    box-shadow: 0 16px 32px rgba(0,0,0,0.15);
}</code></pre>
    </div>
</section>
```

**Стили для задания 2:**
```css
.transition-playground {
    display: flex;
    flex-direction: column;
    gap: 2rem;
    align-items: center;
}

.transition-card {
    width: 300px;
    padding: 2rem;
    background: #f8f9fa;
    border-radius: 12px;
    border: 2px solid #e9ecef;
    text-align: center;
    /* Студенты добавят transition */
}

.btn {
    padding: 1rem 2rem;
    background: #2ecc71;
    color: white;
    border: none;
    border-radius: 8px;
    font-size: 1.1rem;
    cursor: pointer;
    /* Студенты добавят transition */
}

.animated-btn:hover {
    /* Задание: изменить фон, добавить тень, немного поднять */
}

.loading-bar {
    width: 300px;
    height: 8px;
    background: #ecf0f1;
    border-radius: 4px;
    overflow: hidden;
}

.progress {
    width: 30%;
    height: 100%;
    background: #e74c3c;
    /* Задание: добавить плавное изменение ширины при hover */
}
```

---

## **Задание 3: "Сложные последовательности" - Keyframes**

**Добавляем секцию:**
```html
<section class="exercise" id="exercise-3">
    <h2>🎬 Задание 3: Keyframes анимации</h2>
    
    <div class="keyframes-playground">
        <div class="bouncing-ball">Прыгай!</div>
        <div class="pulsing-element">Пульс</div>
        <div class="slide-in-text">Появляющийся текст</div>
    </div>

    <div class="code-template">
        <h4>Пример bounce анимации:</h4>
        <pre><code>@keyframes bounce {
    0%, 100% { transform: translateY(0); }
    30% { transform: translateY(-40px); }
    50% { transform: translateY(0); }
    65% { transform: translateY(-20px); }
}

.bouncing-ball {
    animation: bounce 2s ease infinite;
}</code></pre>
    </div>
</section>
```

**Стили для задания 3:**
```css
.keyframes-playground {
    display: flex;
    gap: 2rem;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
}

.bouncing-ball {
    width: 80px;
    height: 80px;
    background: #9b59b6;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    font-weight: bold;
    /* Задание: добавить bounce анимацию */
}

.pulsing-element {
    width: 100px;
    height: 100px;
    background: #e67e22;
    border-radius: 12px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    /* Задание: добавить пульсацию */
}

.slide-in-text {
    font-size: 2rem;
    font-weight: bold;
    color: #2c3e50;
    /* Задание: добавить появление с прозрачностью и движением */
}
```

---

## **Задание 4: "Интерактивная галерея" - Комбинируем всё**

**Добавляем секцию:**
```html
<section class="exercise" id="exercise-4">
    <h2>🖼️ Задание 4: Интерактивная галерея</h2>
    
    <div class="gallery">
        <div class="gallery-item">
            <img src="https://via.placeholder.com/300x200/3498db/white" alt="Изображение 1">
            <div class="gallery-overlay">
                <h3>Заголовок</h3>
                <p>Описание карточки</p>
            </div>
        </div>
        
        <div class="gallery-item">
            <img src="https://via.placeholder.com/300x200/e74c3c/white" alt="Изображение 2">
            <div class="gallery-overlay">
                <h3>Заголовок</h3>
                <p>Описание карточки</p>
            </div>
        </div>
        
        <div class="gallery-item">
            <img src="https://via.placeholder.com/300x200/2ecc71/white" alt="Изображение 3">
            <div class="gallery-overlay">
                <h3>Заголовок</h3>
                <p>Описание карточки</p>
            </div>
        </div>
    </div>
</section>
```

**Стили для задания 4:**
```css
.gallery {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 2rem;
    margin-top: 2rem;
}

.gallery-item {
    position: relative;
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
    /* Задание: добавить transition для transform */
}

.gallery-item img {
    width: 100%;
    height: 200px;
    object-fit: cover;
    /* Задание: добавить transition для transform */
}

.gallery-overlay {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    background: rgba(0,0,0,0.8);
    color: white;
    padding: 1.5rem;
    transform: translateY(100%);
    /* Задание: добавить transition */
}

/* Задание: 
   - При hover на gallery-item: немного поднять карточку и увеличить тень
   - При hover: изображение должно немного увеличиться
   - Overlay должен плавно выезжать снизу
*/
```

---

## **Задание 5: "Микро-интеракции для кнопок"**

**Добавляем секцию:**
```html
<section class="exercise" id="exercise-5">
    <h2>🎯 Задание 5: Микро-интеракции</h2>
    
    <div class="micro-interactions">
        <button class="btn ripple-btn">Эффект волны</button>
        <button class="btn morph-btn">Преобразование</button>
        <button class="btn loading-btn">
            <span class="btn-text">Загрузка</span>
            <div class="spinner"></div>
        </button>
    </div>
</section>
```

**Стили для задания 5:**
```css
.micro-interactions {
    display: flex;
    gap: 2rem;
    flex-wrap: wrap;
    justify-content: center;
}

.ripple-btn {
    position: relative;
    overflow: hidden;
}

/* Задание: создать эффект расходящейся волны при клике */

.morph-btn {
    /* Задание: плавно менять background-color и transform при hover */
}

.loading-btn {
    position: relative;
}

.spinner {
    width: 20px;
    height: 20px;
    border: 2px solid transparent;
    border-top: 2px solid white;
    border-radius: 50%;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    opacity: 0;
    /* Задание: добавить вращающуюся анимацию */
}

.loading-btn.loading .btn-text {
    opacity: 0;
}

.loading-btn.loading .spinner {
    opacity: 1;
}
```

---

## **Финальный скрипт для интерактивности (scripts/script.js):**

```javascript
// Добавляем функциональность для кнопки загрузки
document.querySelector('.loading-btn').addEventListener('click', function() {
    this.classList.add('loading');
    
    setTimeout(() => {
        this.classList.remove('loading');
    }, 2000);
});

// Эффект ripple для кнопки
document.querySelector('.ripple-btn').addEventListener('click', function(e) {
    const ripple = document.createElement('span');
    const rect = this.getBoundingClientRect();
    const size = Math.max(rect.width, rect.height);
    const x = e.clientX - rect.left - size / 2;
    const y = e.clientY - rect.top - size / 2;
    
    ripple.style.cssText = `
        position: absolute;
        width: ${size}px;
        height: ${size}px;
        background: rgba(255,255,255,0.5);
        border-radius: 50%;
        top: ${y}px;
        left: ${x}px;
        transform: scale(0);
        animation: ripple 0.6s ease-out;
    `;
    
    this.appendChild(ripple);
    
    setTimeout(() => ripple.remove(), 600);
});

// Добавляем стили для ripple эффекта
const style = document.createElement('style');
style.textContent = `
    @keyframes ripple {
        to {
            transform: scale(4);
            opacity: 0;
        }
    }
`;
document.head.appendChild(style);
```

---

## **Чек-лист для студентов:**

- [ ] Задание 1: Все transform свойства работают при hover
- [ ] Задание 2: Плавные переходы с разными timing functions
- [ ] Задание 3: Keyframes анимации с разными эффектами
- [ ] Задание 4: Интерактивная галерея с overlay
- [ ] Задание 5: Микро-интеракции для кнопок
- [ ] Бонус: Добавить медиа-запросы для мобильной адаптации анимаций

### **Критерии оценки:**
- ✅ Плавность анимаций
- ✅ Соответствие принципам производительности
- ✅ Креативность решений
- ✅ Чистота и организация кода
- ✅ Адаптивность анимаций
