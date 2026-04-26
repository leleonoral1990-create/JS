# Итоговый конспект: Свойство HTMLElement.dataset

Свойство **`dataset`** предоставляет удобный интерфейс для чтения и записи пользовательских дата-атрибутов (`data-*`) на элементах. Все атрибуты, имя которых начинается с `data-`, доступны через это свойство как объект-карта (DOMStringMap).



---

## 1. Логика преобразования имен (Naming Convention)

Это самый важный нюанс работы с `dataset`. Имена преобразуются из формата HTML (kebab-case) в формат JavaScript (camelCase) и обратно по следующим правилам:

1.  Удаляется префикс `data-`.
2.  Любой дефис (`-`), за которым следует буква латинского алфавита, удаляется, а буква становится заглавной.

| HTML Атрибут | JavaScript Свойство |
| :--- | :--- |
| `data-id` | `el.dataset.id` |
| **`data-user-id`** | **`el.dataset.userId`** |
| `data-v-mem-info` | `el.dataset.vMemInfo` |

---

## 2. Чтение и Запись данных

### Чтение
Значения всегда возвращаются в виде **строки**. Если атрибута нет, результатом будет `undefined`.

```javascript
const user = document.querySelector('#user-profile');
console.log(user.dataset.userId); // "123" (строка)
```

### Запись
При установке значения оно автоматически приводится к строке.

```javascript
user.dataset.status = 'active'; 
// В HTML это добавит: data-status="active"

user.dataset.score = 500; 
// В HTML это станет строкой: data-score="500"
```

### Удаление
Чтобы полностью удалить атрибут из HTML, используется оператор `delete`.

```javascript
delete user.dataset.status; 
// Атрибут data-status исчезнет из тега
```

---

## 3. Сравнение: dataset vs getAttribute

| Характеристика | `dataset` | `getAttribute('data-...')` |
| :--- | :--- | :--- |
| **Синтаксис** | Объектный (`el.dataset.info`) | Методы (`el.getAttribute('data-info')`) |
| **Имена** | **camelCase** | Исходный kebab-case |
| **Производительность** | Чуть медленнее (создается объект) | Чуть быстрее (прямой доступ) |
| **Удобство** | Высокое для частых операций | Низкое (нужно писать длинные строки) |

---

## 4. Важные нюансы

* **Только строки:** Даже если вы запишете в `dataset` число или объект, в HTML это сохранится как строка (`"[object Object]"`). Для сложных данных используйте `JSON.stringify()`.
* **Регистр в HTML:** Атрибуты в HTML регистронезависимы. Но `dataset` чувствителен к регистру ключей в JavaScript.
* **Символы в именах:** Не используйте в именах дата-атрибутов ничего, кроме латинских букв и дефисов, иначе преобразование в camelCase может работать непредсказуемо.

---

## 🚀 Практический пример использования

Сценарий: динамическое управление состоянием фильтров в каталоге.

```javascript
// HTML: <button class="filter" data-category="electronics" data-min-price="1000">

const filterBtn = document.querySelector('.filter');

// 1. Извлекаем данные
const category = filterBtn.dataset.category;
const minPrice = Number(filterBtn.dataset.minPrice); // Приводим к числу

// 2. Обновляем данные при клике
filterBtn.addEventListener('click', () => {
  // Переключаем статус
  if (filterBtn.dataset.active === 'true') {
    filterBtn.dataset.active = 'false';
    filterBtn.classList.remove('is-active');
  } else {
    filterBtn.dataset.active = 'true';
    filterBtn.classList.add('is-active');
  }
  
  console.log(`Фильтр по ${category} обновлен.`);
});
```
