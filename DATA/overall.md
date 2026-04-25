# Сборный конспект: Блок DATA (Piscine JS)


### 1. Основы и Типы
* **Primitives:** `String`, `Number`, `Boolean`, `Undefined`, `Null`, `Symbol`, `BigInt`.
* **Variables:** `let` (изменяемая), `const` (неизменяемая ссылка).
* **typeof:** Оператор определения типа значения.
* **Truthy/Falsy:** Значения, приводящиеся к `true` или `false` в логическом контексте.
* **Number:** Объект для работы с числами (`isInteger`, `parseInt`, `toFixed`).

### 2. Структуры и Манипуляции
* **Data Structures:** Объекты `{}` (пары ключ-значение) и Массивы `[]` (упорядоченные списки).
* **Set/Get:** Динамическая установка и получение свойств через `.` или `[]`.
* **Freeze:** `Object.freeze()` делает объект неизменяемым (первый уровень).
* **Spread (`...`):** Распаковка элементов массива или свойств объекта (копирование/слияние).
* **Map:** Коллекция пар ключ-значение с любыми типами ключей.
* **Object.entries / fromEntries:** Конвертация объекта в массив пар и обратно.

### 3. Логика и Вычисления
* **Functions:** Блоки кода для повторного использования.
* **If-else / Ternary:** Условное ветвление (`condition ? true : false`).
* **Math:** Математические утилиты (`abs` — модуль, `min/max` — поиск крайних значений, `sign` — знак числа).
* **Methods:** Встроенные функции типов (строковые: `split`, `join`, `toUpperCase`).
* **JSON.stringify:** Сериализация данных в строку формата JSON.

---

## Комплексный пример кода

Этот пример имитирует обработку данных в системе образования, используя все перечисленные темы.

```javascript
"use strict";

// 1. Variables & Freeze: Конфигурация системы
const CONFIG = Object.freeze({
  school: "Tomorrow School",
  minPassingGrade: 50,
  currency: "USD"
});

// 2. Data Structures & Primitives
const rawData = [
  { id: 1, name: "eleonora", scores: [85, 92, -10] }, // Ошибка в данных (-10)
  { id: 2, name: "sergey", scores: [40, 35, 45] },
  { id: 3, name: "admin", scores: [] }
];

// 3. Map: Хранилище метаданных
const studentMeta = new Map();
studentMeta.set(1, "Founder");
studentMeta.set(2, "Student");

// 4. Functions & Logic
function processStudents(data) {
  // Spread syntax: Копируем массив, чтобы не мутировать оригинал
  return [...data].map(student => {
    
    // Set/Get & Methods
    const fullName = student.name.toUpperCase();
    const role = studentMeta.get(student.id) || "Guest";

    // Math & Operators
    // Исправляем отрицательные оценки через Math.abs
    const validScores = student.scores.map(s => Math.abs(s));
    
    const maxScore = validScores.length ? Math.max(...validScores) : 0;
    const minScore = validScores.length ? Math.min(...validScores) : 0;
    const total = validScores.reduce((acc, val) => acc + val, 0);
    const avg = validScores.length ? Number((total / validScores.length).toFixed(2)) : 0;

    // Math.sign & If-else
    let trend = "stable";
    if (Math.sign(maxScore - minScore) === 1) {
      trend = "variable";
    }

    // Ternary operator & Truthy/Falsy
    // Если avg > minPassingGrade — true
    const status = avg >= CONFIG.minPassingGrade ? "Passed" : "Failed";

    // Object.entries / fromEntries: Фильтрация свойств
    // Убираем техническое поле 'id' для итогового отчета
    const filteredInfo = Object.fromEntries(
      Object.entries(student).filter(([key]) => key !== 'id')
    );

    return {
      ...filteredInfo, // Spread объекта
      fullName,
      role,
      avg,
      status,
      trend,
      isProcessed: true // Truthy значение
    };
  });
}

// 5. Execution & Output
const results = processStudents(rawData);

// typeof check
if (typeof results === 'object') {
  // JSON.stringify для вывода
  const report = JSON.stringify(results, null, 2);
  console.log(`--- ${CONFIG.school} Report ---`);
  console.log(report);
}

/* Пример логики Truthy/Falsy в конце:
Если в массиве есть хоть один элемент (truthy), выводим успех.
*/
if (results.length) {
  console.log("Data processing complete.");
}
```
