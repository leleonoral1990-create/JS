# Метод document.getElementById()

Метод `getElementById()` возвращает ссылку на первый элемент, идентификатор которого (`id`) соответствует указанной строке. Это самый быстрый и эффективный способ поиска конкретного элемента в DOM, так как идентификаторы должны быть уникальными.



---

## 1. Синтаксис

```javascript
const element = document.getElementById(id);
```

**Параметры:**
* **`id`**: Регистрозависимая строка, представляющая уникальный `id` искомого элемента.

**Возвращаемое значение:**
* Объект **`Element`**, если элемент найден.
* **`null`**, если элемента с таким идентификатором в документе нет.

---

## 2. Ключевые особенности

### Доступность только в `document`
В отличие от `querySelector` или `getElementsByClassName`, метод `getElementById` существует **только** у объекта `document`. Его нельзя вызвать на отдельном элементе (например, `div.getElementById(...)`), так как поиск всегда идет по всему дереву документа.

### Регистрозависимость (Case-sensitivity)
Метод строго учитывает регистр символов. Если в HTML прописано `id="main-nav"`, вызов `document.getElementById("Main-Nav")` вернет `null`.

### Ожидаемая уникальность
Если на странице присутствуют несколько элементов с одинаковым ID (что является нарушением стандарта HTML), метод вернет только **первый** из них.

### Производительность
Это самый производительный метод поиска. Браузеры оптимизируют доступ по ID, поэтому он работает значительно быстрее, чем `document.querySelector('#id')`.

---

## 3. Сравнение: getElementById vs querySelector

| Характеристика | `getElementById` | `querySelector` |
| :--- | :--- | :--- |
| **Селектор** | Строка ID (`'header'`) | CSS-селектор (`'#header'`) |
| **Скорость** | Максимальная | Ниже (требуется парсинг CSS) |
| **Результат** | Один элемент | Первый найденный элемент |
| **Контекст** | Только `document` | `document` или любой `element` |

---

## Примеры использования в коде

### 1. Изменение контента и стилей
```javascript
const title = document.getElementById('page-title');

if (title) {
    title.textContent = 'Добро пожаловать!';
    title.style.color = 'blue';
}
```

### 2. Получение данных из полей ввода
```javascript
function handleSubmit() {
    const emailInput = document.getElementById('user-email');
    if (emailInput) {
        console.log(`Email пользователя: ${emailInput.value}`);
    }
}
```

### 3. Распространенная ошибка (вызов на элементе)
```javascript
const section = document.getElementById('main-section');

// ❌ ОШИБКА: section.getElementById не функция
// const subItem = section.getElementById('sub-item');

// ✅ ПРАВИЛЬНО (поиск глобален):
const subItem = document.getElementById('sub-item');
```

---

Разобрать ли нам разницу между живыми коллекциями и статичными списками, которые возвращают другие методы поиска?
