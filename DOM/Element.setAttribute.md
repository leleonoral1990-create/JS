# Метод document.createElementNS()

Метод **`createElementNS()`** создает элемент с указанным URI пространства имен (namespace) и квалифицированным именем (имя тега). Это критически важно при работе с XML-документами, такими как **SVG** или **MathML**, где обычный `createElement()` может работать некорректно.



---

## 1. Синтаксис

```javascript
const element = document.createElementNS(namespaceURI, qualifiedName);
```

**Параметры:**
* **`namespaceURI`**: Строка, указывающая пространство имен. Для большинства задач это предопределенные строки (см. таблицу ниже).
* **`qualifiedName`**: Строка, определяющая тип создаваемого элемента (имя тега).

**Возвращаемое значение:**
* Новый объект типа `Element`.

---

## 2. Популярные пространства имен

Браузер использует URI пространства имен, чтобы понять, какие правила рендеринга и API применять к элементу.

| Формат | URI Пространства имен (namespaceURI) |
| :--- | :--- |
| **HTML** | `http://www.w3.org/1999/xhtml` |
| **SVG** | `http://www.w3.org/2000/svg` |
| **MathML** | `http://www.w3.org/1998/Math/MathML` |

---

## 3. Почему нельзя использовать просто createElement()?

Метод `document.createElement()` по умолчанию создает элементы в пространстве имен HTML. Если вы создадите тег `<svg>` или `<circle>` через `createElement()`, браузер воспримет их как «неизвестные HTML-элементы» (по аналогии с `<my-tag>`). Они не будут отрисованы как векторная графика.

> **Правило:** Если вы создаете элементы внутри SVG или MathML динамически через JavaScript, **всегда** используйте `createElementNS()`.

---

## 4. Примеры использования

### Создание SVG-элемента
Для того чтобы нарисовать круг программно, нужно сначала создать сам контейнер `<svg>`, а затем вложенные фигуры.

```javascript
const svgNS = "http://www.w3.org/2000/svg";

// 1. Создаем контейнер SVG
const svg = document.createElementNS(svgNS, "svg");
svg.setAttribute("width", "100");
svg.setAttribute("height", "100");

// 2. Создаем круг
const circle = document.createElementNS(svgNS, "circle");
circle.setAttribute("cx", "50");
circle.setAttribute("cy", "50");
circle.setAttribute("r", "40");
circle.setAttribute("fill", "blue");

// 3. Собираем в DOM
svg.appendChild(circle);
document.body.appendChild(svg);
```

### Создание MathML элемента
Для отображения математических формул:

```javascript
const mathNS = "http://www.w3.org/1998/Math/MathML";
const msup = document.createElementNS(mathNS, "msup"); // Элемент для возведения в степень
```

---

## 5. Сравнение методов

| Характеристика | `createElement()` | `createElementNS()` |
| :--- | :--- | :--- |
| **Пространство имен** | Всегда HTML (автоматически) | Задается вручную |
| **Основное применение** | Обычные теги (div, p, span) | SVG, MathML, XML-документы |
| **Сложность** | Простой синтаксис | Требует знания URI пространства имен |

---

## Резюме
Использование `document.createElementNS()` — это признак глубокого понимания работы DOM. В современной веб-разработке этот метод чаще всего встречается при создании интерактивных графиков, кастомных иконок или сложных визуализаций данных на основе SVG. 

Нужно ли разобрать, как устанавливать атрибуты для таких элементов (ведь для них тоже есть свой метод `setAttributeNS`)?
