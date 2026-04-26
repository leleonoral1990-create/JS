# Свойство window.scrollY

Свойство **`window.scrollY`** интерфейса `Window` возвращает количество пикселей, на которое документ в данный момент прокручен по вертикали. Это значение указывает, насколько «далеко» верхний край видимой области (вьюпорта) ушел вниз от начала страницы.



---

## 1. Синтаксис

```javascript
let y = window.scrollY;
```

**Возвращаемое значение:**
* **Число (`double`)**: Количество пикселей.
* В современных браузерах значение может быть дробным (например, `150.5`), особенно на экранах с высокой плотностью пикселей (Retina).
* Если прокрутки нет (вы в самом верху), значение равно `0`.

---

## 2. scrollY vs pageYOffset vs scrollTop

В веб-разработке есть несколько способов узнать позицию прокрутки. Вот в чем их разница:

| Свойство | Объект | Описание |
| :--- | :--- | :--- |
| **`window.scrollY`** | `window` | Современный стандарт для прокрутки окна. |
| **`window.pageYOffset`** | `window` | Старый псевдоним `scrollY`. Полностью идентичен, используется для обратной совместимости. |
| **`element.scrollTop`** | `Element` | Используется для прокрутки **внутри** конкретного блока (например, `div` со скроллом). |

---

## 3. Практическое применение

### 1. Кнопка «Наверх»
Показываем кнопку только тогда, когда пользователь прокрутил страницу достаточно глубоко.

```javascript
const scrollButton = document.querySelector('#scrollToTop');

window.addEventListener('scroll', () => {
  if (window.scrollY > 500) {
    scrollButton.style.display = 'block';
  } else {
    scrollButton.style.display = 'none';
  }
});
```

### 2. Создание «липкого» (Sticky) меню
Добавление класса при достижении определенной точки.

```javascript
const nav = document.querySelector('nav');
const navTop = nav.offsetTop;

window.addEventListener('scroll', () => {
  if (window.scrollY >= navTop) {
    nav.classList.add('fixed-nav');
  } else {
    nav.classList.remove('fixed-nav');
  }
});
```

### 3. Эффект параллакса
Смещение фонового изображения в зависимости от скорости прокрутки.

```javascript
window.addEventListener('scroll', () => {
  const scrolled = window.scrollY;
  const background = document.querySelector('.hero-bg');
  background.style.transform = `translateY(${scrolled * 0.5}px)`;
});
```

---

## 4. Важные нюансы

1.  **Только чтение**: Вы не можете изменить прокрутку, написав `window.scrollY = 500`. Для этого используйте методы `window.scrollTo()` или `window.scrollBy()`.
2.  **Производительность**: Чтение `scrollY` внутри события `scroll` может быть ресурсозатратным, если делать это слишком часто. Рекомендуется использовать `requestAnimationFrame` или ограничение частоты выполнения (throttling).
3.  **Кроссбраузерность**: В очень старых версиях Internet Explorer `window.scrollY` не поддерживалось. В таких случаях использовали `document.documentElement.scrollTop`.

> **Совет:** Для плавного перемещения к нужной координате лучше использовать современный CSS-метод `scroll-behavior: smooth;` или метод `window.scrollTo({ top: 0, behavior: 'smooth' });`.

Интересует ли вас, как комбинировать `window.scrollY` с `window.innerHeight`, чтобы определить, достиг ли пользователь самого низа страницы?

---
*Rule 2: EXPERT GUIDE applied.*
