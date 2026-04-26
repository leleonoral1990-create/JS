# Метод Document.getElementsByTagName()

Метод `getElementsByTagName()` возвращает **живую (live)** HTML-коллекцию элементов с указанным именем тега. Поиск ведется по всему документу, включая корневой элемент.

---

## 1. Синтаксис

```javascript
const elements = document.getElementsByTagName(name);
const elements = element.getElementsByTagName(name);
```

**Параметры:**
* **`name`**: Строка, представляющая имя тега (например, `'div'`, `'li'`). Специальное значение `'*'` возвращает все элементы документа.

**Возвращаемое значение:**
* **`HTMLCollection`**: Список найденных элементов в том порядке, в котором они появляются в дереве.

---

## 2. Ключевые особенности

### Живая коллекция (Live Collection)
В отличие от `querySelectorAll`, результат `getElementsByTagName` автоматически обновляется при изменении DOM. Если вы добавите новый тег в документ после вызова метода, он сразу появится в ранее созданной коллекции.



### Контекст поиска
* При вызове на **`document`**: поиск идет по всей странице.
* При вызове на конкретном **`element`**: поиск идет только среди потомков этого элемента (исключая его самого).

### Регистронезависимость
В HTML-документах поиск имен тегов не чувствителен к регистру: `'div'` и `'DIV'` вернут один и тот же результат.

---

## 3. HTMLCollection vs Массив

`HTMLCollection` — это массивоподобный объект, но не массив.
* **Что можно:** обращаться по индексу (`coll[0]`), использовать свойство `.length`, перебирать через `for...of`.
* **Что нельзя:** использовать методы массивов (`.map()`, `.filter()`, `.forEach()`) напрямую. 
* **Как преобразовать в массив:** `Array.from(collection)` или `[...collection]`.

---

## 4. Сравнение: getElementsByTagName vs querySelectorAll

| Характеристика | `getElementsByTagName` | `querySelectorAll` |
| :--- | :--- | :--- |
| **Тип результата** | `HTMLCollection` | `NodeList` |
| **Состояние** | **Живое (Live)** | Статичное (Static) |
| **Гибкость** | Только имя тега | Любой CSS-селектор |
| **Производительность** | Быстрее (специализированный) | Медленнее (универсальный) |

---

## Примеры использования в коде

### 1. Базовый поиск и итерация
```javascript
// Находим все параграфы
const paragraphs = document.getElementsByTagName('p');

console.log(`Найдено параграфов: ${paragraphs.length}`);

// Перебор через for...of
for (let p of paragraphs) {
    p.style.color = 'blue';
}
```

### 2. Поиск внутри элемента
```javascript
const list = document.getElementById('myList');
const items = list.getElementsByTagName('li'); // Только <li> внутри этого списка

console.log(items.length);
```

### 3. Использование "живой" природы коллекции
```javascript
const divs = document.getElementsByTagName('div');
console.log(divs.length); // Например, 2

// Добавляем новый div в body
const newDiv = document.createElement('div');
document.body.appendChild(newDiv);

// Коллекция обновилась сама!
console.log(divs.length); // Теперь 3
```

### 4. Использование универсального селектора
```javascript
// Получить вообще все элементы на странице
const allElements = document.getElementsByTagName('*');
console.log(allElements.length);
```
