# Функция isValid() (библиотека date-fns)

`date-fns` — это популярная библиотека для работы с датами в JavaScript, которую часто называют "Lodash для дат". Она предоставляет множество мелких, независимых функций. 

Функция `isValid()` проверяет, является ли переданное значение корректным объектом даты.

## Почему это нужно? Проблема нативного JS

В чистом JavaScript создание даты из бессмысленной строки не вызывает ошибку:
```javascript
const myDate = new Date('какая-то дичь');
console.log(myDate); // вернет объект со статусом "Invalid Date"
```
Технически `myDate` — это объект типа `Date`, но его внутреннее числовое значение времени (timestamp) равно `NaN` (Not-a-Number). Проверять это нативным кодом громоздко (`isNaN(myDate.getTime())`). Функция `isValid()` делает эту проверку элегантной и читаемой.

---

## Синтаксис

```javascript
import { isValid } from 'date-fns';

isValid(date)
```

**Параметры:**
* **`date`**: Объект `Date` или число (timestamp), которое нужно проверить.

**Возвращаемое значение:**
* `true`, если дата корректна.
* `false`, если дата некорректна (является `Invalid Date`).

---

## Примеры использования

### 1. Проверка корректных дат
```javascript
import { isValid } from 'date-fns';

// Текущая дата
const today = new Date();
console.log(isValid(today)); // true

// Корректная строка, переведенная в дату
const parsedDate = new Date('2026-04-26');
console.log(isValid(parsedDate)); // true

// Корректный timestamp (число миллисекунд)
const timestamp = 1612137600000;
console.log(isValid(timestamp)); // true
```

### 2. Отлов "Invalid Date"
```javascript
// Попытка создать дату из некорректной строки
const badDate = new Date('hello world');

console.log(isValid(badDate)); // false
```

### 3. Важный нюанс версий v2+ (Строки)
Начиная со 2-й версии библиотеки `date-fns`, функции строго ожидают на вход объекты `Date` или числа (timestamp). Если вы передадите просто строку, `isValid` может вернуть `false` (так как строка — это не объект `Date`) или потребовать явного парсинга.

```javascript
// ❌ ПЛОХО (передача строки напрямую)
console.log(isValid('2026-04-26')); // false

// ✅ ХОРОШО (сначала преобразуем в объект Date)
console.log(isValid(new Date('2026-04-26'))); // true
```
