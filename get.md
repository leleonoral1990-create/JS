# Доступ к значениям

Для работы с созданными структурами данных необходимо уметь извлекать из них значения. Ниже приведены примеры данных, которые будут использоваться в этом разделе:

```javascript
const street = {
  number: 175,
  type: 'boulevard',
  name: 'Matabiau',
}

const address = {
  country: 'France',
  town: 'Toulouse',
  postalCode: 31000,
  street,
}

const userClement = {
  firstname: 'Clement',
  lastname: 'Denis',
  email: 'cdenis@example.com',
  age: 29,
  address,
}

const users = [
  userClement,
  {
    firstname: 'Sofiane',
    lastname: 'Martinez',
    email: 'smartine@example.com',
    age: 34,
    address: {
      country: 'France',
      town: 'St-Ouens',
      postalCode: 93400,
      street: {
        number: 78,
        type: 'rue',
        name: 'Garibaldi',
      },
    },
  },
]

const allowedCountries = ['France', 'Spain', 'Portugal', 'Russia', 'Iceland']

const coords = [
  [32, 45],
  [-38, 57],
  [87, 99],
  [57, -2],
  [-74, -29],
]
```

---

## 1. Доступ через квадратные скобки `['key']`

Первый способ получить доступ к значению объекта — использовать квадратные скобки и строковый ключ.

```javascript
console.log(street['number']) // Получит значение 175
console.log(street['name'])   // Получит значение 'Matabiau'
```

### Несуществующие свойства
Если попытаться получить доступ к свойству, которое не определено в объекте, JavaScript вернет `undefined`.

```javascript
console.log(userClement['name']) // Ключ 'name' не определен, результат: undefined
console.log(userClement['firstname']) // 'Clement'
```

### Вложенный доступ
Для доступа к глубоко вложенным свойствам скобки используются последовательно:

```javascript
console.log(userClement['address']['town'])           // 'Toulouse'
console.log(userClement['address']['street']['name']) // 'Matabiau'
```

---

## 2. Доступ к элементам массива

В массивах ключами являются числовые индексы. Отсчет всегда начинается с **0**.

```javascript
console.log(allowedCountries[0]) // 'France'
console.log(allowedCountries[1]) // 'Spain'
```

### Вложенные массивы
Для доступа к значениям в многомерных массивах индексы указываются друг за другом:

```javascript
console.log(coords[0][0]) // 32
console.log(coords[3][1]) // -2
```

**Предупреждение:** Важно не пытаться получить доступ к свойству несуществующего элемента. Например, `coords[151][0]` вызовет ошибку, так как `coords[151]` вернет `undefined`, а у `undefined` нельзя прочитать свойство `[0]`.

---

## 3. Динамический доступ (переменные и выражения)

Поскольку ключи — это строки, можно использовать значения переменных для обращения к свойствам.

```javascript
const myKey = 'name'
console.log(street[myKey])   // Использует значение переменной ('name'), выведет 'Matabiau'
console.log(street['myKey']) // Использует строку 'myKey', выведет undefined
```

Внутри квадратных скобок можно писать любые выражения, которые возвращают допустимую строку или индекс:

```javascript
// Составление ключа из строк и переменных
console.log(userClement['last' + myKey]) // Обратится к 'lastname'

// Математические операции для индексов массива
const start = 1
let position = 0
console.log(allowedCountries[start + position++]) // allowedCountries[1]
console.log(allowedCountries[start + position++]) // allowedCountries[2]
```

---

## 4. Упрощенный доступ через точку `.`

Если ключ является допустимым идентификатором, можно использовать более простой синтаксис через точку.

```javascript
console.log(userClement.address.street.name) // 'Matabiau'
```

Этот метод вы используете с самого начала в команде `console.log`. Объект `console` имеет свойство `log`, которое является функцией. Технически можно было бы написать `console['log']('text')`, но точечная нотация гораздо удобнее.

**Важно:** JavaScript всегда неявно преобразует ключи в строки перед доступом к свойству объекта.

---

## 5. Смешивание структур

Объекты и массивы можно комбинировать в цепочки любой сложности.

```javascript
console.log(users[1].address.street.number) // 78
console.log(users[0].address.street.number) // 175
```

**Ограничение:** Нельзя использовать точечную нотацию для доступа к индексам массива. Запись `users.1` является недопустимой, так как идентификатор не может начинаться с цифры. В таких случаях всегда используются квадратные скобки: `users[1]`.
