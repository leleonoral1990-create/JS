# Интерфейс Element (Web API)

Интерфейс **Element** — это базовый класс для всех объектов, представляющих теги в DOM. Он наследует свойства `Node`, но расширяет их специфическими методами для работы с атрибутами, классами и геометрией.

---

## 1. Element vs Node: Фундаментальное различие

* **Node (Узел)**: Базовый тип. Включает текстовые узлы, комментарии и переносы строк.
* **Element (Элемент)**: Строго HTML-тег (`<div>`, `<span>` и т.д.).

| Свойство навигации | Тип узла | Результат |
| :--- | :--- | :--- |
| `childNodes` | **Node** | Все узлы (включая текст и комментарии) |
| `children` | **Element** | Только вложенные теги |
| `parentNode` | **Node** | Любой родительский узел |
| `parentElement` | **Element** | Родитель, если он является тегом |

---

## 2. Работа с содержимым и классами

### Свойства содержимого
* **`innerHTML`**: HTML-код внутри элемента (строка). Позволяет динамически создавать вложенную структуру.
* **`textContent`**: Весь текст внутри элемента (без тегов). Безопасен для вставки пользовательских данных.
* **`outerHTML`**: Элемент целиком (включая его собственные теги).

### Управление классами (`classList`)
Объект `classList` — современный способ манипуляции CSS-классами:
* `add('class')` — добавить.
* `remove('class')` — удалить.
* `toggle('class')` — переключить (удалить, если есть; добавить, если нет).
* `contains('class')` — вернуть `true/false`.

---

## 3. Атрибуты и Dataset

### Методы атрибутов
Позволяют работать с любыми атрибутами тега напрямую.
* `getAttribute(name)` — получить значение.
* `setAttribute(name, value)` — установить.
* `hasAttribute(name)` — проверить наличие.
* `removeAttribute(name)` — удалить.

### Свойство `dataset`
Специальное свойство для работы с атрибутами вида `data-*`.
* HTML: `<div data-user-id="5" data-role="admin"></div>`
* JS: `el.dataset.userId` (автоматическое преобразование из kebab-case в camelCase).

---

## 4. Продвинутая навигация и Поиск

Методы поиска, доступные для каждого элемента:
* **`querySelector(selector)`**: Первый дочерний элемент, подходящий под CSS-селектор.
* **`querySelectorAll(selector)`**: Статичная коллекция всех подходящих дочерних элементов.
* **`closest(selector)`**: Поиск ближайшего родителя (включая сам элемент) вверх по дереву.
* **`matches(selector)`**: Проверка, соответствует ли элемент селектору.

---

## 5. Геометрия и Координаты



* **`getBoundingClientRect()`**: Возвращает объект с координатами (`top`, `left`, `right`, `bottom`) и размерами относительно вьюпорта (окна браузера).
* **`clientHeight` / `clientWidth`**: Видимая область (контент + padding, без border и scrollbar).
* **`scrollHeight` / `scrollWidth`**: Полный размер контента, включая скрытый за прокруткой.
* **`scrollTop` / `scrollLeft`**: Текущая позиция прокрутки элемента (доступно для чтения и записи).

---

## 6. Манипуляция DOM

Современные методы для вставки и удаления (работают со строками и узлами):
* `append(...nodes)` — в конец элемента.
* `prepend(...nodes)` — в начало элемента.
* `before(...nodes)` — перед элементом.
* `after(...nodes)` — после элемента.
* `remove()` — полное удаление элемента из DOM.

---

## Практический пример использования

Сценарий: обработка интерактивного элемента (карточки) с использованием всех возможностей `Element`.

```javascript
// 1. Поиск элемента
const card = document.querySelector('.js-product-card');

if (card) {
  // 2. Работа с классами и атрибутами
  card.classList.add('is-initialized');
  const productId = card.getAttribute('id');
  const price = card.dataset.productPrice;

  // 3. Динамическое изменение содержимого
  const title = card.querySelector('.title');
  title.textContent = "Обновленное название"; // Безопасно

  // 4. Добавление нового элемента
  const badge = document.createElement('div');
  badge.className = 'badge';
  badge.innerHTML = '<strong>SALE</strong>';
  card.prepend(badge);

  // 5. Проверка геометрии
  const rect = card.getBoundingClientRect();
  console.log(`Позиция карточки: ${rect.top}px от верха окна`);
}

// 6. Делегирование через closest
document.addEventListener('click', (e) => {
  const btn = e.target.closest('.delete-btn');
  if (btn) {
    const targetCard = btn.closest('.js-product-card');
    targetCard?.remove(); // Удаление элемента
  }
});
```
