# Итоговый конспект: Свойство HTMLElement.style

Свойство **`style`** используется для получения или установки **инлайновых (встроенных)** стилей элемента. Оно возвращает объект `CSSStyleDeclaration`, который содержит список всех CSS-свойств, объявленных непосредственно в атрибуте `style` тега.



---

## 1. Правила именования (CamelCase)

В JavaScript свойства объекта `style` пишутся в формате **camelCase**, а не через дефис, как в CSS.

| CSS свойство | JavaScript (style) |
| :--- | :--- |
| `background-color` | `element.style.backgroundColor` |
| `margin-top` | `element.style.marginTop` |
| `z-index` | `element.style.zIndex` |
| `border-bottom-style` | `element.style.borderBottomStyle` |

---

## 2. Работа со значениями

### Чтение и запись
Значения свойств всегда являются **строками**. Даже если вы работаете с числами, необходимо добавлять единицы измерения (`px`, `%`, `em`).

```javascript
const box = document.querySelector('.box');

// Установка значения (обязательно строка с единицами измерения)
box.style.width = '100px';
box.style.opacity = '0.5';

// Чтение значения
console.log(box.style.width); // "100px"
```

### Удаление стиля
Чтобы удалить конкретное инлайновое свойство, ему нужно присвоить пустую строку `""`. Элемент вернется к стилям, заданным в CSS-файле.

```javascript
box.style.display = ""; // Удаляет инлайновый display
```

---

## 3. Свойство cssText

Если нужно установить сразу много стилей одной строкой, удобнее использовать свойство **`cssText`**.

> **Важно:** Запись в `cssText` полностью **перезаписывает** текущий атрибут `style`.

```javascript
box.style.cssText = "color: red; background-color: yellow; width: 200px;";
```

---

## 4. Сравнение: style vs getComputedStyle()

Это самое частое место для ошибок. Объект `style` видит **только** то, что написано в атрибуте `style` самого тега.

| Характеристика | `element.style` | `window.getComputedStyle(el)` |
| :--- | :--- | :--- |
| **Что видит** | Только инлайновые стили | Итоговые стили (из CSS-файлов, браузера и т.д.) |
| **Чтение/Запись** | Можно и читать, и записывать | **Только для чтения** |
| **Единицы** | Как записано (`10rem`, `50%`) | Всегда в пикселях (`px`) |



---

## 5. Примеры использования в коде

### Сценарий 1: Динамическое изменение размеров при прокрутке
```javascript
window.addEventListener('scroll', () => {
  const scrolled = window.scrollY;
  const header = document.querySelector('header');
  
  // Уменьшаем прозрачность в зависимости от скролла
  header.style.opacity = Math.max(0, 1 - scrolled / 500);
});
```

### Сценарий 2: Переключатель темы (Dark Mode)
```javascript
const toggleBtn = document.querySelector('#theme-toggle');

toggleBtn.addEventListener('click', () => {
  const isDark = document.body.style.backgroundColor === 'black';
  
  if (isDark) {
    document.body.style.backgroundColor = 'white';
    document.body.style.color = 'black';
  } else {
    // Устанавливаем стили напрямую
    document.body.style.backgroundColor = 'black';
    document.body.style.color = 'white';
  }
});
```

### Сценарий 3: Безопасная работа с префиксами
Для некоторых свойств (например, `webkit`) можно использовать строковый доступ:
```javascript
const el = document.body;
el.style['webkitTransform'] = 'rotate(45deg)';
```

---

### Резюме
Используйте `element.style` для быстрых динамических изменений (анимации, JS-логика отображения). Для глобального изменения оформления лучше манипулировать классами через `classList`, оставляя описание самих стилей в CSS-файлах.
