# Метод Document.getElementsByClassName()

Метод `getElementsByClassName()` возвращает **живую (live)** HTML-коллекцию элементов, которые имеют все указанные имена классов. Этот метод является одним из самых быстрых способов поиска элементов в DOM.



---

## 1. Синтаксис

```javascript
const elements = document.getElementsByClassName(names);
const elements = element.getElementsByClassName(names);
```

**Параметры:**
* **`names`**: Строка, содержащая одно или несколько имен классов, разделенных пробелами.

**Возвращаемое значение:**
* **`HTMLCollection`**: Список найденных элементов. Если совпадений нет, возвращается пустая коллекция (длина 0).

---

## 2. Ключевые особенности

### Живая коллекция (Live Collection)
Результат метода автоматически синхронизируется с состоянием DOM. Если после вызова метода вы добавите в документ новый элемент с указанным классом или удалите класс у существующего, коллекция обновится мгновенно без повторного вызова метода.

### Поиск по нескольким классам
Вы можете искать элементы, которые обладают одновременно несколькими классами. Порядок имен в строке значения не имеет.

```javascript
// Выберет элементы, у которых есть И класс 'btn', И класс 'primary'
const primaryButtons = document.getElementsByClassName('btn primary');
```

### Область поиска
* **На `document`**: Поиск по всему документу.
* **На `element`**: Поиск только среди потомков этого конкретного элемента (исключая его самого).

---

## 3. HTMLCollection vs Массив

Помните, что `HTMLCollection` — это **не массив**, а массивоподобный объект.

* **Что можно**: Использовать свойство `.length`, обращаться по индексу `[0]`, перебирать через цикл `for...of`.
* **Что нельзя**: Использовать методы массивов напрямую (`.forEach()`, `.map()`, `.filter()`).
* **Как преобразовать**: Чтобы получить настоящий массив, используйте спред-оператор `[...collection]` или `Array.from(collection)`.

---

## 4. Сравнение с querySelectorAll

| Характеристика | `getElementsByClassName` | `querySelectorAll` |
| :--- | :--- | :--- |
| **Селектор** | Только классы | Любой CSS-селектор |
| **Тип результата** | `HTMLCollection` | `NodeList` |
| **Состояние** | **Живое (Live)** | Статичное (Static) |
| **Скорость** | Очень высокая | Незначительно ниже |

---

## Примеры использования в коде

### 1. Базовая итерация
```javascript
const alerts = document.getElementsByClassName('alert-danger');

for (let item of alerts) {
    item.textContent = 'Внимание: произошла ошибка!';
}
```

### 2. Использование "живой" природы коллекции
```javascript
const activeElements = document.getElementsByClassName('active');
console.log(activeElements.length); // Например, 1

// Удаляем класс у элемента
activeElements[0].classList.remove('active');

// Коллекция уменьшилась автоматически!
console.log(activeElements.length); // 0
```

### 3. Поиск внутри конкретного блока
```javascript
const sidebar = document.getElementById('sidebar');
const menuItems = sidebar.getElementsByClassName('item');

console.log(`В сайдбаре найдено элементов: ${menuItems.length}`);
```

### 4. Фильтрация (через массив)
```javascript
const cards = document.getElementsByClassName('card');

// Превращаем в массив, чтобы оставить только карточки с заголовком
const titledCards = Array.from(cards).filter(card => {
    return card.querySelector('h2') !== null;
});
```
