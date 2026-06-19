# JavaScript

JavaScript específico: funciones, arrow functions, closures, DOM, bucles, arrays.

**Total de preguntas:** 35

---

## Pregunta 1

¿Con JavaScript se puede trabajar funciones sin necesidad de tener una clase asociada a ella?

- ✅ Sí
- ○ No

> **Respuesta correcta:** Opción 1

---

## Pregunta 2

En JavaScript ¿qué objeto es clave para poder acceder a los elementos del navegador?

- ✅ document
- ○ windows
- ○ elements
- ○ objects

> **Respuesta correcta:** Opción 1

---

## Pregunta 3

Una función arrow de JavaScript que suma dos números es…

- ✅ (a, b) ⇒ a + b
- ○ () ⇒ a + b
- ○ (a, b) → a + b
- ○ (a, 2) ⇒ a + 2

> **Respuesta correcta:** Opción 1

---

## Pregunta 4

What is the expected output of the following code?
```js
const a = 5;
const b = -3;
console.log(a > 0 || (b > 0 && a < -10));
```

- ✅ TRUE
- ○ FALSE
- ○ undefined
- ○ Error

> **Respuesta correcta:** Opción 1

---

## Pregunta 5

Given the following code snippet, what is the purpose of the second parameter in the `stringify` function call?
```js
let obj = { name: 'Object', id: 11 };
function stringifyExample(key, value) {
  if (typeof value !== 'string') return undefined;
  return value;
}
JSON.stringify(obj, stringifyExample);
```

- ✅ It is a replacer function that filters specific types of properties during stringification. The original object remains unmodified.
- ○ It formats the output indentation.
- ○ It validates the JSON before stringifying.
- ○ It converts all values to strings.

> **Respuesta correcta:** Opción 1

---

## Pregunta 6

What is the result of the following code snippet?
```js
var i = 0;
while(i < 5) {
  console.log(i);
  i++;
}
```

- ✅ The numbers 0-4 are logged to the console.
- ○ The numbers 1-5 are logged to the console.
- ○ The number 5 is logged once.
- ○ Infinite loop

> **Respuesta correcta:** Opción 1

---

## Pregunta 7

Given the following code snippet, which code block uses a for-in loop to iterate over the properties of the person object and log their **values** to the console?
```js
const person = { firstName: 'John', lastName: 'Doe', age: 56 };
```

- ✅ ```js
for(let x in person) { console.log(person[x]); }
```
- ○ ```js
for(let x in person) { console.log(x); }
```
- ○ ```js
for(let x of person) { console.log(x); }
```
- ○ ```js
for(let x of person) { console.log(person[x]); }
```

> **Respuesta correcta:** Opción 1

---

## Pregunta 8

What is an example of a while loop?

- ✅ ```js
let i = 0;
while(i < 10) {
  console.log(i);
  i++;
}
```
- ○ ```js
for(let i = 0; i < 10; i++) {}
```
- ○ ```js
do {} while(false);
```
- ○ ```js
if(true) {}
```

> **Respuesta correcta:** Opción 1

---

## Pregunta 9

When the `.length` property is used on an empty array, which value is returned?

- ✅ 0
- ○ null
- ○ undefined
- ○ -1

> **Respuesta correcta:** Opción 1

---

## Pregunta 10

What is the purpose of the `default` keyword in a switch statement?

- ✅ It defines a block of code that will be executed if none of the cases match.
- ○ It sets the initial case.
- ○ It breaks out of the switch.
- ○ It defines a fallback value for the expression.

> **Respuesta correcta:** Opción 1

---

## Pregunta 11

Given the following array, which statement returns the number of elements?
```js
let pets = ['cat', 'dogs', 'birds', 'fish'];
```

- ✅ pets.length
- ○ pets.size()
- ○ pets.count
- ○ len(pets)

> **Respuesta correcta:** Opción 1

---

## Pregunta 12

When considering JavaScript rules for variable names, which are true? (Select all that apply.)

- ✅ Numbers cannot be the first character of a variable name.
- ○ Variable names can contain spaces.
- ✅ Reserved keywords cannot be used as variable names.
- ○ Variable names are case-insensitive.

> **Respuestas correctas:** Opciones 1, 3

---

## Pregunta 13

Given the following nested loops, how many times is the outer loop called?
```js
const shoes = ['sandals', 'sneakers', 'loafers', 'slippers'];
const colors = ['green', 'blue', 'red', 'silver', 'white'];
for (let i = 0; i < shoes.length; i++) {
  for(let j = 0; j < colors.length; j++) {
    console.log(shoes[i] + ' ' + colors[j]);
  }
}
```

- ✅ 4
- ○ 5
- ○ 20
- ○ 9

> **Respuesta correcta:** Opción 1

---

## Pregunta 14

Which function uses a try-catch-finally block?

- ✅ ```js
function checkDoor(who) {
  try { /* ... */ }
  catch(err) { /* ... */ }
  finally { /* ... */ }
}
```
- ○ ```js
function checkDoor(who) { if(who) {} }
```
- ○ ```js
function checkDoor(who) { throw 'error'; }
```
- ○ ```js
function checkDoor(who) { return who; }
```

> **Respuesta correcta:** Opción 1

---

## Pregunta 15

Given the following function, how would it look as an arrow function?
```js
hello = function() {
  return 'Hello World!';
}
```

- ✅ ```js
hello = () => { return 'Hello World!'; }
```
- ○ ```js
hello => { return 'Hello World!'; }
```
- ○ ```js
hello = function => 'Hello World!';
```
- ○ ```js
() => hello = 'Hello World!';
```

> **Respuesta correcta:** Opción 1

---

## Pregunta 16

Which line of code correctly passes the `sayHello` function as an argument to another function?
```js
function sayHello() {
  console.log('Hello World!');
}
```

- ✅ ```js
setTimeout(sayHello, 3000);
```
- ○ ```js
setTimeout(sayHello(), 3000);
```
- ○ ```js
setTimeout('sayHello', 3000);
```
- ○ ```js
setTimeout(() => sayHello(), 3000);
```

> **Respuesta correcta:** Opción 1

---

## Pregunta 17

Given the following array, which line removes 'fish' from the array?
```js
var pets = ['cats', 'dogs', 'birds', 'fish'];
```

- ✅ pets.pop()
- ○ pets.shift()
- ○ pets.slice(3)
- ○ pets.splice(3, 1)

> **Respuesta correcta:** Opción 1

---

## Pregunta 18

Given `let n = 5;` which option correctly reassigns `n` with a new data type?

- ✅ ```js
n = 'This is a reassignment.';
```
- ○ ```js
n = true;
```
- ○ ```js
n = 10;
```
- ○ ```js
let n = 'text';
```

> **Respuesta correcta:** Opción 1

---

## Pregunta 19

Which statements about the `return` keyword are true in JavaScript? (Select all that apply.)

- ✅ The `return` keyword ends the execution of a function.
- ○ A function can have multiple return values.
- ✅ If a return statement does not have a value, it returns `undefined`.
- ○ The `return` keyword can only be used once per function.

> **Respuestas correctas:** Opciones 1, 3

---

## Pregunta 20

Which statements are true regarding arrow functions? (Select all that apply.)

- ○ Arrow functions require the `function` keyword.
- ✅ No `function` keyword is required when using an arrow function.
- ✅ There is no binding of `this` in arrow functions.
- ✅ Arrow functions offer a way to write shorter, more concise function syntax.

> **Respuestas correctas:** Opciones 2, 3, 4

---

## Pregunta 21

What is the purpose of comments in JavaScript?

- ○ To execute code conditionally.
- ✅ To explain code and provide metadata.
- ○ To debug errors automatically.
- ○ To optimize performance.

> **Respuesta correcta:** Opción 2

---

## Pregunta 22

Which switch statement is correct?

- ✅ ```js
let text;
let pet = 'cat';
switch(pet) {
  case 'cat': text = 'Meow!'; break;
  case 'dog': text = 'Bark!'; break;
  case 'bird': text = 'Chirp!'; break;
}
```
- ○ ```js
switch pet {
  case 'cat' -> 'Meow!'
}
```
- ○ ```js
switch(pet) { 'cat' => 'Meow!' }
```
- ○ ```js
if switch(pet) case 'cat' { 'Meow!' }
```

> **Respuesta correcta:** Opción 1

---

## Pregunta 23

Which code snippet uses nested conditionals to create the following behavior?
1. Alert 'You can drive.' if age >= 16 and hasLicense is true.
2. Alert 'You need a license.' if age >= 16 and hasLicense is false.
3. Alert 'You must be 16 to get a license.' if age < 16.

- ✅ ```js
if(age >= 16) {
  if(hasLicense) alert('You can drive.');
  else alert('You need a license.');
} else alert('You must be 16 to get a license.');
```
- ○ ```js
if(age >= 16 && hasLicense) alert('You can drive.');
```
- ○ ```js
if(hasLicense) alert('You can drive.');
else alert('You need a license.');
```
- ○ ```js
if(age < 16) alert('You must be 16.');
```

> **Respuesta correcta:** Opción 1

---

## Pregunta 24

Given the following code snippet, which call passes functions correctly as arguments?
```js
function sayBye(daytime, nighttime) {
  let currentTime = 1600;
  if(currentTime > 1700 || currentTime < 400) nighttime();
  else daytime();
}
function sayGoodBye() { console.log('good bye!'); }
function sayGoodNight() { console.log('good night!'); }
```

- ○ ```js
sayBye(sayGoodNight(), sayGoodBye());
```
- ○ ```js
sayBye('sayGoodNight', 'sayGoodBye');
```
- ✅ ```js
sayBye(sayGoodNight, sayGoodBye);
```
- ○ ```js
sayBye(() => sayGoodNight, () => sayGoodBye);
```

> **Respuesta correcta:** Opción 3

---

## Pregunta 25

What is returned to `n` from the sum function if no arguments are passed?
```js
function sum(p1, p2) { return p1 + p2; }
let n = sum();
```

- ✅ NaN
- ○ undefined
- ○ null
- ○ 0

> **Respuesta correcta:** Opción 1

---

## Pregunta 26

Given the array `const degrees = [45, 30, 28, 44];` and function `function checkFreezing(temp) { return temp <= 32; }` which statement returns an array of freezing temperatures?

- ✅ degrees.filter(checkFreezing)
- ○ degrees.map(checkFreezing)
- ○ degrees.reduce(checkFreezing)
- ○ degrees.forEach(checkFreezing)

> **Respuesta correcta:** Opción 1

---

## Pregunta 27

Which function uses AJAX to update a portion of the webpage when a button is clicked?
```html
<div id='test'>
  <button type='button' onclick='loadDoc()'>Change Content</button>
</div>
```

- ✅ ```js
function loadDoc() {
  const xhttp = new XMLHttpRequest();
  xhttp.onload = function() {
    document.getElementById('test').innerHTML = this.responseText;
  };
  xhttp.open('GET', 'test.txt');
  xhttp.send();
}
```
- ○ ```js
function loadDoc() { fetch('test.txt'); }
```
- ○ ```js
function loadDoc() { $('#test').load('test.txt'); }
```
- ○ ```js
function loadDoc() { window.location = 'test.txt'; }
```

> **Respuesta correcta:** Opción 1

---

## Pregunta 28

Which variable names are legal and valid according to JavaScript rules? (Select all that apply.)

- ✅ legal_name5
- ○ 2fast2furious
- ○ my-var
- ✅ legalName
- ○ let
- ✅ legal_name

> **Respuestas correctas:** Opciones 1, 4, 6

---

## Pregunta 29

Which snippet creates a for loop that logs the numbers 0-4?

- ✅ ```js
for(let i = 0; i < 5; i++) { console.log(i); }
```
- ○ ```js
for(let i = 1; i <= 5; i++) { console.log(i); }
```
- ○ ```js
for(let i = 0; i <= 4; i++) { console.log(i++); }
```
- ○ ```js
for(let i = 0; i > 5; i++) { console.log(i); }
```

> **Respuesta correcta:** Opción 1

---

## Pregunta 30

Given the code snippet, what will `isEqual` return?
```js
let arr1 = [5, 14, 70];
let arr2 = [5, 14, 70];
function isEqual(arr1, arr2) {
  let equality = false;
  if(arr1 == arr2) equality = true;
  return equality;
}
```

- ✅ false
- ○ true
- ○ undefined
- ○ Error

> **Respuesta correcta:** Opción 1

---

## Pregunta 31

What is the difference between a for-in loop and a for-of loop?

- ○ for-in iterates over values of an iterable; for-of iterates over object properties.
- ○ for-in and for-of are identical.
- ✅ for-in loops through properties of an object; for-of loops through values of an iterable.
- ○ for-in is for numbers; for-of is for strings.

> **Respuesta correcta:** Opción 3

---

## Pregunta 32

Given `const veggies = ['peas', 'carrots', 'broccoli', 'cabbage'];` which statement removes the first element?

- ✅ veggies.shift()
- ○ veggies.pop()
- ○ veggies.slice(0)
- ○ veggies.splice(1, 1)

> **Respuesta correcta:** Opción 1

---

## Pregunta 33

What is a correct example of nested for loops?

- ✅ ```js
for(let i = 0; i < 4; i++) {
  for(let j = 0; j < 4; j++) { /* ... */ }
}
```
- ○ ```js
for(let i = 0; i < 4; i++) { /* ... */ }
for(let j = 0; j < 4; j++) { /* ... */ }
```
- ○ ```js
for(let i = 0; i < 4; i++) { for(let i = 0; i < 4; i++) { /* ... */ } }
```
- ○ ```js
for(let i = 0, j = 0; i < 4; i++, j++) { /* ... */ }
```

> **Respuesta correcta:** Opción 1

---

## Pregunta 34

Given the following variable declarations `let a = 3; let b = 15;` which conditions evaluate to true? (Select all that apply.)

- ✅ if(a > 0 || b > 0)
- ○ if(a > 0 && b < 0)
- ✅ if(a > 0 || b < 0)
- ✅ if(a < 0 || b > 0)

> **Respuestas correctas:** Opciones 1, 3, 4

---

## Pregunta 35

Which function correctly demonstrates the use of default parameters?

- ✅ ```js
function sum(a, b, c = 0, d = 0) { return a + b + c + d; }
```
- ○ ```js
function sum(a, b, c, d) { return a + b + c + d; }
```
- ○ ```js
function sum(a, b) { let c = 0; let d = 0; }
```
- ○ ```js
function sum(a = 0, b) { return a + b; }
```

> **Respuesta correcta:** Opción 1

---
