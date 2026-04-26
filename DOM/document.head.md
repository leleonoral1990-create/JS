# Свойство document.head

Свойство **`document.head`** возвращает элемент `<head>` текущего документа. Это удобный способ быстрого доступа к контейнеру, который содержит метаинформацию о странице (стили, скрипты, мета-теги, заголовки).



---

## 1. Синтаксис

```javascript
const headElement = document.head;
```

**Возвращаемое значение:**
* Объект **`HTMLHeadElement`**.
* Это свойство доступно только для чтения (read-only), хотя вы можете изменять содержимое внутри самого элемента `<head>`.

---

## 2. Ключевые особенности

1.  **Сокращение для поиска**: Это свойство является современным эквивалентом записи `document.getElementsByTagName('head')[0]`.
2.  **Всегда доступен**: В корректном HTML-документе этот элемент всегда присутствует. Если в разметке тег `<head>` опущен, браузер создаст его автоматически при парсинге.
3.  **Место для метаданных**: В этот элемент обычно добавляются:
    * Стили (`<link>`, `<style>`).
    * Скрипты (`<script>`).
    * Мета-теги (`<meta>`).
    * Заголовок страницы (`<title>`).
    * Фавиконы.

---

## 3. Примеры использования в коде

### 1. Динамическое добавление стилей
Позволяет загрузить внешний CSS-файл только при наступлении определенного события.

```javascript
const link = document.createElement('link');
link.rel = 'stylesheet';
link.href = 'https://example.com/styles.css';

// Вставляем в head
document.head.append(link);
```

### 2. Изменение иконки сайта (favicon)
```javascript
function changeFavicon(src) {
  let link = document.querySelector("link[rel~='icon']");
  
  if (!link) {
    link = document.createElement('link');
    link.rel = 'icon';
    document.head.append(link);
  }
  
  link.href = src;
}

changeFavicon('/new-icon.png');
```

### 3. Добавление Meta-тега
```javascript
const meta = document.createElement('meta');
meta.name = 'viewport';
meta.content = 'width=device-width, initial-scale=1';

document.head.append(meta);
```

---

## 4. Сравнение основных свойств Document

| Свойство | Возвращаемый элемент | Назначение |
| :--- | :--- | :--- |
| **`document.documentElement`** | `<html>` | Корневой элемент документа. |
| **`document.head`** | `<head>` | Метаданные и ресурсы. |
| **`document.body`** | `<body>` | Основное содержимое страницы. |

---

> **Важно:** При манипуляциях с `document.head` (например, добавлении тяжелых скриптов без атрибута `async`/`defer`) можно случайно заблокировать отрисовку страницы пользователю, так как браузер парсит содержимое `<head>` перед тем, как начать отображать `<body>`.
