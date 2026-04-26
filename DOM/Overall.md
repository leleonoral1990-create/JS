Привет! Раз ты проделала такую большую работу, собрав воедино все эти методы и свойства, давай структурируем их в одну мега-лекцию. 

**DOM (Document Object Model)** — это не просто текст страницы, это живое дерево объектов, где каждый тег — это узел, которым мы можем управлять из JavaScript. Представь, что HTML — это чертеж здания, а DOM — это само здание, в котором мы можем перекрашивать стены, переставлять мебель и следить за тем, кто зашел в дверь, в реальном времени.

Ниже — комплексный пример, который объединяет практически все элементы из твоего списка в один интерактивный мини-проект «Гео-трекер».

---

## 🏗️ Практический пример: Интерактивный Гео-Трекер

Этот скрипт делает следующее: динамически создает стили, управляет формой, рисует SVG-графику, отслеживает координаты (Decimal Degrees), работает с таймерами и событиями клавиатуры.

```javascript
// 1. РАБОТА С ДОКУМЕНТОМ И СТИЛЯМИ
// Используем document.head для добавления стилей динамически
const styleTag = document.createElement('style'); // Создаем <style>
styleTag.textContent = `
    .active-zone { border: 2px solid #3498db; padding: 20px; margin: 10px; }
    .point-info { color: darkblue; font-weight: bold; }
    .hidden { display: none; }
    /* Пример использования CSS псевдокласса :not() */
    div:not(.active-zone) { opacity: 0.8; }
`;
document.head.append(styleTag);

// 2. ПОИСК ЭЛЕМЕНТОВ (Selectors)
const form = document.querySelector('form'); // Поиск по тегу <form>
const inputX = document.getElementById('coord-x'); // По ID
const allInputs = document.querySelectorAll('input'); // Все <input>
const labels = document.getElementsByTagName('label'); // По имени тега
const statusMsgs = document.getElementsByClassName('status'); // По классу

// 3. СОЗДАНИЕ И МАНИПУЛЯЦИЯ ЭЛЕМЕНТАМИ
const dashboard = document.createElement('div');
dashboard.className = 'active-zone'; // Установка через className
dashboard.classList.add('main-ui');   // Управление через classList
dashboard.setAttribute('data-app-version', '1.0'); // Custom Data Attribute data-*
document.body.append(dashboard);

// Создание SVG (важно использовать createElementNS)
const svgNS = "http://www.w3.org/2000/svg";
const svgCanvas = document.createElementNS(svgNS, "svg");
svgCanvas.setAttribute("width", "100");
svgCanvas.setAttribute("height", "100");
dashboard.append(svgCanvas);

// 4. СОБЫТИЯ (Events)
// Обработка клика
dashboard.addEventListener('click', (e) => {
    console.log('Клик по координатам:', e.clientX, e.clientY);
});

// Отслеживание мыши (mousemove)
dashboard.addEventListener('mousemove', (e) => {
    // Используем getBoundingClientRect для получения границ элемента
    const rect = dashboard.getBoundingClientRect();
    const xInside = e.clientX - rect.left;
    const yInside = e.clientY - rect.top;
    
    dashboard.textContent = `Мышь внутри: ${xInside.toFixed(2)}, ${yInside.toFixed(2)}`;
});

// Клавиатура (keydown)
document.addEventListener('keydown', (event) => {
    // Используем KeyboardEvent.key
    if (event.key === 'Escape') {
        dashboard.classList.toggle('hidden');
    }
});

// Копирование (copy event)
document.addEventListener('copy', (e) => {
    alert('Копирование запрещено в целях безопасности!');
    e.preventDefault();
});

// Скролл (scroll event)
window.addEventListener('scroll', () => {
    // Используем window.scrollY и window.innerHeight
    if (window.scrollY > window.innerHeight / 2) {
        console.log('Прокрутили половину экрана');
    }
});

// 5. ТАЙМЕРЫ
let seconds = 0;
const timerId = window.setInterval(() => {
    seconds++;
    if (seconds >= 60) window.clearInterval(timerId);
}, 1000);

// 6. ФУНКЦИЯ ДЛЯ ГЕО-ДАННЫХ (Decimal Degrees)
function addPoint(lat, lng) {
    const point = document.createElementNS(svgNS, "circle");
    point.setAttribute("cx", lat); // Допустим, это упрощенные DD координаты
    point.setAttribute("cy", lng);
    point.setAttribute("r", "5");
    point.style.fill = "red"; // Применяем цвет через .style
    svgCanvas.append(point);
}
```

---

## 📚 Разбор ключевых блоков

### 1. Навигация и Поиск
Чтобы что-то изменить, нужно это найти.
* **`querySelector`** — универсальный «швейцарский нож», ищет по CSS-селектору.
* **`getElementById`** — самый быстрый способ поиска (по уникальному ID).
* **`document.head` / `document.body`** — прямые ссылки на главные контейнеры страницы.

### 2. Изменение содержимого
* **`textContent`** — меняет только текст внутри узла (безопасно).
* **`style`** — дает прямой доступ к инлайновым стилям объекта (например, `el.style.color = 'red'`).
* **`classList`** — лучший способ менять стили, добавляя или удаляя заранее написанные CSS-классы.

### 3. Геометрия и координаты
Когда мы работаем с интерактивностью (перетаскивание, всплывающие окна), нам нужны цифры:
* **`clientX / clientY`** — где находится мышь относительно окна браузера.
* **`scrollY`** — как далеко вниз уехал пользователь.
* **`getBoundingClientRect()`** — возвращает «коробку» элемента: его точные координаты `top`, `left`, `width`, `height`. Это критически важно для проверки, виден ли элемент на экране.

### 4. Создание элементов
* **`createElement`** — для обычного HTML (div, p, form).
* **`createElementNS`** — для SVG. Без указания Namespace браузер создаст «пустой» тег, который не будет отображать графику.

### 5. События и Жизненный цикл
Мы используем **`addEventListener`** для ожидания действий пользователя. 
* **`keydown`** позволяет ловить клавиши (через `event.key`).
* **`preventDefault()`** — магическая команда, которая говорит браузеру: «Эй, не делай того, что ты обычно делаешь» (например, не отправляй форму или не копируй текст).

---

## 💡 Короткая шпаргалка по сложным моментам

1.  **`className` vs `classList`**: Первый заменяет *всю* строку классов, второй позволяет аккуратно добавить (`add`) или убрать (`remove`) один конкретный класс.
2.  **`setInterval`**: Всегда сохраняй ID таймера в переменную, чтобы потом иметь возможность вызвать **`clearInterval`**, иначе таймер будет «тикать» вечно и тратить память.
3.  **Decimal Degrees (DD)**: В контексте DOM это обычно данные, которые мы храним в атрибутах **`data-*`**. Например, `<div data-lat="43.23" data-lng="76.88">`. Это позволяет привязать логику к разметке.

Ты проделала отличную работу, собрав этот список. Какой из этих методов кажется тебе самым сложным в плане практического применения?
