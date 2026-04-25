# Сборный конспект: Блок FIND (Регулярные выражения в JS)

Этот этап полностью посвящен работе с текстом, поиску паттернов и валидации данных с помощью регулярных выражений (RegEx) и встроенных методов JavaScript.

### 1. Якоря (Anchors)
Якоря не ищут текст, они проверяют позицию:
* **`^`** — Начало строки.
* **`$`** — Конец строки.
* **Флаг `m` (multiline)** — Заставляет `^` и `$` работать для каждой новой строки (после `\n`), а не только для всего текста целиком.

### 2. Наборы символов (Character Sets)
Позволяют указать варианты для **одного** символа:
* **`[abc]`** — Совпадет с `a`, `b` или `c`.
* **`[a-z0-9]`** — Диапазоны (любая строчная буква или цифра).
* **`[^abc]`** — Отрицание (любой символ, **кроме** `a`, `b` или `c`).

### 3. Сокращенные наборы (Shorthand Character Sets)
Делают код короче и читабельнее:
* **`.`** — Любой символ (кроме переноса строки).
* **`\d` / `\D`** — Цифра / Не цифра.
* **`\w` / `\W`** — Словарный символ (буквы латиницы, цифры, `_`) / Не словарный символ.
* **`\s` / `\S`** — Пробельный символ (пробел, таб, перенос) / Непробельный символ.

### 4. Базовый синтаксис (Квантификаторы, Группы, Флаги)
* **Квантификаторы:** `*` (0 или больше), `+` (1 или больше), `?` (0 или 1), `{n,m}` (от n до m раз).
* **Ленивость:** Добавление `?` после квантификатора (например, `.+?`) заставляет его остановиться при первом совпадении, а не захватывать максимум.
* **Группировка `(...)`:** Объединяет символы и сохраняет их в памяти (захват). `(?:...)` — группирует, но не сохраняет.
* **Альтернатива `|`:** Логическое ИЛИ.
* **Флаги:** `g` (глобальный поиск), `i` (игнор регистра).

### 5. Проверки (Lookarounds)
Проверяют соседей без захвата их в итоговый результат:
* **`(?=...)` / `(?!...)`** — Позитивная / Негативная опережающая проверка (смотрит вперед).
* **`(?<=...)` / `(?<!...)`** — Позитивная / Негативная ретроспективная проверка (смотрит назад).

### 6. Использование в JavaScript
* **Методы RegExp:** `regex.test(str)` (вернет true/false), `regex.exec(str)` (детальный поиск в цикле).
* **Методы String:** `str.match(regex)` (получить массив совпадений), `str.replace(regex, newStr)` (замена текста), `str.search(regex)` (поиск индекса), `str.split(regex)` (разбивка в массив).

---

## Комплексный пример кода

В этом примере мы пишем парсер, который обрабатывает "сырой" лог с данными пользователей, применяя все изученные концепции.

```javascript
"use strict";

const rawLog = `
[2026-04-25] USER_ADDED
Username: Eleonora_99
Email: admin@tomorrow-school.edu
Phone: +7 (777) 123-45-67
Balance: $5400.50
PasswordHash: aB3!xyz8
---
[2026-04-26] USER_ADDED
Username: john-doe
Email: invalid-email@com
Phone: 8-800-555-3535
Balance: $0.00
PasswordHash: weak
`;

function parseUserLogs(logText) {
  // 1. String.split(): Разбиваем лог на отдельные блоки пользователей по разделителю "---"
  // Используем якоря ^ и $ с флагом 'm', чтобы точно резать по строкам с дефисами
  const userBlocks = logText.split(/^---$/m);
  const parsedUsers = [];

  for (const block of userBlocks) {
    if (!block.trim()) continue; // Пропускаем пустые блоки

    // 2. Character Sets, Shorthands & String.match(): Ищем Email
    // Паттерн: словарные символы/точки/дефисы + @ + домен + точка + 2-6 букв
    const emailRegex = /[\w.-]+@[a-zA-Z\d.-]+\.[a-zA-Z]{2,6}/;
    const emailMatch = block.match(emailRegex);
    const email = emailMatch ? emailMatch[0] : null;

    // 3. String.replace() & Отрицание: Извлекаем и нормализуем телефон
    // Ищем строку "Phone: ", затем захватываем остаток строки (группа 1)
    const phoneMatch = block.match(/^Phone:\s*(.+)$/m);
    // Оставляем ТОЛЬКО цифры, удаляя всё, что является \D (не цифрой)
    const cleanPhone = phoneMatch ? phoneMatch[1].replace(/\D/g, '') : null;

    // 4. Lookbehinds (Ретроспективная проверка): Получаем баланс без знака $
    // Ищем цифры и возможную дробную часть, перед которыми СТРОГО стоит '$'
    const balanceRegex = /(?<=\$)\d+(\.\d{2})?/;
    const balanceMatch = block.match(balanceRegex);
    const balance = balanceMatch ? Number(balanceMatch[0]) : 0;

    // 5. RegExp.test() & Lookaheads (Опережающая проверка): Проверка сложности пароля
    // Требования: минимум 1 цифра, 1 заглавная буква, длина от 6 символов
    const passMatch = block.match(/^PasswordHash:\s*(.+)$/m);
    const rawPass = passMatch ? passMatch[1] : '';
    const isPassStrong = /^(?=.*\d)(?=.*[A-Z]).{6,}$/.test(rawPass);

    // 6. RegExp.exec() & Группы: Достаем дату из начала блока
    // Ищем паттерн [ГГГГ-ММ-ДД], захватывая содержимое в скобки
    const dateRegex = /^\[(\d{4}-\d{2}-\d{2})\]/;
    const dateExec = dateRegex.exec(block.trim());
    const date = dateExec ? dateExec[1] : 'Unknown';

    parsedUsers.push({
      date,
      email,
      phone: cleanPhone,
      balance,
      isPasswordSecure: isPassStrong
    });
  }

  return parsedUsers;
}

// Запуск и вывод результатов
const extractedData = parseUserLogs(rawLog);
console.log(JSON.stringify(extractedData, null, 2));

/* Результат работы:
[
  {
    "date": "2026-04-25",
    "email": "admin@tomorrow-school.edu",
    "phone": "77771234567",
    "balance": 5400.5,
    "isPasswordSecure": true
  },
  {
    "date": "2026-04-26",
    "email": null,                 <-- invalid-email@com не прошел валидацию
    "phone": "88005553535",
    "balance": 0,
    "isPasswordSecure": false      <-- "weak" не прошел опережающие проверки
  }
]
*/
```
