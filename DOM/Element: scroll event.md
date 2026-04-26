# Событие scroll

Событие **`scroll`** происходит, когда пользователь прокручивает содержимое элемента или всего документа. Это одно из самых «активных» событий в вебе, так как оно может генерироваться десятки раз в секунду при быстром перемещении полосы прокрутки.



---

## 1. Справочная информация

| Характеристика | Значение |
| :--- | :--- |
| **Всплывает** | **Нет** (на элементах), но всплывает от `document` к `window` |
| **Отменяемое** | **Нет** (нельзя предотвратить прокрутку через `preventDefault`) |
| **Интерфейс** | `Event` (не содержит специфических данных о координатах) |
| **Цель (Target)** | Элемент со свойством `overflow: scroll` (или `auto`), `document` или `window` |

---

## 2. Как получить позицию прокрутки

Поскольку объект события `scroll` сам по себе не сообщает, «где именно» мы находимся, координаты нужно запрашивать у целевого элемента:

* **Для обычного элемента**: Используйте свойства **`scrollTop`** (вертикаль) и **`scrollLeft`** (горизонталь).
* **Для всего окна (window)**: Используйте **`window.scrollY`** и **`window.scrollX`**.

```javascript
// Текущая позиция прокрутки элемента
const top = element.scrollTop;
const left = element.scrollLeft;

// Текущая позиция прокрутки страницы
const pageTop = window.scrollY;
```

---

## 3. Производительность и Оптимизация

Событие `scroll` генерируется очень часто. Если выполнять внутри обработчика тяжелые вычисления или изменять DOM, страница начнет «тормозить» (эффект джиттера или лага).

### Рекомендация 1: Throttling (Ограничение частоты)
Используйте `requestAnimationFrame`, чтобы синхронизировать выполнение кода с частотой обновления экрана.

```javascript
let isScrolling = false;

window.addEventListener('scroll', () => {
  if (!isScrolling) {
    window.requestAnimationFrame(() => {
      // Ваша логика здесь
      console.log('Позиция:', window.scrollY);
      isScrolling = false;
    });
    isScrolling = true;
  }
});
```

### Рекомендация 2: Пассивные слушатели
Всегда добавляйте `{ passive: true }`, если вы не планируете (а вы и не можете в `scroll`) отменять действие. Это помогает браузеру оптимизировать прокрутку, особенно на мобильных устройствах.

```javascript
window.addEventListener('scroll', handler, { passive: true });
```

---

## 4. Сравнение: scroll vs wheel

| Событие | Когда срабатывает | Можно ли отменить? |
| :--- | :--- | :--- |
| **`scroll`** | Когда позиция прокрутки **уже изменилась**. | **Нет** |
| **`wheel`** | Когда пользователь крутит колесико (даже если скролла нет). | **Да** (через `preventDefault`) |

---

## 5. Практические примеры

### 1. Прогресс-бар чтения
Визуальная полоска вверху страницы, которая заполняется по мере чтения.

```javascript
window.addEventListener('scroll', () => {
  const winScroll = document.body.scrollTop || document.documentElement.scrollTop;
  const height = document.documentElement.scrollHeight - document.documentElement.clientHeight;
  const scrolled = (winScroll / height) * 100;
  
  document.getElementById("myBar").style.width = scrolled + "%";
});
```

### 2. Кнопка «Наверх»
Появление кнопки только после того, как пользователь прокрутил страницу вниз на 300 пикселей.

```javascript
const topBtn = document.querySelector('#go-top');

window.addEventListener('scroll', () => {
  if (window.scrollY > 300) {
    topBtn.classList.add('is-visible');
  } else {
    topBtn.classList.remove('is-visible');
  }
});
```

### 3. Бесконечная прокрутка (Infinite Scroll)
```javascript
window.addEventListener('scroll', () => {
  const { scrollTop, scrollHeight, clientHeight } = document.documentElement;
  
  // Если до конца страницы осталось меньше 100px
  if (scrollTop + clientHeight >= scrollHeight - 100) {
    loadMoreContent();
  }
}, { passive: true });
```

---

## Современная альтернатива: Intersection Observer API
Для многих задач (например, запуск анимации при появлении элемента или ленивая загрузка изображений) сейчас лучше использовать **Intersection Observer**. Он гораздо эффективнее, так как не требует постоянного отслеживания события `scroll` и работает в отдельном потоке.

Хотите взглянуть, как переписать логику появления элементов со `scroll` на `Intersection Observer`, чтобы сэкономить ресурсы процессора?
