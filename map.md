# Структура данных Map в JavaScript

**Map** — это коллекция для хранения данных в виде пар «ключ: значение», подобно обычному объекту (`Object`). Главное отличие `Map` заключается в том, что он **позволяет использовать ключи любого типа** (включая объекты, функции и любые примитивы), тогда как обычный объект приводит все ключи к строкам или символам.

Кроме того, `Map` всегда сохраняет порядок добавления элементов.

## Создание Map
Map создается с помощью ключевого слова `new`. При создании можно сразу передать итерируемый объект (например, массив массивов) с начальными данными.

```javascript
// Создание пустого Map
const myMap = new Map();

// Создание Map с начальными данными
const userMap = new Map([
  ['name', 'Alex'],
  ['age', 30]
]);
```

---

## Основные свойства и методы

### 1. `set(key, value)`
Добавляет или обновляет значение по ключу. Возвращает сам `Map`, что позволяет объединять вызовы в цепочку (chaining).

```javascript
const map = new Map();

// Ключом может быть строка
map.set('1', 'str1');

// Ключом может быть число
map.set(1, 'num1');

// Ключом может быть логическое значение
map.set(true, 'bool1');

// Ключом может быть объект!
const objKey = { id: 123 };
map.set(objKey, 'Данные объекта');

// Цепочка вызовов
map.set('name', 'John').set('age', 25);
```

### 2. `get(key)`
Возвращает значение по ключу. Если ключа нет, возвращает `undefined`.

```javascript
console.log(map.get('1'));    // "str1"
console.log(map.get(1));      // "num1" (Map различает типы ключей: '1' и 1 — это разные ключи)
console.log(map.get(objKey)); // "Данные объекта"
```

### 3. `has(key)`
Возвращает `true`, если ключ существует в `Map`, и `false` в противном случае.

```javascript
console.log(map.has('name')); // true
console.log(map.has('email')); // false
```

### 4. `delete(key)`
Удаляет элемент по ключу. Возвращает `true`, если элемент существовал и был удален, и `false`, если такого ключа не было.

```javascript
map.delete('age');
console.log(map.has('age')); // false
```

### 5. `clear()`
Полностью очищает `Map`, удаляя все элементы.

```javascript
map.clear();
```

### 6. Свойство `size`
В отличие от объектов, у `Map` есть встроенное свойство `size`, которое возвращает текущее количество элементов.

```javascript
const map2 = new Map();
map2.set('a', 1);
map2.set('b', 2);
console.log(map2.size); // 2
```

---

## Итерация (Перебор элементов)

`Map` является итерируемым объектом, поэтому его элементы можно перебирать в цикле `for...of` или встроенным методом `forEach`. Порядок перебора всегда строго соответствует порядку добавления элементов.

```javascript
const recipeMap = new Map([
  ['огурец', 500],
  ['помидор', 350],
  ['лук', 50]
]);
```

### Перебор ключей: `keys()`
Возвращает итерируемый объект для ключей.
```javascript
for (let vegetable of recipeMap.keys()) {
  console.log(vegetable); // "огурец", "помидор", "лук"
}
```

### Перебор значений: `values()`
Возвращает итерируемый объект для значений.
```javascript
for (let amount of recipeMap.values()) {
  console.log(amount); // 500, 350, 50
}
```

### Перебор пар [ключ, значение]: `entries()`
Возвращает итерируемый объект для пар. Это поведение по умолчанию при использовании `for...of` напрямую на `Map`.
```javascript
for (let [key, value] of recipeMap) { // То же самое, что и recipeMap.entries()
  console.log(`${key}: ${value}`); 
  // "огурец: 500", "помидор: 350", "лук: 50"
}
```

### Метод `forEach`
Работает аналогично методу массивов `Array.prototype.forEach`.
```javascript
recipeMap.forEach((value, key, map) => {
  console.log(`${key}: ${value}`);
});
```

---

## Преобразование: Map ↔ Object ↔ Array

**Из Объекта в Map:**
Если данные приходят в виде обычного объекта, их можно конвертировать с помощью `Object.entries`.
```javascript
const obj = { name: "Alex", age: 30 };
const mapFromObj = new Map(Object.entries(obj));
```

**Из Map в Объект:**
Используется `Object.fromEntries`.
```javascript
const newObj = Object.fromEntries(mapFromObj); 
// { name: "Alex", age: 30 }
```

**Из Map в Массив (Spread-синтаксис):**
```javascript
const arr = [...recipeMap]; 
// [['огурец', 500], ['помидор', 350], ['лук', 50]]
```
