# Специалностите на JavaScript

Тази глава накратко обобщава характеристиките на JavaScript, които научихме досега, обръщайки специално внимание на финните моменти.

## Структура на кода

Изразите са разделени с точка и запетая:

```js run no-beautify
alert('Здравей'); alert('Свят!');
```

Обикновено прекъсването на реда се третира и като разделител, така че това също ще работи:

```js run no-beautify
alert('Здравей')
alert('Свят!')
```

Това се нарича "автоматично вмъкване на запетая". Понякога не работи, например:

```js run
alert("След това съобщение ще има грешка")

[1, 2].forEach(alert)
```

Повечето ръководства по стилa на кода са съгласни, че трябва да поставяме запетая след всеки израз.

След блок от код `{...}` и синтактични конструкции не се изискват запетая, като циклите:

```js
function f() {
  // след деклариране на функция не е необходима точка и запетая
}

for(;;) {
  // не е необходима точка и запетая след цикъла
}
```

...Но дори и ако можем да поставим "екстра" точка и запетая някъде, това не е грешка. Тя ще бъде игнорирана.

Още в: <info:structure>.

## Строг/Стриктен режим

За да активираме напълно всички функции на съвременния JavaScript, трябва да започнем скриптовете ни с `"use strict"`.

```js
'use strict';

...
```

Директивата трябва да бъде в началото на скрипта или в началото на тялото на функцията.

Без `"use strict"`, всичко все още ще работи, но някои функции ще се държат по старомоден начин, или в "съвместим" начин. По принцип предпочитаме модерното поведение.

Някои съвременни характеристики на езика (като класовете, които ще ги научим в бъдеще) активират строгия режин безусловно.

Още в: <info:strict-mode>.

## Променливи

Могат да бъдат декларирани използвайки:

- `let`
- `const` (константа, статичени стойности / не могат да се променят)
- `var` (стар стил, ще ги видим по-късно)

Името на променливата може да включва:
- Букви и цифри, но първият знак може да не е цифра.
- Символите `$` и `_` са нормални, наравно с буквите.
- Нелатинските букви и йероглифи също са позволени, но обикновено не се използват.

Променливите са динамично типизирани. Могат да съхранят всякакви стойности:

```js
let x = 5;
x = "Иван";
```

Съществуват 8 типа данни:

- `number` както за числата с плаваща запетая, така и за целите числа,
- `bigint` за цели числа с произволна дължина,
- `string` за стрингове,
- `boolean` за логически стойности: `true/false`,
- `null` -- тип с единична стойност `null`, означаващ "празно" или "не съществуващ",
- `undefined` -- тип с единична стойност `undefined`, означаващ "не е зададен",
- `object` и `symbol` -- за сложни структури от данни и уникални идентификатори, които все още не сме ги научили.

Операторът `typeof` връща типа за стойност, с две изключения:
```js
typeof null == "object" // грешка в езика
typeof function(){} == "function" // функциите се третират специално
```

Още в: <info:variables> и <info:types>.

## Взаимодействие

Ние използваме браузър като работна среда, така че обикновените функции на потребителския интерфейс ще са:

[`prompt(question, [default])`](mdn:api/Window/prompt)
: Да попитаме за `question`, и да върнем това, което потребителят е въвел или `null` ако са натиснали "Cancel" (Отказ).

[`confirm(question)`](mdn:api/Window/confirm)
: Да попитаме за `question` и да предлагаме да се избира между "Ok" и "Cancel". Изборът се връща като `true/false`.

[`alert(message)`](mdn:api/Window/alert)
: Изпринтира `message`.

Всички тези функции са *модални*, т.е. спират изпълнението на кода и не позволяват на посетителя да взаимодейства със страницата, докато не отговори.

Например:

```js run
let userName = prompt("Как се казвате?", "Алиса");
let isTeaWanted = confirm("Искате ли чай?");

alert( "Посетител: " + userName ); // Алиса
alert( "Искали се чай: " + isTeaWanted ); // true (Да)
```

Още в: <info:alert-prompt-confirm>.

## Оператори

JavaScript поддържа следните оператори:

Аритметични
: Обикновенни: `* + - /`, също и `%` за остатъка от модулното делене и `**` за степенуване на число.

    Бинарния плюс `+` конкатенира стринговете. И ако някой от операндите е стринг, другият също се преобразува в стринг:

    ```js run
    alert( '1' + 2 ); // '12', стринг
    alert( 1 + '2' ); // '12', стринг
    ```

Присвояване
: Има просто присвояване: `a = b` и комбинирано такъв като този `a *= 2`.

Побитови операции
: Побитовите операции работят с 32 битови цели числа в най-ниското побитово ниво: погледнете [документацията](mdn:/JavaScript/Reference/Operators/Bitwise_Operators) когато ви e нужнo.

  ```js run
      alert(5 & 13); // 0101 & 1101 = 0101
      // Очакван изход: 5;

      alert(5 | 13); // 0101 | 1101 = 1101
      // Очакван изход: 13
  ```
  
Оператор на нулево коализиране
: Операторът `??` опредоставя начин за избор на определена стойност от списък на променливи. Резултатът на `a ?? b` е `a` освен ако `null/undefined`, тогава е `b`.

Условни
: Единственият оператор с три параметъра: `cond ? resultA : resultB`. Ако `cond` е вярно, връща `resultA`, иначе връща `resultB`.

  ```js run
      let isBigger = 1 > 0 ? true : false // true
  ```

Логически оператори
: Логически 'И" `&&` или "ИЛИ" `||` оценява уравнението и връща стойността в която е спряла (не е задължително да е `true`/`false`). Логическия "НЕ" `!` превръща стойността към "boolean" тип и връща противоположната й стойност.

  ```js run
      alert( true || false ); // true
      alert( true && false ); // false
      alert(!true); // false
  ```

Сравненията
: Проверката за равенство `==` за стойности от различни типове ги преобразува в число (освен `null` и `undefined` които се равняват един на друг и нищо друго), така че те са равни:

  ```js run
  alert( 0 == false ); // true
  alert( 0 == '' ); // true
  ```

   Други сравнения същo се преобразуват в число.

   Операторът за строго равенство `===` не прави преобразуването: различни видове винаги означават различни стойности за него.

   Стойностите `null` и `undefined` са специални: те са равни `==` помежду си и не се равняват на нищо друго.

   Сравнение за По-голямо/По-малко сравнява стринговете буква по буква, други видове се преобразуват в число.

Други оператори
: Има малко други, като оператора "comma" (запетайка).

Още в: <info:operators>, <info:comparison>, <info:logical-operators>.

## Цикли

- Покрихме 3 вида цикли:

    ```js
    // 1
    while (condition) {
      ...
    }

    // 2
    do {
      ...
    } while (condition);

    // 3
    for(let i = 0; i < 10; i++) {
      ...
    }
    ```

- Променливата, декларирана в `for(let...)` цикъла е видима само вътре в цикъла. Но можем и да пропуснем `let` и използваме повторно съществуващата променлива.
- Директивите `break/continue` ни позволяват да излезем от целия цикъл/итерация. Използвайте етикетите, за да спрете вложените цикли.

Подробности в: <info:while-for>.

По-късно ще проучим повече видове цикли за справяне с обекти.

## Констукцията "switch"

Констукцията "switch" може да замени многократните `if` проверки. То използва `===` (строго равенство) за сравнения.

Например:

```js run
let age = prompt('Възрастта ви?', 18);

switch (age) {
  case 18:
    alert("Няма да проработи"); // резултатът на "prompt" е стринг, а не число

  case "18":
    alert("Това работи!");
    break;

  default:
    alert("Всяка стойност, която не е равна на нито една от тези по-горе");
}
```

Подробности в: <info:switch>.

## Функции

Разкрихме три начина за създаване на функция в JavaScript:

1. Декларация за функция: функцията в основния поток на код

    ```js
    function sum(a, b) {
      let result = a + b;

      return result;
    }
    ```

2. Функция Израз: функцията в контекста на израз

    ```js
    let sum = function(a, b) {
      let result = a + b;

      return result;
    };
    ```

3. "Arrow" или т.н. функции със стрелкички:

    ```js
    // изразът е в дясната страна
    let sum = (a, b) => a + b;

    // или многоредов синтаксис с { ... }, тука обаче се нуждаем от израза "return" тук:
    let sum = (a, b) => {
      // ...
      return a + b;
    }

    // без аргументи
    let sayHi = () => alert("Hello");

    // с един аргумент
    let double = n => n * 2;
    ```

- Функциите могат да имат локални променливи: тези, декларирани вътре в тялото му. Такива променливи са видими само във функцията.
- Параметрите могат да имат стойности по подразбиране: `function sum(a = 1, b = 2) {...}`.
- Функциите винаги връщат нещо.Ако няма `return` израз, тогава резултатът е `undefined`.

<<<<<<< HEAD
Подробности: вижте <info:function-basics>, <info:arrow-functions-basics>.
=======
- Functions may have local variables: those declared inside its body or its parameter list. Such variables are only visible inside the function.
- Parameters can have default values: `function sum(a = 1, b = 2) {...}`.
- Functions always return something. If there's no `return` statement, then the result is `undefined`.

Details: see <info:function-basics>, <info:arrow-functions-basics>.
>>>>>>> f6ae0b5a5f3e48074312ca3e47c17c92a5a52328

## Има още

Това беше кратък списък с функциите на JavaScript. Към момента сме изучавали само основите. По-нататък в урока ще намерите още специални и разширени функции на JavaScript.
