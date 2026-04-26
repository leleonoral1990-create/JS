# URLSearchParams

Интерфейс **`URLSearchParams`** предоставляет методы для работы со строкой запроса (query string) URL-адреса. Он позволяет легко считывать, добавлять и изменять параметры без ручного парсинга строки.

---

## 1. Конструктор

```javascript
// Из строки
const params = new URLSearchParams('?id=123&name=test');

// Из текущего URL
const params = new URLSearchParams(window.location.search);

// Из объекта
const params = new URLSearchParams({ category: 'books', page: 1 });
```

---

## 2. Основные методы

| Метод | Описание |
| :--- | :--- |
| **`get(name)`** | Возвращает первое значение по имени параметра. |
| **`getAll(name)`** | Возвращает массив всех значений (если ключ дублируется). |
| **`has(name)`** | Проверяет наличие параметра (возвращает `true/false`). |
| **`set(name, value)`** | Устанавливает значение (заменяет все существующие с этим именем). |
| **`append(name, value)`** | Добавляет новое значение, не удаляя существующие. |
| **`delete(name)`** | Удаляет параметр. |
| **`toString()`** | Возвращает параметры в виде строки для использования в URL. |
| **`sort()`** | Сортирует все пары ключ/значение по именам. |

---

## 3. Примеры использования

### Чтение параметров
```javascript
const urlParams = new URLSearchParams('?type=article&tag=js&tag=web');

console.log(urlParams.get('type')); // "article"
console.log(urlParams.getAll('tag')); // ["js", "web"]
```

### Модификация и генерация строки
```javascript
const params = new URLSearchParams();
params.set('sort', 'price');
params.append('filter', 'active');
params.append('filter', 'new');

console.log(params.toString()); // "sort=price&filter=active&filter=new"
```

### Преобразование в объект
Используется в связке с `Object.fromEntries()`:
```javascript
const params = new URLSearchParams('?user=admin&status=active');
const obj = Object.fromEntries(params);

console.log(obj); // { user: "admin", status: "active" }
```

---

## 4. Особенности
* **Автоматическое кодирование:** Метод `toString()` автоматически применяет `encodeURIComponent` к ключам и значениям (например, пробелы превращаются в `+` или `%20`).
* **Итерируемость:** Объект можно перебирать циклами `for...of`.
* **Порядок:** Параметры сохраняются в том порядке, в котором они были добавлены.
