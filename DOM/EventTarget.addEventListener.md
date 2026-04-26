# Метод EventTarget.addEventListener()

Метод **`addEventListener()`** регистрирует обработчик события для определённого типа события на объекте. Он может быть вызван на любом объекте, поддерживающем события (элементы DOM, `document`, `window`, `XMLHttpRequest` и др.).



---

## 1. Синтаксис

```javascript
target.addEventListener(type, listener);
target.addEventListener(type, listener, options);
target.addEventListener(type, listener, useCapture);
```

**Параметры:**
* **`type`**: Строка, чувствительная к регистру, представляющая тип события (например, `'click'`, `'keydown'`).
* **`listener`**: Функция (callback) или объект с методом `handleEvent`, который будет вызван при наступлении события.
* **`options`** (необязательно): Объект с настройками обработчика.
* **`useCapture`** (необязательно): Логическое значение. Если `true`, обработчик сработает на фазе погружения (capture). По умолчанию — `false` (фаза всплытия).

---

## 2. Настройки (Параметр options)

| Свойство | Тип | Описание |
| :--- | :--- | :--- |
| **`capture`** | Boolean | Если `true`, обработчик запускается на фазе погружения. |
| **`once`** | Boolean | Если `true`, обработчик автоматически удалится после первого выполнения. |
| **`passive`** | Boolean | Если `true`, обработчик никогда не вызовет `preventDefault()`. Ускоряет прокрутку. |
| **`signal`** | AbortSignal | Позволяет удалить обработчик с помощью `AbortController.abort()`. |

---

## 3. Ключевые особенности

### Множественные обработчики
В отличие от свойств вида `onclick`, `addEventListener` позволяет назначать несколько обработчиков на одно и то же событие одного элемента.

```javascript
el.addEventListener('click', func1);
el.addEventListener('click', func2); // Оба сработают
```

### Значение `this`
Внутри функции-обработчика `this` обычно ссылается на элемент, на котором сработал обработчик (то же самое, что `event.currentTarget`). 
*Примечание:* У стрелочных функций `this` определяется контекстом их создания.

### Анонимные функции
Если вы используете анонимную функцию в качестве `listener`, вы не сможете удалить этот обработчик позже через `removeEventListener`.

---

## 4. Сравнение: addEventListener vs onEvent

| Характеристика | `addEventListener` | `element.onclick = ...` |
| :--- | :--- | :--- |
| **Кол-во обработчиков** | Неограниченно | Только один (перезаписывается) |
| **Фазы события** | Погружение и Всплытие | Только Всплытие |
| **Удаление** | Через `removeEventListener` | Присвоением `null` |
| **Параметры** | Есть (once, passive и др.) | Нет |

---

## 5. Примеры использования

### 1. Базовый клик с параметром `once`
```javascript
const btn = document.querySelector('button');

btn.addEventListener('click', () => {
    console.log('Эта кнопка сработает только один раз');
}, { once: true });
```

### 2. Использование AbortController для удаления
Современный способ массового удаления обработчиков.

```javascript
const controller = new AbortController();

window.addEventListener('resize', () => {
    console.log('Изменение размера...');
}, { signal: controller.signal });

// Позже удаляем обработчик
controller.abort();
```

### 3. Обработка на фазе погружения (Capture)
```javascript
document.querySelector('.parent').addEventListener('click', () => {
    console.log('Сначала я (погружение)');
}, true);

document.querySelector('.child').addEventListener('click', () => {
    console.log('Потом я (всплытие)');
});
```

---

## Резюме
Метод `addEventListener` — стандарт для работы с событиями в современном JS. Он обеспечивает гибкость, поддержку множества слушателей и тонкую настройку через объект `options`, что делает код более чистым и производительным (особенно с `passive: true`).
