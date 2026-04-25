# Глобальный объект Number в JavaScript

Объект `Number` является объектом-обёрткой, позволяющим работать с числовыми значениями. Его можно использовать как функцию для приведения типов (превращения строк и других типов в числа) или использовать его встроенные константы и методы.

*Примечание к материалу: свойства `Number.number` не существует в стандартном JavaScript, скорее всего это опечатка в исходном списке. Все остальные методы и свойства разобраны ниже.*

---

## 1. Функция `Number` (Приведение типов)
Если вызвать `Number` как функцию, она попытается преобразовать переданное значение в примитивное число.

```javascript
console.log(Number('42'));      // 42
console.log(Number('42.5px'));  // NaN (не может распарсить, если есть буквы)
console.log(Number(true));      // 1
console.log(Number(false));     // 0
console.log(Number(null));      // 0
console.log(Number(undefined)); // NaN
```

---

## 2. Статические свойства (Константы)
Это фиксированные значения, встроенные в сам объект `Number`. Они используются для определения границ возможностей чисел в JS.

* **`Number.EPSILON`**
    Разница между единицей и минимальным числом, большим единицы, которое может быть представлено в JS. Используется для точного сравнения дробей (решает проблему `0.1 + 0.2`).
    ```javascript
    const result = Math.abs((0.1 + 0.2) - 0.3);
    console.log(result < Number.EPSILON); // true
    ```

* **`Number.MAX_SAFE_INTEGER` / `Number.MIN_SAFE_INTEGER`**
    Максимальное и минимальное **безопасное** целое число ($2^{53} - 1$ и $-(2^{53} - 1)$). За этими пределами математика целых чисел начинает ошибаться из-за потери точности.
    ```javascript
    console.log(Number.MAX_SAFE_INTEGER); // 9007199254740991
    console.log(Number.MIN_SAFE_INTEGER); // -9007199254740991
    ```

* **`Number.MAX_VALUE` / `Number.MIN_VALUE`**
    Максимальное и минимальное (ближайшее к нулю, но не отрицательное) представимое число с плавающей точкой.
    ```javascript
    console.log(Number.MAX_VALUE); // ~ 1.79E+308
    console.log(Number.MIN_VALUE); // 5e-324
    ```

* **`Number.NaN`**
    Специальное значение "Not-A-Number" (не число). Аналогично глобальному `NaN`.

* **`Number.POSITIVE_INFINITY` / `Number.NEGATIVE_INFINITY`**
    Положительная и отрицательная бесконечность. Возвращается при переполнении или делении на ноль.
    ```javascript
    console.log(1 / 0 === Number.POSITIVE_INFINITY); // true
    console.log(-1 / 0 === Number.NEGATIVE_INFINITY); // true
    ```

---

## 3. Статические методы
Вызываются напрямую у объекта `Number`. Помогают проверять числа или извлекать их из строк.

* **`Number.isFinite(value)`**
    Проверяет, является ли переданное значение конечным числом (не `Infinity`, не `-Infinity` и не `NaN`). В отличие от глобального `isFinite()`, **не** пытается принудительно преобразовать строку в число.
    ```javascript
    console.log(Number.isFinite(42));       // true
    console.log(Number.isFinite(Infinity)); // false
    console.log(Number.isFinite('42'));     // false (строка не число)
    ```

* **`Number.isInteger(value)`**
    Проверяет, является ли переданное значение целым числом (без дробной части).
    ```javascript
    console.log(Number.isInteger(42));   // true
    console.log(Number.isInteger(42.5)); // false
    ```

* **`Number.isNaN(value)`**
    Проверяет, равно ли значение строго `NaN`. Это более безопасная и строгая версия глобальной функции `isNaN()`.
    ```javascript
    console.log(Number.isNaN(NaN));       // true
    console.log(Number.isNaN('hello'));   // false (строка — это строка, а не NaN)
    ```

* **`Number.isSafeInteger(value)`**
    Проверяет, является ли число целым и находится ли оно в безопасных пределах (между `MIN_SAFE_INTEGER` и `MAX_SAFE_INTEGER`).
    ```javascript
    console.log(Number.isSafeInteger(9007199254740991)); // true
    console.log(Number.isSafeInteger(9007199254740992)); // false
    ```

* **`Number.parseInt(string, radix)` / `Number.parseFloat(string)`**
    Извлекают целое (`parseInt`) или дробное (`parseFloat`) число из строки. Читают строку слева направо, пока не встретят нечисловой символ. `radix` указывает систему счисления (от 2 до 36, по умолчанию 10).
    ```javascript
    console.log(Number.parseInt('42px'));       // 42
    console.log(Number.parseInt('101', 2));     // 5 (из двоичной в десятичную)
    console.log(Number.parseFloat('3.14rem'));  // 3.14
    ```

---

## 4. Методы экземпляра (Методы прототипа)
Эти методы указаны в списке как `Number.toFixed` и т.д., но на практике они вызываются **у самих чисел** (экземпляров).
*Внимание: если вы вызываете метод напрямую у числа-литерала, нужно использовать две точки `..` или оборачивать число в скобки `()`, чтобы интерпретатор не спутал точку с десятичной запятой.*

* **`toExponential(fractionDigits)`**
    Возвращает строку, представляющую число в экспоненциальной (научной) записи.
    ```javascript
    const num = 123456;
    console.log(num.toExponential(2)); // "1.23e+5"
    ```

* **`toFixed(digits)`**
    Форматирует число, оставляя указанное количество знаков после запятой, округляя при необходимости. **Возвращает строку!**
    ```javascript
    const num = 12.345;
    console.log(num.toFixed(2)); // "12.35"
    console.log((42).toFixed(3)); // "42.000"
    ```

* **`toLocaleString(locales, options)`**
    Возвращает строку с языко-зависимым представлением числа (например, с разделением тысяч или форматом валюты).
    ```javascript
    const num = 1234567.89;
    console.log(num.toLocaleString('ru-RU')); // "1 234 567,89"
    console.log(num.toLocaleString('en-US', { style: 'currency', currency: 'USD' })); // "$1,234,567.89"
    ```

* **`toPrecision(precision)`**
    Возвращает строку, представляющую число с указанной точностью (общее количество значащих цифр).
    ```javascript
    const num = 12.345;
    console.log(num.toPrecision(3)); // "12.3"
    console.log(num.toPrecision(5)); // "12.345"
    ```

* **`toString(radix)`**
    Возвращает строковое представление числа в указанной системе счисления (от 2 до 36).
    ```javascript
    const num = 255;
    console.log(num.toString());   // "255" (десятичная)
    console.log(num.toString(16)); // "ff" (шестнадцатеричная)
    console.log(num.toString(2));  // "11111111" (двоичная)
    ```

* **`valueOf()`**
    Возвращает примитивное значение объекта `Number`. В повседневном коде используется редко, так как JS вызывает его автоматически (под капотом) при математических операциях.
    ```javascript
    const numObj = new Number(42);
    console.log(typeof numObj); // "object"
    console.log(typeof numObj.valueOf()); // "number"
    ```
