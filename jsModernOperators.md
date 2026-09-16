# 📦 08. Data Structures, Modern Operators and Strings

> **Topic:** `ES6+`
> Destructuring, spread/rest, short-circuiting, maps, sets, and string methods.
## 📑 Table of Contents

- [Destructuring Arrays](#destructuring-arrays)
- [Destructuring Objects](#destructuring-objects)
- [The Spread Operator (...)](#the-spread-operator)
- [Rest Pattern and Parameters](#rest-pattern-and-parameters)
- [Short Circuiting (&& and Il)](#short-circuiting-and-il)
- [The Nullish Coalescing Operator (??)](#the-nullish-coalescing-operator)
- [Logical Assignment Operators](#logical-assignment-operators)
- [Coding Challenge ##1](#coding-challenge-1)
- [Looping Arrays: The for-of Loop](#looping-arrays-the-for-of-loop)
- [Enhanced Object Literals](#enhanced-object-literals)
- [Optional Chaining (?.)](#optional-chaining)
- [Looping Objects: Object Keys, Values, and Entries](#looping-objects-object-keys-values-and-entries)
- [Coding Challenge ##2](#coding-challenge-2)
- [Sets](#sets)
- [Maps: Fundamentals](#maps-fundamentals)
- [Maps: Iterataion](#maps-iterataion)
- [Summary: Which Data Structure to Use?](#summary-which-data-structure-to-use)
- [Coding Challenge ##3](#coding-challenge-3)
- [Working With Strings Part 1](#working-with-strings-part-1)
- [Working With Strings Part 2](#working-with-strings-part-2)
- [Working With Strings Part 3](#working-with-strings-part-3)
- [Coding Challenge ##4](#coding-challenge-4)
- [String Methods Practice](#string-methods-practice)

---

## Destructuring Arrays
> 📝 **What is Destructuring?** Destructuring is an ES6 feature that lets you "unpack" values from arrays or properties from objects into distinct variables. It's cleaner and more declarative than manual indexing.

**Why Use Destructuring?**
- Less code: `const [a, b] = arr` vs `const a = arr[0]; const b = arr[1];`
- Self-documenting: Variable names describe what each position means
- Easy swapping: `[a, b] = [b, a]` (no temp variable needed!)
- Nested unpacking: `const [a, [b, c]] = nested`


```js
'use strict';

const { SlowBuffer } = require('buffer');

// Data needed for a later exercise
const flights =
  '_Delayed_Departure;fao93766109;txl2133758440;11:25+_Arrival;bru0943384722;fao93766109;11:45+_Delayed_Arrival;hel7439299980;fao93766109;12:05+_Departure;fao93766109;lis2323639855;12:30';

// Data needed for first part of the section
const restaurant = {
  name: 'Classico Italiano',
  location: 'Via Angelo Tavanti 23, Firenze, Italy',
  categories: ['Italian', 'Pizzeria', 'Vegetarian', 'Organic'],
  starterMenu: ['Focaccia', 'Bruschetta', 'Garlic Bread', 'Caprese Salad'],
  mainMenu: ['Pizza', 'Pasta', 'Risotto'],

  openingHours: {
    thu: {
      open: 12,
      close: 22,
    },
    fri: {
      open: 11,
      close: 23,
    },
    sat: {
      open: 0, // Open 24 hours
      close: 24,
    },
  },
  order: function (starterIndex, mainIndex) {
    return [this.starterMenu[starterIndex], this.mainMenu[mainIndex]];
  },
};
```
```js
const arr = [2, 3, 4];
const a = arr[0];
const b = arr[1];
const c = arr[2];
console.log(a, b, c);

// Can be done as following using destructring

const [x, y, z] = arr;

// Nested Destructuringc
const nested = [2, 4, [5, 6]];
// Const [i, , j] = nested;
const [i, , [j, k]] = nested;
console.log(i, j, k);

let [main, ,starter] = restaurant.categories;
console.log(main, starter)

// Switching variables
// Const temp = main
// Main = secondary
// Secondary = main
// console.log(main, secondary)

// Recieve 2 values from a function
const [starter, mainCourse] = restaurant.order(2, 0);
console.log(starter, mainCourse); // → Garlic Bread Pizza

// Without Default Values
const [p, q, r] = [8, 9];
console.log(p, q, r); // → p = 8, q = 9, r = undefined

// With Default Values
const [s = 1, t = 1, u = 1] = [8, 9];
console.log(s, t, u); // → s = 8, t = 9, u = 1
```

---

## Destructuring Objects
> 💡 **Object vs. Array Destructuring:** Array destructuring uses **position** (`[a, b] = arr`), while object destructuring uses **property names** (`{a, b} = obj`). The variable names must match the property keys unless you rename them.

**Practical Use Cases:**
- Extracting API response fields
- Destructuring function parameters: `function({name, age})`
- Setting defaults for missing properties: `{name = 'Guest'} = obj`
- Renaming for clarity: `{name: userName} = obj`


```js
const restaurant = {
  name: 'Classico Italiano',
  location: 'Via Angelo Tavanti 23, Firenze, Italy',
  categories: ['Italian', 'Pizzeria', 'Vegetarian', 'Organic'],
  starterMenu: ['Focaccia', 'Bruschetta', 'Garlic Bread', 'Caprese Salad'],
  mainMenu: ['Pizza', 'Pasta', 'Risotto'],

  openingHours: {
    thu: {
      open: 12,
      close: 22,
    },
    fri: {
      open: 11,
      close: 23,
    },
    sat: {
      open: 0, // Open 24 hours
      close: 24,
    },
  },
  order: function (starterIndex, mainIndex) {
    return [this.starterMenu[starterIndex], this.mainMenu[mainIndex]];
  },
  // Using object destructuring with default values in function parameters
  orderDelivery: function ({ starterIndex = 1, mainIndex = 0, time = '20:00', address }) {
    console.log(
      `Order received! ${this.starterMenu[starterIndex]} and ${this.mainMenu[mainIndex]} will be delivered to ${address} at ${time}`
    );
  },
  // Using rest parameters to accept any number of arguments
  orderPizza: function (mainIngredient, ...otherIngredients) {
    console.log(mainIngredient);
    console.log(otherIngredients);
  },
};
```

```js
///////////////////////////////////////
// Destructuring Objects
restaurant.orderDelivery({
  time: '22:30',
  address: 'Via del Sole, 21',
  mainIndex: 2,
  starterIndex: 2,
});

restaurant.orderDelivery({
  address: 'Via del Sole, 21',
  starterIndex: 1,
});

const { name, openingHours, categories } = restaurant;
console.log(name, openingHours, categories);

const {
  name: restaurantName,
  openingHours: hours,
  categories: tags,
} = restaurant;
console.log(restaurantName, hours, tags);

// Default values
const { menu = [], starterMenu: starters = [] } = restaurant;
console.log(menu, starters);

// Mutating variables
let a = 111;
let b = 999;
const obj = { a: 23, b: 7, c: 14 };
({ a, b } = obj);
console.log(a, b);

// Nested objects
const {
  fri: { open: o, close: c },
} = openingHours;
console.log(o, c);


///////////////////////////////////////
// Destructuring Arrays
const arr = [2, 3, 4];
const a = arr[0];
const b = arr[1];
const c = arr[2];

const [x, y, z] = arr;
console.log(x, y, z);
console.log(arr);

let [main, , secondary] = restaurant.categories;
console.log(main, secondary);

// Switching variables
// Const temp = main;
// Main = secondary;
// Secondary = temp;
// console.log(main, secondary);

[main, secondary] = [secondary, main];
console.log(main, secondary);

// Receive 2 return values from a function
const [starter, mainCourse] = restaurant.order(2, 0);
console.log(starter, mainCourse);

// Nested destructuring
const nested = [2, 4, [5, 6]];
// Const [i, , j] = nested;
const [i, , [j, k]] = nested;
console.log(i, j, k);

// Default values
const [p = 1, q = 1, r = 1] = [8, 9];
console.log(p, q, r);
```

---

## The Spread Operator (...)
> 📝 **The Spread Operator** (`...`) "spreads" the elements of an iterable (array, string, map, set) into individual elements. Think of it as "unpacking" a box of items so you can use them individually.

**Three Main Uses:**
1. **Copy arrays:** `const copy = [...original]`
2. **Merge arrays:** `const merged = [...arr1, ...arr2]`
3. **Pass to functions:** `Math.max(...numbers)`

> 💡 **Shallow Copy Warning:** `const copy = [...arr]` creates a shallow copy. If the array contains objects, those inner objects are still shared references.


```js
///////////////////////////////////////
// The Spread Operator (...)

const arr = [7, 8, 9];
const badNewArr = [1, 2, arr[0], arr[1], arr[2]];
console.log(badNewArr); // → [1,2,7,8,9]

const newArr = [1, 2, ...arr];
console.log(newArr); // → [1,2,7,8,9]

console.log(...newArr); // → 1,2,7,8,9
console.log(1, 2, 7, 8, 9); // → 1,2,7,8,9

const newMenu = [...restaurant.mainMenu, 'Gnocci'];
console.log(newMenu); // → ['Pizza', 'Pasta', 'Risotto', 'Gnocci']

// Copy array
const mainMenuCopy = [...restaurant.mainMenu];

// Join 2 arrays
const menu = [...restaurant.starterMenu, ...restaurant.mainMenu];
console.log(menu);

// Iterables: arrays, strings, maps, sets. NOT objects
const str = 'Jonas';
const letters = [...str, ' ', 'S.'];
console.log(letters); // → ['J', 'o', 'n', 'a', 's', ' ', 'S.']
console.log(...str); // → J o n a s
// console.log(`${...str} Schmedtmann`); // ❌ SyntaxError - spread doesn't work inside template literals

// Real-world example
const ingredients = [
  // Prompt("Let's make pasta! Ingredient 1?"),
  // Prompt('Ingredient 2?'),
  // Prompt('Ingredient 3'),
];
console.log(ingredients);

restaurant.orderPasta(ingredients[0], ingredients[1], ingredients[2]);
restaurant.orderPasta(...ingredients);

// Objects
const newRestaurant = { foundedIn: 1998, ...restaurant, founder: 'Guiseppe' };
console.log(newRestaurant);

const restaurantCopy = { ...restaurant };
restaurantCopy.name = 'Ristorante Roma';
console.log(restaurantCopy.name);
console.log(restaurant.name);
```
- Spread operator takes all the elements from the array and it also doesn't create new variables. And as a consequence, we can only use it in places where we would otherwise write values separated by commas.
- two important use cases of the spread operator are which is to create shallow copies of arrays, and to merge two arrays together.
- So basically, most of the built-in data structures in JavaScript are now iterables, but except objects.

---

## Rest Pattern and Parameters
> 📝 **Spread vs. Rest — Same Syntax, Opposite Direction:**
> - **Spread** "expands" an array into elements → used on the **right side** of `=` or in arguments
> - **Rest** "collects" elements into an array → used on the **left side** of `=` or in parameters

```js
// SPREAD (unpack)
const arr = [1, 2, ...[3, 4]];  // → [1, 2, 3, 4]

// REST (pack)
const [a, b, ...others] = [1, 2, 3, 4];  // others → [3, 4]
```

> 🎯 **Remember:** Spread unpacks. Rest packs. The direction of data flow determines which one you're using.


```js
///////////////////////////////////////
// Rest Pattern and Parameters
// 1) Destructuring

// SPREAD, because on RIGHT side of =
const arr = [1, 2, ...[3, 4]];

// REST, because on LEFT side of =
const [a, b, ...others] = [1, 2, 3, 4, 5];
console.log(a, b, others); // → 1, 2, [3, 4, 5]

const [pizza, , risotto, ...otherFood] = [
  ...restaurant.mainMenu,
  ...restaurant.starterMenu,
];
console.log(pizza, risotto, otherFood); // does not include the skipped values

// Objects
const { sat, ...weekdays } = restaurant.openingHours;
console.log(weekdays);

// 2) Functions
const add = function (...numbers) {
  let sum = 0;
  for (let i = 0; i < numbers.length; i++) sum += numbers[i];
  console.log(sum);
};

add(2, 3);
add(5, 3, 7, 2);
add(8, 2, 5, 3, 2, 1, 4);

const x = [23, 5, 7];
add(...x);

restaurant.orderPizza('mushrooms', 'onion', 'olives', 'spinach');
restaurant.orderPizza('mushrooms');
```
- So the spread operator is used where we would otherwise write values, separated by a comma. On the other hand the rest is pattern is basically used where we would otherwise write variable names separated by commas.


---

## Short Circuiting (&& and Il)

```js
///////////////////////////////////////
// Short Circuiting (&& and ||)

console.log('---- OR ----');
// Use ANY data type, return ANY data type, short-circuiting
console.log(3 || 'Jonas'); // → 3
console.log('' || 'Jonas'); // → Jonas
console.log(true || 0); // → true
console.log(undefined || null); // → null

console.log(undefined || 0 || '' || 'Hello' || 23 || null); // → Hello

restaurant.numGuests = 0;
const guests1 = restaurant.numGuests ? restaurant.numGuests : 10;
console.log(guests1); // → 10

const guests2 = restaurant.numGuests || 10;
console.log(guests2); // →

console.log('---- AND ----');
console.log(0 && 'Jonas'); // → false
console.log(7 && 'Jonas'); // → true

	console.log('Hello' && 23 && null && 'jonas'); // → null

// Practical example
if (restaurant.orderPizza) {
  restaurant.orderPizza('mushrooms', 'spinach');
}

// Above if block can be written as
restaurant.orderPizza && restaurant.orderPizza('mushrooms', 'spinach');
```
- So the OR operator will return the first truthy. value of all the operands, or simply the last value if all of them are falsy.
- On the other hand, the AND operator will return the first falsy value or the last value if all of them are truthy.
- And as for practical applications, we can use the OR operator to set default values, and we can use the AND operator to execute code in the second operand if the first one IS true.

---

## The Nullish Coalescing Operator (??)

```js
// The Nullish Coalescing Operator
restaurant.numGuests = 0;
const guests = restaurant.numGuests || 10;
console.log(guests);

// Nullish: null and undefined (NOT 0 or '')
const guestCorrect = restaurant.numGuests ?? 10;
console.log(guestCorrect);
```
- The problem was that we wanted 'number of guests' to be zero but because of how  OR operator || works the result will be 10 instead of 0.
- In order to solve the problem ES20 introduced 'The Nullish Coalescing Operator (??)'

---

## Logical Assignment Operators

```js
///////////////////////////////////////
// Logical Assignment Operators
const rest1 = {
  name: 'Capri',
  // NumGuests: 20,
  numGuests: 0,
};

const rest2 = {
  name: 'La Piazza',
  owner: 'Giovanni Rossi',
};

// OR assignment operator
// Rest1.numGuests = rest1.numGuests || 10;
// Rest2.numGuests = rest2.numGuests || 10;
// Rest1.numGuests ||= 10;
// Rest2.numGuests ||= 10;

// Nullish assignment operator (null or undefined)
rest1.numGuests ??= 10;
rest2.numGuests ??= 10;

// AND assignment operator
// Rest1.owner = rest1.owner && '<ANONYMOUS>';
// Rest2.owner = rest2.owner && '<ANONYMOUS>';
rest1.owner &&= '<ANONYMOUS>';
rest2.owner &&= '<ANONYMOUS>';

console.log(rest1);
console.log(rest2);
```
- Introduced in ES2021
- And so basically, what the logical and assignment operator does is to assign a value to a variable if it is currently truthy.

---

## Coding Challenge ##1
We're building a football betting app (soccer for my American friends 😅)!

Suppose we get data from a web service about a certain game (below). In this challenge we're gonna work with the data. So here are your tasks:

1. Create one player array for each team (variables 'players1' and 'players2')
2. The first player in any player array is the goalkeeper and the others are field players. For Bayern Munich (team 1) create one variable ('gk') with the goalkeeper's name, and one array ('fieldPlayers') with all the remaining 10 field players
3. Create an array 'allPlayers' containing all players of both teams (22 players)
4. During the game, Bayern Munich (team 1) used 3 substitute players. So create a new array ('players1Final') containing all the original team1 players plus 'Thiago', 'Coutinho' and 'Perisic'
5. Based on the game.odds object, create one variable for each odd (called 'team1', 'draw' and 'team2')
6. Write a function ('printGoals') that receives an arbitrary number of player names (NOT an array) and prints each of them to the console, along with the number of goals that were scored in total (number of player names passed in)
7. The team with the lower odd is more likely to win. Print to the console which team is more likely to win, WITHOUT using an if/else statement or the ternary operator.

TEST DATA FOR 6: Use players 'Davies', 'Muller', 'Lewandowski' and 'Kimmich'. Then, call the function again with players from game.scored

```js
// 1.
const [players1, players2] = game.players;
console.log(players1, players2);

// 2.
const [gk, ...fieldPlayers] = players1;
console.log(gk, fieldPlayers);

// 3.
const allPlayers = [...players1, ...players2];
console.log(allPlayers);

// 4.
const players1Final = [...players1, 'Thiago', 'Coutinho', 'Periscic'];

// 5.
const {
  odds: { team1, x: draw, team2 },
} = game;
console.log(team1, draw, team2);

// 6.
const printGoals = function (...players) {
  console.log(players);
  console.log(`${players.length} goals were scored`);
};

// PrintGoals('Davies', 'Muller', 'Lewandowski', 'Kimmich');
// PrintGoals('Davies', 'Muller');
printGoals(...game.scored);

// 7.
team1 < team2 && console.log('Team 1 is more likely to win');
team1 > team2 && console.log('Team 2 is more likely to win');
```

---

## Looping Arrays: The for-of Loop

```js
///////////////////////////////////////
// The for-of Loop
const menu = [...restaurant.starterMenu, ...restaurant.mainMenu];

for (const item of menu) console.log(item);

for (const [i, el] of menu.entries()) {
  console.log(`${i + 1}: ${el}`);
}

// console.log([...menu.entries()]);
```

---

## Enhanced Object Literals

```js
const weekdays = ['mon', 'tue', 'wed', 'thu', 'fri', 'sat', 'sun'];
const openingHours = {
  [weekdays[3]]: {
    open: 12,
    close: 22,
  },
  [weekdays[4]]: {
    open: 11,
    close: 23,
  },
  [weekdays[5]]: {
    open: 0, // Open 24 hours
    close: 24,
  },
};

const restaurant = {
  name: 'Classico Italiano',
  location: 'Via Angelo Tavanti 23, Firenze, Italy',
  categories: ['Italian', 'Pizzeria', 'Vegetarian', 'Organic'],
  starterMenu: ['Focaccia', 'Bruschetta', 'Garlic Bread', 'Caprese Salad'],
  mainMenu: ['Pizza', 'Pasta', 'Risotto'],

  // ES6 enhanced object literals
  openingHours,

  order(starterIndex, mainIndex) {
    return [this.starterMenu[starterIndex], this.mainMenu[mainIndex]];
  },

  orderDelivery({ starterIndex = 1, mainIndex = 0, time = '20:00', address }) {
    console.log(
      `Order received! ${this.starterMenu[starterIndex]} and ${this.mainMenu[mainIndex]} will be delivered to ${address} at ${time}`
    );
  },

  orderPasta(ing1, ing2, ing3) {
    console.log(
      `Here is your declicious pasta with ${ing1}, ${ing2} and ${ing3}`
    );
  },

  orderPizza(mainIngredient, ...otherIngredients) {
    console.log(mainIngredient);
    console.log(otherIngredients);
  },
};
```

---

## Optional Chaining (?.)

```js
///////////////////////////////////////
// Optional Chaining
if (restaurant.openingHours && restaurant.openingHours.mon)
  console.log(restaurant.openingHours.mon.open); // → undefined

// console.log(restaurant.openingHours.mon.open);

// WITH optional chaining
console.log(restaurant.openingHours.mon?.open);
console.log(restaurant.openingHours?.mon?.open);

// Example
const days = ['mon', 'tue', 'wed', 'thu', 'fri', 'sat', 'sun'];

for (const day of days) {
  const open = restaurant.openingHours[day]?.open ?? 'closed';
  console.log(`On ${day}, we open at ${open}`);
}
// →
// On mon, we open at closed
// On Tue, open closed
// On wed, we open at closed
// On thu, we open at 12
// On fri, we open at 11
// On sat, we open at 0
// On sun, we open at closed

// Methods
console.log(restaurant.order?.(0, 1) ?? 'Method does not exist'); // → ['Focaccia', 'Pasta']

console.log(restaurant.orderRisotto?.(0, 1) ?? 'Method does not exist'); // → Method does not exist

// Arrays
const users = [{ name: 'Jonas', email: 'hello@jonas.io' }];
// Const users = [];

console.log(users[0]?.name ?? 'User array empty'); // → Jonas

if (users.length > 0) console.log(users[0].name);
else console.log('user array empty'); // → Jonas
```

---

## Looping Objects: Object Keys, Values, and Entries

```js
///////////////////////////////////////
// Looping Objects: Object Keys, Values, and Entries

// Property NAMES
const properties = Object.keys(openingHours);
console.log(properties); // → ['thu', 'fri', 'sat']

let openStr = `We are open on ${properties.length} days: `;
for (const day of properties) {
  openStr += `${day}, `;
}
console.log(openStr); // → We are open on 3 days: thu, fri, sat,

// Property VALUES
const values = Object.values(openingHours);
console.log(values);

// →
// 0: {open: 12, close: 22}
// 1: {open: 11, close: 23}
// 2: {open: 0, close: 24}

// Entire object
const entries = Object.entries(openingHours);
// console.log(entries);

// [key, value]
for (const [day, { open, close }] of entries) {
  console.log(`On ${day} we open at ${open} and close at ${close}`);
}
// →
// On thu we open at 12 and close at 22
// On fri we open at 11 and close at 23
// On sat we open at 0 and close at 24
```

---

## Coding Challenge ##2
Let's continue with our football betting app!

1. Loop over the game.scored array and print each player name to the console, along with the goal number (Example: "Goal 1: Lewandowski")
2. Use a loop to calculate the average odd and log it to the console (We already studied how to calculate averages, you can go check if you don't remember)
3. Print the 3 odds to the console, but in a nice formatted way, exaclty like this:
      Odd of victory Bayern Munich: 1.33
      Odd of draw: 3.25
      Odd of victory Borrussia Dortmund: 6.5

Get the team names directly from the game object, don't hardcode them (except for "draw"). HINT: Note how the odds and the game objects have the same property names

BONUS: Create an object called 'scorers' which contains the names of the players who scored as properties, and the number of goals as the value. In this game, it will look like this:

   ```js
{
	Gnarby: 1,
	Hummels: 1,
	Lewandowski: 2
  }
```
```js
const game = {
  team1: 'Bayern Munich',
  team2: 'Borrussia Dortmund',
  players: [
    [
      'Neuer',
      'Pavard',
      'Martinez',
      'Alaba',
      'Davies',
      'Kimmich',
      'Goretzka',
      'Coman',
      'Muller',
      'Gnarby',
      'Lewandowski',
    ],
    [
      'Burki',
      'Schulz',
      'Hummels',
      'Akanji',
      'Hakimi',
      'Weigl',
      'Witsel',
      'Hazard',
      'Brandt',
      'Sancho',
      'Gotze',
    ],
  ],
  score: '4:0',
  scored: ['Lewandowski', 'Gnarby', 'Lewandowski', 'Hummels'],
  date: 'Nov 9th, 2037',
  odds: {
    team1: 1.33,
    x: 3.25,
    team2: 6.5,
  },
};
```
**Solution**

```js
// 1.
for (const [i, player] of game.scored.entries())
  console.log(`Goal ${i + 1}: ${player}`);

// 2.
const odds = Object.values(game.odds);
let average = 0;
for (const odd of odds) average += odd;
average /= odds.length;
console.log(average);

// 3.
for (const [team, odd] of Object.entries(game.odds)) {
  const teamStr = team === 'x' ? 'draw' : `victory ${game[team]}`;
  console.log(`Odd of ${teamStr} ${odd}`);
}

// Odd of victory Bayern Munich: 1.33
// Odd of draw: 3.25
// Odd of victory Borrussia Dortmund: 6.5

// BONUS
// So the solution is to loop over the array, and add the array elements as object properties, and then increase the count as we encounter a new occurrence of a certain element
const scorers = {};
for (const player of game.scored) {
  scorers[player] ? scorers[player]++ : (scorers[player] = 1);
}
```

---

## Sets

```js
///////////////////////////////////////
// Sets
const ordersSet = new Set([
  'Pasta',
  'Pizza',
  'Pizza',
  'Risotto',
  'Pasta',
  'Pizza',
]);

console.log(ordersSet); // → Set(3) {'Pasta', 'Pizza', 'Risotto'}

console.log(new Set('Jonas')); // → Set(5) {'J', 'o', 'n', 'a', 's'}

console.log(ordersSet.size); // → 3
console.log(ordersSet.has('Pizza')); // → true
console.log(ordersSet.has('Bread')); // → false
ordersSet.add('Garlic Bread'); // → Set(4) {'Pasta', 'Pizza', 'Risotto', 'Garlic Bread'}
ordersSet.add('Garlic Bread'); // → // → Set(4) {'Pasta', 'Pizza', 'Risotto', 'Garlic Bread'}
ordersSet.delete('Risotto'); // → Set(3) {'Pasta', 'Pizza', 'Garlic Bread'}
// ordersSet.clear(); // → deletes all elements of set
console.log(ordersSet);

for (const order of ordersSet) console.log(order); // → Pasta Pizza Garlic Bread

// Example
const staff = ['Waiter', 'Chef', 'Waiter', 'Manager', 'Chef', 'Waiter'];
const staffUnique = [...new Set(staff)];
console.log(staffUnique); // → ['Waiter', 'Chef', 'Manager']

console.log(
  new Set(['Waiter', 'Chef', 'Waiter', 'Manager', 'Chef', 'Waiter']).size
); // → 3

console.log(new Set('jonasschmedtmann').size); // → 11
```
- And a set is basically just a collection of unique values. So that means that a set can never have any. duplicates.

---

## Maps: Fundamentals

```js
///////////////////////////////////////
// Maps: Fundamentals
const rest = new Map();
rest.set('name', 'Classico Italiano');
rest.set(1, 'Firenze, Italy');
console.log(rest.set(2, 'Lisbon, Portugal')); // → Map(3) {'name' => 'Classico Italiano', 1 => 'Firenze, Italy', 2 => 'Lisbon, Portugal'}

rest
  .set('categories', ['Italian', 'Pizzeria', 'Vegetarian', 'Organic'])
  .set('open', 11)
  .set('close', 23)
  .set(true, 'We are open :D')
  .set(false, 'We are closed :(');

console.log(rest.get('name')); // → Classico Italiano
console.log(rest.get(true)); // → We are open : D
console.log(rest.get(1)); // → Firenze, Italy

const time = 8;
console.log(rest.get(time > rest.get('open') && time < rest.get('close')));
// → We are closed :(

console.log(rest.has('categories')); // → true
rest.delete(2); // → it will delete - 2, 'Lisbon, Portugal'
// rest.clear(); // → deletes whole map

const arr = [1, 2]; // it is important to decalre 'arr' here.
rest.set(arr, 'Test');
rest.set(document.querySelector('h1'), 'Heading');
console.log(rest);
console.log(rest.size); // → 9

console.log(rest.get(arr)); // → Test
```

---

## Maps: Iterataion

```js
///////////////////////////////////////
// Maps: Iteration
const question = new Map([
  ['question', 'What is the best programming language in the world?'],
  [1, 'C'],
  [2, 'Java'],
  [3, 'JavaScript'],
  ['correct', 3],
  [true, 'Correct 🎉'],
  [false, 'Try again!'],
]);
console.log(question);

// Convert object to map
console.log(Object.entries(openingHours));
const hoursMap = new Map(Object.entries(openingHours));
console.log(hoursMap);

// Quiz app
console.log(question.get('question'));
for (const [key, value] of question) {
  if (typeof key === 'number') console.log(`Answer ${key}: ${value}`);
}
// Const answer = Number(prompt('Your answer'));
const answer = 3;
console.log(answer);

console.log(question.get(question.get('correct') === answer));

// Convert map to array
console.log([...question]);
// console.log(question.entries());
console.log([...question.keys()]);
console.log([...question.values()]);
```

---

## Summary: Which Data Structure to Use?
> 🖼️ **Visual:** *Data Structures, Modern Operators and Strings*

### Arrays vs. Sets
> 🖼️ **Visual:** *Data Structures, Modern Operators and Strings*

### Objects vs. Maps
> 🖼️ **Visual:** *Data Structures, Modern Operators and Strings*


---

## Coding Challenge ##3
Let's continue with our football betting app! This time, we have a map with a log of the events that happened during the game. The values are the events themselves, and the keys are the minutes in which each event happened (a football game has 90 minutes plus some extra time).

1. Create an array 'events' of the different game events that happened (no duplicates)
2. After the game has finished, is was found that the yellow card from minute 64 was unfair. So remove this event from the game events log.
3. Print the following string to the console: "An event happened, on average, every 9 minutes" (keep in mind that a game has 90 minutes)
4. Loop over the events and log them to the console, marking whether it's in the first half or second half (after 45 min) of the game, like this: [FIRST HALF] 17: ⚽️ GOAL

```JS
const gameEvents = new Map([
  [17, '⚽️ GOAL'],
  [36, '🔁 Substitution'],
  [47, '⚽️ GOAL'],
  [61, '🔁 Substitution'],
  [64, '🔶 Yellow card'],
  [69, '🔴 Red card'],
  [70, '🔁 Substitution'],
  [72, '🔁 Substitution'],
  [76, '⚽️ GOAL'],
  [80, '⚽️ GOAL'],
  [92, '🔶 Yellow card'],
]);

```
**Solution**
```js
// 1.
const events = [...new Set(gameEvents.values())];
console.log(events);

// 2.
gameEvents.delete(64);

// 3.
console.log(
  `An event happened, on average, every ${90 / gameEvents.size} minutes`
);
const time = [...gameEvents.keys()].pop();
console.log(time);
console.log(
  `An event happened, on average, every ${time / gameEvents.size} minutes`
);

// 4.
for (const [min, event] of gameEvents) {
  const half = min <= 45 ? 'FIRST' : 'SECOND';
  console.log(`[${half} HALF] ${min}: ${event}`);
}

```

---

## Working With Strings Part 1
```js
///////////////////////////////////////
// Working With Strings - Part 1
const airline = 'TAP Air Portugal';
const plane = 'A320';

console.log(plane[0]); // output: A
console.log(plane[1]); // output: 3
console.log(plane[2]); // output: 2
console.log('B737'[0]); // output:  B

console.log(airline.length); // output: 16
console.log('B737'.length); // output: 4

console.log(airline.indexOf('r')); // output: 6
console.log(airline.lastIndexOf('r')); // output: 10
console.log(airline.indexOf('portugal')); // output: -1

console.log(airline.slice(4)); // output: Air Portugal
console.log(airline.slice(4, 7)); // output: Air

console.log(airline.slice(0, airline.indexOf(' '))); // output: TAP
console.log(airline.slice(airline.lastIndexOf(' ') + 1)); // output: Portugal

console.log(airline.slice(-2)); // output: al
console.log(airline.slice(1, -1)); // output: AP Air Portuga

const checkMiddleSeat = function (seat) {
  // B and E are middle seats
  const s = seat.slice(-1);
  if (s === 'B' || s === 'E') console.log('You got the middle seat 😬');
  else console.log('You got lucky 😎');
};

checkMiddleSeat('11B'); // output: You got the middle seat 😬
checkMiddleSeat('23C'); // output: You got lucky 😎
checkMiddleSeat('3E');// output: You got the middle seat 😬

console.log(new String('jonas')); // output: String {'jonas'}
console.log(typeof new String('jonas')); // output: object

console.log(typeof new String('jonas').slice(1)); // output: string

```
- Whenever we call a method on a string, JavaScript will automatically behind the scenes convert that string primitive to a string object with the same content. And then it's on that object where the methods are called. All right and this process is called boxing because it basically takes our string and puts it into a box which is the object.

---

## Working With Strings Part 2
```js
///////////////////////////////////////
// Working With Strings - Part 2

const airline = 'TAP Air Portugal';

console.log(airline.toLowerCase()); // output: tap air portugal
console.log(airline.toUpperCase()); // output: TAP AIR PORTUGAL

// Fix capitalization in name
const passenger = 'jOnAS'; // Jonas
const passengerLower = passenger.toLowerCase();
const passengerCorrect =
  passengerLower[0].toUpperCase() + passengerLower.slice(1);
console.log(passengerCorrect); // output: Jonas

// Comparing emails
const email = 'hello@jonas.io';
const loginEmail = '  Hello@Jonas.Io \n';

// const lowerEmail = loginEmail.toLowerCase();
// const trimmedEmail = lowerEmail.trim();
const normalizedEmail = loginEmail.toLowerCase().trim();
console.log(normalizedEmail); // output: hello@jonas.io
console.log(email === normalizedEmail); // output: true

// replacing
const priceGB = '288,97£';
const priceUS = priceGB.replace('£', '$').replace(',', '.');
console.log(priceUS); // output: 288.97$

const announcement =
  'All passengers come to boarding door 23. Boarding door 23!';

console.log(announcement.replace('door', 'gate')); // output: All passengers come to boarding gate 23. Boarding door 23!
// console.log(announcement.replaceAll('door', 'gate'));
console.log(announcement.replace(/door/g, 'gate')); // output: All passengers come to boarding gate 23. Boarding gate 23!

// Booleans
const plane = 'Airbus A320neo';
console.log(plane.includes('A320')); // output: true
console.log(plane.includes('Boeing')); // output: false
console.log(plane.startsWith('Airb')); // output: true

if (plane.startsWith('Airbus') && plane.endsWith('neo')) {
  console.log('Part of the NEW ARirbus family'); // output: Part of the NEW ARirbus family
}

// Practice exercise
const checkBaggage = function (items) {
  const baggage = items.toLowerCase();

  if (baggage.includes('knife') || baggage.includes('gun')) {
    console.log('You are NOT allowed on board');
  } else {
    console.log('Welcome aboard!');
  }
};

checkBaggage('I have a laptop, some Food and a pocket Knife'); // output: You are NOT allowed on board
checkBaggage('Socks and camera'); // output: Welcome aboard!
checkBaggage('Got some snacks and a gun for protection'); // output: You are NOT allowed on board

```

---

## Working With Strings Part 3
```js
///////////////////////////////////////
// Working With Strings - Part 2

// Split and join
console.log('a+very+nice+string'.split('+')); // output: ['a', 'very', 'nice', 'string']
console.log('Jonas Schmedtmann'.split(' ')); // output: ['Jonas', 'Schmedtmann']

const [firstName, lastName] = 'Jonas Schmedtmann'.split(' ');

const newName = ['Mr.', firstName, lastName.toUpperCase()].join(' ');
console.log(newName); // output: Mr. Jonas SCHMEDTMANN

const capitalizeName = function (name) {
  const names = name.split(' ');
  const namesUpper = [];

  for (const n of names) {
    // namesUpper.push(n[0].toUpperCase() + n.slice(1));
    namesUpper.push(n.replace(n[0], n[0].toUpperCase()));
  }
  console.log(namesUpper.join(' '));
};

capitalizeName('jessica ann smith davis'); // output: Jessica Ann Smith Davis
capitalizeName('jonas schmedtmann'); // output:

// Padding
const message = 'Go to gate 23!';
console.log(message.padStart(20, '+').padEnd(30, '+')); // output: ++++++Go to gate 23!++++++++++
console.log('Jonas'.padStart(20, '+').padEnd(30, '+')); // output:+++++++++++++++Jonas++++++++++


const maskCreditCard = function (number) {
  const str = number + '';
  const last = str.slice(-4);
  return last.padStart(str.length, '*');
};

console.log(maskCreditCard(64637836)); // output: ****7836
console.log(maskCreditCard(43378463864647384)); // output: *************7384
console.log(maskCreditCard('334859493847755774747')); // output: *****************4747

// Repeat
const message2 = 'Bad waether... All Departues Delayed... ';
console.log(message2.repeat(5)); // output: Bad waether... All Departues Delayed... Bad waether... All Departues Delayed... Bad waether... All Departues Delayed... Bad waether... All Departues Delayed... Bad waether... All Departues Delayed...

const planesInLine = function (n) {
  console.log(`There are ${n} planes in line ${'🛩'.repeat(n)}`);
};
planesInLine(5); // output: There are 5 planes in line 🛩🛩🛩🛩🛩
planesInLine(3); // output: There are 3 planes in line 🛩🛩🛩
planesInLine(12); // output: There are 12 planes in line 🛩🛩🛩🛩🛩🛩🛩🛩🛩🛩🛩🛩

```

---

## Coding Challenge ##4
Write a program that receives a list of variable names written in underscore_case and convert them to camelCase.

The input will come from a textarea inserted into the DOM (see code below), and conversion will happen when the button is pressed.

THIS TEST DATA (pasted to textarea)
```js
underscore_case
	first_name
Some_Variable
	calculate_AGE
delayed_departure

```js
SHOULD PRODUCE THIS OUTPUT (5 separate console.log outputs)
```js
underscoreCase      ✅
firstName           ✅✅
someVariable        ✅✅✅
calculateAge        ✅✅✅✅
delayedDeparture    ✅✅✅✅✅

```
HINT 1: Remember which character defines a new line in the textarea 😉
HINT 2: The solution only needs to work for a variable made out of 2 words, like a_b
HINT 3: Start without worrying about the ✅. Tackle that only after you have the variable name conversion working 😉
HINT 4: This challenge is difficult on purpose, so start watching the solution in case you're stuck. Then pause and continue!

Afterwards, test with your own test data!

**Solution**
```js
document.body.append(document.createElement('textarea'));
document.body.append(document.createElement('button'));

document.querySelector('button').addEventListener('click', function () {
  const text = document.querySelector('textarea').value;
  const rows = text.split('\n');

  for (const [i, row] of rows.entries()) {
    const [first, second] = row.toLowerCase().trim().split('_');

    const output = `${first}${second.replace(
      second[0],
      second[0].toUpperCase()
    )}`;
    console.log(`${output.padEnd(20)}${'✅'.repeat(i + 1)}`);
  }
});

```

---

## String Methods Practice
```js
///////////////////////////////////////
// String Methods Practice

const flights = '_Delayed_Departure;fao93766109;txl2133758440;11:25+_Arrival;bru0943384722;fao93766109;11:45+_Delayed_Arrival;hel7439299980;fao93766109;12:05+_Departure;fao93766109;lis2323639855;12:30';

// 🔴 Delayed Departure from FAO to TXL (11h25)
//              Arrival from BRU to FAO (11h45)
//   🔴 Delayed Arrival from HEL to FAO (12h05)
//            Departure from FAO to LIS (12h30)

const getCode = str => str.slice(0, 3).toUpperCase();

for (const flight of flights.split('+')) {
  const [type, from, to, time] = flight.split(';');
  const output = `${type.startsWith('_Delayed') ? '🔴' : ''}${type.replaceAll(
    '_',
    ' '
  )} ${getCode(from)} ${getCode(to)} (${time.replace(':', 'h')})`.padStart(36);
  console.log(output);
}

```
---

## 📋 Cheat Sheet

| Pattern | Syntax |
|---------|--------|
| Array destructuring | `const [a, b] = arr` |
| Object destructuring | `const {a, b} = obj` |
| Spread (expand) | `const newArr = [...arr, x]` |
| Rest (collect) | `function fn(...args)` |
| Short-circuit OR | `value || default` |
| Short-circuit AND | `condition && render()` |
| Nullish coalescing | `value ?? default` (only null/undefined) |
| Optional chaining | `obj?.prop?.method?.()` |

> 💡 **Best Practice:** Use `??` instead of `||` when `0` or `''` are valid values you want to keep.


## 🧭 Navigation
← [📄 07. How JavaScript Works Behind the Scenes](<07. How JavaScript Works Behind the Scenes.md>) | [🏠 Home](<README.md>) | [📄 09. A Closer Look at Functions](<09. A Closer Look at Functions.md>) →

---

*Happy Coding! 🚀*