# Сборный конспект: Блок LOOP (Piscine JS)

### 1. Циклы и Рекурсия (Управление потоком)
* **Loops (`while`, `for..of`):** Базовые конструкции для многократного выполнения кода. `while` работает, пока условие истинно. `for..of` используется для удобного перебора итерируемых коллекций (массивов, строк).
* **Recursion (Рекурсия):** Функция, вызывающая саму себя. Альтернатива циклам. Обязательно требует "базового случая" (условия выхода), чтобы избежать переполнения стека.

### 2. Поиск и извлечение в Массивах
* **`Array.includes(value)`:** Возвращает `true`/`false`. В отличие от других методов поиска, способен найти `NaN`.
* **`Array.indexOf(value)`:** Возвращает индекс первого найденного элемента (слева направо) или `-1`. Использует строгое сравнение (`===`).
* **`Array.lastIndexOf(value)`:** Ищет элемент с конца массива (справа налево) и возвращает его индекс или `-1`.
* **`Array.slice(start, end)`:** Немутирующий метод. Возвращает новый массив, копируя элементы от `start` до `end` (не включая `end`).

### 3. Трансформация Массивов
* **`Array.reverse()`:** **Мутирующий** метод. Разворачивает массив задом наперед (изменяет оригинал и возвращает ссылку на него).
* **`Array.join(separator)`:** Склеивает все элементы массива в одну строку, вставляя между ними указанный `separator` (по умолчанию запятая).
* **`Array.flat(depth)`:** "Распаковывает" вложенные массивы в один плоский массив до указанной глубины (по умолчанию 1, для полной распаковки — `Infinity`). Удаляет пустые ячейки.

### 4. Строки и Математика
* **`String.repeat(count)`:** Создает новую строку, повторяя исходную указанное количество раз.
* **`String.split(separator)`:** Разбивает строку на массив подстрок по указанному разделителю. Логическая противоположность `Array.join()`.
* **`Math`:** Глобальный объект для математических операций (`Math.max()`, `Math.min()`, `Math.abs()`, `Math.round()` и др.). Не является конструктором.

---

## Комплексный пример кода

Данный скрипт демонстрирует обработку "грязного" лога данных, применяя все вышеперечисленные концепции.

```javascript
"use strict";

// Исходные вложенные данные с разными типами
const rawLog = [
  "START",
  [15, -42.5, 8],
  "ERROR_404",
  [NaN, [100, 200]],
  "ERROR_404",
  "END"
];

// 1. Array.flat: Распаковываем массив на всю глубину
const flatLog = rawLog.flat(Infinity);
// Результат: ["START", 15, -42.5, 8, "ERROR_404", NaN, 100, 200, "ERROR_404", "END"]

// 2. Loops (for..of) & Array.includes: Сортировка данных
const strings = [];
const numbers = [];

for (const item of flatLog) {
  if (typeof item === 'string') {
    strings.push(item);
  } else if (typeof item === 'number' && !Number.isNaN(item)) {
    numbers.push(item);
  }
}

// Проверяем наличие битых данных (NaN) с помощью includes
const hasCorruptedData = flatLog.includes(NaN);

// 3. Array.indexOf & Array.lastIndexOf: Поиск индексов дубликатов
const firstErrorIndex = strings.indexOf("ERROR_404"); // 1
const lastErrorIndex = strings.lastIndexOf("ERROR_404"); // 2

// 4. Array.slice: Берем часть массива (без "START" и "END")
// Индексы 1 и 3 (не включая 3)
const errorsOnly = strings.slice(1, 3); // ["ERROR_404", "ERROR_404"]

// 5. Array.reverse, Array.join, String.split: Трансформации
// Мутируем массив, склеиваем в строку и снова разбиваем по другому разделителю
const reversedStrings = strings.slice().reverse(); // Копируем через slice(), затем переворачиваем
const joinedLog = reversedStrings.join(" | "); // "END | ERROR_404 | ERROR_404 | START"
const splitLog = joinedLog.split(" | "); // Снова массив

// 6. Math: Математическая обработка числового лога
// Ищем модули, минимумы и максимумы
const maxVal = Math.max(...numbers); // 200
const minVal = Math.min(...numbers); // -42.5
const absMin = Math.abs(minVal);     // 42.5
const roundedAbs = Math.round(absMin); // 43

// 7. Recursion: Рекурсивная функция для подсчета суммы чисел
const calculateSum = (arr, index = 0) => {
  // Базовый случай: дошли до конца массива
  if (index === arr.length) return 0;
  // Рекурсивный шаг
  return arr[index] + calculateSum(arr, index + 1);
};

const totalSum = calculateSum(numbers); // 15 + (-42.5) + 8 + 100 + 200 = 280.5

// 8. String.repeat & while: Создание декоративного заголовка
let header = "=".repeat(15); 
let stars = "";
let counter = 0;

while (counter < 3) {
  stars += "*";
  counter++;
}

console.log(`${header}${stars} LOG REPORT ${stars}${header}`);
console.log(`Corrupted Data (NaN): ${hasCorruptedData}`);
console.log(`First Error at string index: ${firstErrorIndex}`);
console.log(`Max Value: ${maxVal}, Rounded Abs Min: ${roundedAbs}`);
console.log(`Total Numbers Sum (Recursive): ${totalSum}`);
console.log(`Reversed Log: ${joinedLog}`);
```
