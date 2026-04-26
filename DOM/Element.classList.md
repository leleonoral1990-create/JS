# Итоговый конспект: Свойство Element.classList

Свойство **`classList`** возвращает живую коллекцию **`DOMTokenList`**, содержащую все CSS-классы элемента. Это наиболее удобный и современный способ манипулирования классами без необходимости парсить строку `className` вручную.



---

## 1. Основные методы

| Метод | Описание | Пример |
| :--- | :--- | :--- |
| **`add(...class)`** | Добавляет один или несколько классов. | `el.classList.add('a', 'b')` |
| **`remove(...class)`** | Удаляет один или несколько классов. | `el.classList.remove('old')` |
| **`toggle(class, bool)`** | Переключает класс (удаляет, если есть; добавляет, если нет). | `el.classList.toggle('active')` |
| **`contains(class)`** | Проверяет наличие класса (возвращает `true` или `false`). | `el.classList.contains('hidden')` |
| **`replace(old, new)`** | Заменяет существующий класс на новый. | `el.classList.replace('off', 'on')` |

---

## 2. Ключевые особенности

1.  **Тип данных**: `classList` — это не массив, а **`DOMTokenList`**. Он итерируемый (можно использовать `forEach` или `for...of`), но не имеет методов вроде `map` или `filter`. Чтобы превратить его в массив, используйте `[...el.classList]`.
2.  **Свойство `length`**: Позволяет узнать количество классов у элемента.
3.  **Свойство `value`**: Возвращает список классов в виде обычной строки (аналог `className`).
4.  **Безопасность**: Методы игнорируют дубликаты при добавлении и не вызывают ошибок, если вы пытаетесь удалить несуществующий класс.

---

## 3. Сравнение: classList vs className

| Характеристика | `classList` | `className` |
| :--- | :--- | :--- |
| **Формат** | Массивоподобный объект | Строка (`"btn btn-primary"`) |
| **Удобство** | Высокое (встроенные методы) | Низкое (требует `split` / `join`) |
| **Риск** | Безопасно изменяет один класс | Можно случайно перезаписать все классы |
| **Производительность** | Оптимизировано для точечных изменений | Быстрее, если нужно заменить всё сразу |

---

## 4. Примеры использования в коде

### 1. Продвинутый toggle
Метод `toggle` принимает второй необязательный аргумент — логическое условие. Если оно `true`, класс добавится, если `false` — удалится.

```javascript
const themeBtn = document.querySelector('.theme-switcher');

// Класс 'dark-mode' добавится только если isDarkProperty === true
themeBtn.classList.toggle('dark-mode', window.matchMedia('(prefers-color-scheme: dark)').matches);
```

### 2. Массовое добавление и удаление
```javascript
const modal = document.querySelector('.modal');

// Можно передавать список аргументов
modal.classList.add('visible', 'fade-in', 'highlight');

// И так же массово удалять
modal.classList.remove('hidden', 'loading');
```

### 3. Проверка и замена
```javascript
const statusIcon = document.querySelector('.status-icon');

if (statusIcon.classList.contains('is-error')) {
  // Заменяем класс ошибки на класс успеха
  statusIcon.classList.replace('is-error', 'is-success');
}
```

### 4. Итерация по классам
```javascript
const box = document.querySelector('.box');

box.classList.forEach(className => {
  console.log(`У элемента есть класс: ${className}`);
});
```

---

## Резюме
Используйте `classList` для любых манипуляций с классами в JS-логике. Это предотвращает ошибки при работе со строками и делает код декларативным. К `className` стоит обращаться только в тех редких случаях, когда вам нужно полностью «обнулить» список классов или заменить его целиком одной операцией.
