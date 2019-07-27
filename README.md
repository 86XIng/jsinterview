

---

###### 3. 输出是什么？

```javascript
const shape = {
  radius: 10,
  diameter() {
    return this.radius * 2
  },
  perimeter: () => 2 * Math.PI * this.radius
}

shape.diameter()
shape.perimeter()
```

- A: `20` and `62.83185307179586`
- B: `20` and `NaN`
- C: `20` and `63`
- D: `NaN` and `63`

<details><summary><b>答案</b></summary>
<p>

#### 答案: B

注意 `diameter` 的值是一个常规函数，但是 `perimeter` 的值是一个箭头函数。

对于箭头函数，`this` 关键字指向的是它当前周围作用域（简单来说是包含箭头函数的常规函数，如果没有常规函数的话就是全局对象），这个行为和常规函数不同。这意味着当我们调用 `perimeter` 时，`this` 不是指向 `shape` 对象，而是它的周围作用域（在例子中是 `window`）。

在 `window` 中没有 `radius` 这个属性，因此返回 `undefined`。

</p>
</details>

---

###### 5. 哪一个是无效的？

```javascript
const bird = {
  size: 'small'
}

const mouse = {
  name: 'Mickey',
  small: true
}
```

- A: `mouse.bird.size`
- B: `mouse[bird.size]`
- C: `mouse[bird["size"]]`
- D: All of them are valid

<details><summary><b>答案</b></summary>
<p>

#### 答案: A

在 JavaScript 中，所有对象的 keys 都是字符串（除非对象是 Symbol）。尽管我们可能不会定义它们为字符串，但它们在底层总会被转换为字符串。

当我们使用括号语法时（[]），JavaScript 会解释（或者 unboxes）语句。它首先看到第一个开始括号 `[` 并继续前进直到找到结束括号 `]`。只有这样，它才会计算语句的值。

`mouse[bird.size]`：首先计算 `bird.size`，这会得到 `small`。`mouse["small"]` 返回 `true`。

然后使用点语法的话，上面这一切都不会发生。`mouse` 没有 `bird` 这个 key，这也就意味着 `mouse.bird` 是 `undefined`。然后当我们使用点语法 `mouse.bird.size` 时，因为 `mouse.bird` 是 `undefined`，这也就变成了 `undefined.size`。这个行为是无效的，并且会抛出一个错误类似 `Cannot read property "size" of undefined`。

</p>
</details>

---


###### 8. 输出是什么？

```javascript
class Chameleon {
  static colorChange(newColor) {
    this.newColor = newColor
    return this.newColor
  }

  constructor({ newColor = 'green' } = {}) {
    this.newColor = newColor
  }
}

const freddie = new Chameleon({ newColor: 'purple' })
freddie.colorChange('orange')
```

- A: `orange`
- B: `purple`
- C: `green`
- D: `TypeError`

<details><summary><b>答案</b></summary>
<p>

#### 答案: D

`colorChange` 是一个静态方法。静态方法被设计为只能被创建它们的构造器使用（也就是 `Chameleon`），并且不能传递给实例。因为 `freddie` 是一个实例，静态方法不能被实例使用，因此抛出了 `TypeError` 错误。

</p>
</details>

---

###### 9. 输出是什么？

```javascript
let greeting
greetign = {} // Typo!
console.log(greetign)
```

- A: `{}`
- B: `ReferenceError: greetign is not defined`
- C: `undefined`

<details><summary><b>答案</b></summary>
<p>

#### 答案: A

代码打印出了一个对象，这是因为我们在全局对象上创建了一个空对象！当我们将 `greeting` 写错成 `greetign` 时，JS 解释器实际在上浏览器中将它视为 `global.greetign = {}` （或者 `window.greetign = {}`）。

为了避免这个为题，我们可以使用 `"use strict"。这能确保当你声明变量时必须赋值。

</p>
</details>

---

###### 10. 当我们这么做时，会发生什么？

```javascript
function bark() {
  console.log('Woof!')
}

bark.animal = 'dog'
```

- A: 正常运行!
- B: `SyntaxError`. 你不能通过这种方式给函数增加属性。
- C: `undefined`
- D: `ReferenceError`

<details><summary><b>答案</b></summary>
<p>

#### 答案: A

这在 JavaScript 中是可以的，因为函数是对象！（除了基本类型之外其他都是对象）

函数是一个特殊的对象。你写的这个代码其实不是一个实际的函数。函数是一个拥有属性的对象，并且属性也可被调用。

</p>
</details>

---

###### 11. 输出是什么？

```javascript
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const member = new Person("Lydia", "Hallie");
Person.getFullName = function () {
  return `${this.firstName} ${this.lastName}`;
}

console.log(member.getFullName());
```

- A: `TypeError`
- B: `SyntaxError`
- C: `Lydia Hallie`
- D: `undefined` `undefined`

<details><summary><b>答案</b></summary>
<p>

#### 答案: A

你不能像常规对象那样，给构造函数添加属性。如果你想一次性给所有实例添加特性，你应该使用原型。因此本例中，使用如下方式：

```js
Person.prototype.getFullName = function () {
  return `${this.firstName} ${this.lastName}`;
}
```

这才会使 `member.getFullName()` 起作用。为什么这么做有益的？假设我们将这个方法添加到构造函数本身里。也许不是每个 `Person` 实例都需要这个方法。这将浪费大量内存空间，因为它们仍然具有该属性，这将占用每个实例的内存空间。相反，如果我们只将它添加到原型中，那么它只存在于内存中的一个位置，但是所有实例都可以访问它！

</p>
</details>

---

###### 12. 输出是什么？

```javascript
function Person(firstName, lastName) {
  this.firstName = firstName
  this.lastName = lastName
}

const lydia = new Person('Lydia', 'Hallie')
const sarah = Person('Sarah', 'Smith')

console.log(lydia)
console.log(sarah)
```

- A: `Person {firstName: "Lydia", lastName: "Hallie"}` and `undefined`
- B: `Person {firstName: "Lydia", lastName: "Hallie"}` and `Person {firstName: "Sarah", lastName: "Smith"}`
- C: `Person {firstName: "Lydia", lastName: "Hallie"}` and `{}`
- D:`Person {firstName: "Lydia", lastName: "Hallie"}` and `ReferenceError`

<details><summary><b>答案</b></summary>
<p>

#### 答案: A

对于 `sarah`，我们没有使用 `new` 关键字。当使用 `new` 时，`this` 引用我们创建的空对象。当未使用 `new` 时，`this` 引用的是**全局对象**（global object）。

我们说 `this.firstName` 等于 `"Sarah"`，并且 `this.lastName` 等于 `"Smith"`。实际上我们做的是，定义了 `global.firstName = 'Sarah'` 和 `global.lastName = 'Smith'`。而 `sarah` 本身是 `undefined`。

</p>
</details>

---


###### 14. 所有对象都有原型。

- A: true
- B: false

<details><summary><b>答案</b></summary>
<p>

#### 答案: B

除了**基本对象**（base object），所有对象都有原型。基本对象可以访问一些方法和属性，比如 `.toString`。这就是为什么你可以使用内置的 JavaScript 方法！所有这些方法在原型上都是可用的。虽然 JavaScript 不能直接在对象上找到这些方法，但 JavaScript 会沿着原型链找到它们，以便于你使用。

</p>
</details>

---


###### 17. 输出是什么？

```javascript
function getPersonInfo(one, two, three) {
  console.log(one)
  console.log(two)
  console.log(three)
}

const person = 'Lydia'
const age = 21

getPersonInfo`${person} is ${age} years old`
```

- A: `"Lydia"` `21` `["", " is ", " years old"]`
- B: `["", " is ", " years old"]` `"Lydia"` `21`
- C: `"Lydia"` `["", " is ", " years old"]` `21`

<details><summary><b>答案</b></summary>
<p>

#### 答案: B

如果使用标记模板字面量，第一个参数的值总是包含字符串的数组。其余的参数获取的是传递的表达式的值！

</p>
</details>

---

###### 18. 输出是什么？

```javascript
function checkAge(data) {
  if (data === { age: 18 }) {
    console.log('You are an adult!')
  } else if (data == { age: 18 }) {
    console.log('You are still an adult.')
  } else {
    console.log(`Hmm.. You don't have an age I guess`)
  }
}

checkAge({ age: 18 })
```

- A: `You are an adult!`
- B: `You are still an adult.`
- C: `Hmm.. You don't have an age I guess`

<details><summary><b>答案</b></summary>
<p>

#### 答案: C

在测试相等性时，基本类型通过它们的值（value）进行比较，而对象通过它们的引用（reference）进行比较。JavaScript 检查对象是否具有对内存中相同位置的引用。

题目中我们正在比较的两个对象不是同一个引用：作为参数传递的对象引用的内存位置，与用于判断相等的对象所引用的内存位置并不同。

这也是 `{ age: 18 } === { age: 18 }` 和 `{ age: 18 } == { age: 18 }` 都返回 `false` 的原因。

</p>
</details>

---

###### 19. 输出是什么？

```javascript
function getAge(...args) {
  console.log(typeof args)
}

getAge(21)
```

- A: `"number"`
- B: `"array"`
- C: `"object"`
- D: `"NaN"`

<details><summary><b>答案</b></summary>
<p>

#### 答案: C

扩展运算符（`...args`）会返回实参组成的数组。而数组是对象，因此 `typeof args` 返回 `"object"`。

</p>
</details>

---

###### 20. 输出是什么？

```javascript
function getAge() {
  'use strict'
  age = 21
  console.log(age)
}

getAge()
```

- A: `21`
- B: `undefined`
- C: `ReferenceError`
- D: `TypeError`

<details><summary><b>答案</b></summary>
<p>

#### 答案: C

使用 `"use strict"`，你可以确保不会意外地声明全局变量。我们从来没有声明变量 `age`，因为我们使用 `"use strict"`，它将抛出一个引用错误。如果我们不使用 `"use strict"`，它就会工作，因为属性 `age` 会被添加到全局对象中了。

</p>
</details>

---



###### 24. 输出是什么？

```javascript
const obj = { 1: 'a', 2: 'b', 3: 'c' }
const set = new Set([1, 2, 3, 4, 5])

obj.hasOwnProperty('1')
obj.hasOwnProperty(1)
set.has('1')
set.has(1)
```

- A: `false` `true` `false` `true`
- B: `false` `true` `true` `true`
- C: `true` `true` `false` `true`
- D: `true` `true` `true` `true`

<details><summary><b>答案</b></summary>
<p>

#### 答案: C

所有对象的键（不包括 Symbol）在底层都是字符串，即使你自己没有将其作为字符串输入。这就是为什么 `obj.hasOwnProperty('1')` 也返回 `true`。

对于集合，它不是这样工作的。在我们的集合中没有 `'1'`：`set.has('1')` 返回 `false`。它有数字类型为 `1`，`set.has(1)` 返回 `true`。

</p>
</details>

---

###### 25. 输出是什么？

```javascript
const obj = { a: 'one', b: 'two', a: 'three' }
console.log(obj)
```

- A: `{ a: "one", b: "two" }`
- B: `{ b: "two", a: "three" }`
- C: `{ a: "three", b: "two" }`
- D: `SyntaxError`

<details><summary><b>答案</b></summary>
<p>

#### 答案: C

如果你有两个名称相同的键，则键会被替换掉。它仍然位于第一个键出现的位置，但是值是最后出现那个键的值。

</p>
</details>

---


###### 28. 输出是什么？

```javascript
String.prototype.giveLydiaPizza = () => {
  return 'Just give Lydia pizza already!'
}

const name = 'Lydia'

name.giveLydiaPizza()
```

- A: `"Just give Lydia pizza already!"`
- B: `TypeError: not a function`
- C: `SyntaxError`
- D: `undefined`

<details><summary><b>答案</b></summary>
<p>

#### 答案: A

`String` 是内置的构造函数，我们可以向它添加属性。我只是在它的原型中添加了一个方法。基本类型字符串被自动转换为字符串对象，由字符串原型函数生成。因此，所有 string(string 对象)都可以访问该方法！

</p>
</details>

---

###### 29. 输出是什么？

```javascript
const a = {}
const b = { key: 'b' }
const c = { key: 'c' }

a[b] = 123
a[c] = 456

console.log(a[b])
```

- A: `123`
- B: `456`
- C: `undefined`
- D: `ReferenceError`

<details><summary><b>答案</b></summary>
<p>

#### 答案: B

对象的键被自动转换为字符串。我们试图将一个对象 `b` 设置为对象 `a` 的键，且相应的值为 `123`。

然而，当字符串化一个对象时，它会变成 `"[object Object]"`。因此这里说的是，`a["[object Object]"] = 123`。然后，我们再一次做了同样的事情，`c` 是另外一个对象，这里也有隐式字符串化，于是，`a["[object Object]"] = 456`。

然后，我们打印 `a[b]`，也就是 `a["[object Object]"]`。之前刚设置为 `456`，因此返回的是 `456`。

</p>
</details>

---


###### 31. 当点击按钮时，event.target是什么？

```html
<div onclick="console.log('first div')">
  <div onclick="console.log('second div')">
    <button onclick="console.log('button')">
      Click!
    </button>
  </div>
</div>
```

- A: Outer `div`
- B: Inner `div`
- C: `button`
- D: 一个包含所有嵌套元素的数组。

<details><summary><b>答案</b></summary>
<p>

#### 答案: C

导致事件的最深嵌套的元素是事件的 target。你可以通过 `event.stopPropagation` 来停止冒泡。

</p>
</details>

---


###### 33. 输出是什么？

```javascript
const person = { name: 'Lydia' }

function sayHi(age) {
  console.log(`${this.name} is ${age}`)
}

sayHi.call(person, 21)
sayHi.bind(person, 21)
```

- A: `undefined is 21` `Lydia is 21`
- B: `function` `function`
- C: `Lydia is 21` `Lydia is 21`
- D: `Lydia is 21` `function`

<details><summary><b>答案</b></summary>
<p>

#### 答案: D

使用这两种方法，我们都可以传递我们希望 `this` 关键字引用的对象。但是，`.call` 是**立即执行**的。

`.bind` 返回函数的**副本**，但带有绑定上下文！它不是立即执行的。

</p>
</details>

---

###### 34. 输出是什么？

```javascript
function sayHi() {
  return (() => 0)()
}

typeof sayHi()
```

- A: `"object"`
- B: `"number"`
- C: `"function"`
- D: `"undefined"`

<details><summary><b>答案</b></summary>
<p>

#### 答案: B

`sayHi` 方法返回的是立即执行函数(IIFE)的返回值.此立即执行函数的返回值是 `0`， 类型是 `number`

参考：只有7种内置类型：`null`，`undefined`，`boolean`，`number`，`string`，`object` 和 `symbol`。 ``function`` 不是一种类型，函数是对象，它的类型是``object``。

</p>
</details>

---

###### 36. 输出是什么？

```javascript
console.log(typeof typeof 1)
```

- A: `"number"`
- B: `"string"`
- C: `"object"`
- D: `"undefined"`

<details><summary><b>答案</b></summary>
<p>

#### 答案: B

`typeof 1` 返回 `"number"`。
`typeof "number"` 返回 `"string"`。

</p>
</details>

---

###### 37. 输出是什么？

```javascript
const numbers = [1, 2, 3]
numbers[10] = 11
console.log(numbers)
```

- A: `[1, 2, 3, 7 x null, 11]`
- B: `[1, 2, 3, 11]`
- C: `[1, 2, 3, 7 x empty, 11]`
- D: `SyntaxError`

<details><summary><b>答案</b></summary>
<p>

#### 答案: C

当你为数组设置超过数组长度的值的时候， JavaScript 会创建名为 "empty slots" 的东西。它们的值实际上是 `undefined`。你会看到以下场景：

`[1, 2, 3, 7 x empty, 11]`

这取决于你的运行环境（每个浏览器，以及 node 环境，都有可能不同）

</p>
</details>

---

###### 38. 输出是什么？

```javascript
(() => {
  let x, y
  try {
    throw new Error()
  } catch (x) {
    (x = 1), (y = 2)
    console.log(x)
  }
  console.log(x)
  console.log(y)
})()
```

- A: `1` `undefined` `2`
- B: `undefined` `undefined` `undefined`
- C: `1` `1` `2`
- D: `1` `undefined` `undefined`

<details><summary><b>答案</b></summary>
<p>

#### 答案: A

`catch` 代码块接收参数 `x`。当我们传递参数时，这与之前定义的变量 `x` 不同 。这个 `x` 是属于 `catch` 块级作用域的。

然后，我们将块级作用域中的变量赋值为 `1`，同时也设置了变量 `y` 的值。现在，我们打印块级作用域中的变量 `x`，值为 `1`。

`catch` 块之外的变量 `x` 的值仍为 `undefined`， `y` 的值为 `2`。当我们在 `catch` 块之外执行 `console.log(x)` 时，返回 `undefined`，`y` 返回 `2`。

</p>
</details>

---


###### 40. 输出是什么？

```javascript
[[0, 1], [2, 3]].reduce(
  (acc, cur) => {
    return acc.concat(cur)
  },
  [1, 2]
)
```

- A: `[0, 1, 2, 3, 1, 2]`
- B: `[6, 1, 2]`
- C: `[1, 2, 0, 1, 2, 3]`
- D: `[1, 2, 6]`

<details><summary><b>答案</b></summary>
<p>

#### 答案: C

`[1, 2]`是初始值。初始值将会作为首次调用时第一个参数 `acc` 的值。在第一次执行时， `acc` 的值是 `[1, 2]`， `cur` 的值是 `[0, 1]`。合并它们，结果为 `[1, 2, 0, 1]`。
第二次执行， `acc` 的值是 `[1, 2, 0, 1]`， `cur` 的值是 `[2, 3]`。合并它们，最终结果为 `[1, 2, 0, 1, 2, 3]`

</p>
</details>

---


###### 44. 输出是什么?

```javascript
function* generator(i) {
  yield i;
  yield i * 2;
}

const gen = generator(10);

console.log(gen.next().value);
console.log(gen.next().value);
```

- A: `[0, 10], [10, 20]`
- B: `20, 20`
- C: `10, 20`
- D: `0, 10 and 10, 20`

<details><summary><b>答案</b></summary>
<p>

#### 答案: C

一般的函数在执行之后是不能中途停下的。但是，生成器函数却可以中途“停下”，之后可以再从停下的地方继续。当生成器遇到`yield`关键字的时候，会生成`yield`后面的值。注意，生成器在这种情况下不 _返回_ (_return_ )值，而是 _生成_ (_yield_)值。

首先，我们用`10`作为参数`i`来初始化生成器函数。然后使用`next()`方法一步步执行生成器。第一次执行生成器的时候，`i`的值为`10`，遇到第一个`yield`关键字，它要生成`i`的值。此时，生成器“暂停”，生成了`10`。

然后，我们再执行`next()`方法。生成器会从刚才暂停的地方继续，这个时候`i`还是`10`。于是我们走到了第二个`yield`关键字处，这时候需要生成的值是`i*2`，`i`为`10`，那么此时生成的值便是`20`。所以这道题的最终结果是`10,20`。


</p>
</details>

###### 45. 返回值是什么?

```javascript
const firstPromise = new Promise((res, rej) => {
  setTimeout(res, 500, "one");
});

const secondPromise = new Promise((res, rej) => {
  setTimeout(res, 100, "two");
});

Promise.race([firstPromise, secondPromise]).then(res => console.log(res));
```

- A: `"one"`
- B: `"two"`
- C: `"two" "one"`
- D: `"one" "two"`

<details><summary><b>答案</b></summary>
<p>

#### 答案: B

当我们向`Promise.race`方法中传入多个`Promise`时，会进行 _优先_ 解析。在这个例子中，我们用`setTimeout`给`firstPromise`和`secondPromise`分别设定了500ms和100ms的定时器。这意味着`secondPromise`会首先解析出字符串`two`。那么此时`res`参数即为`two`，是为输出结果。

</p>
</details>


---

###### 46. 输出是什么?

```javascript
let person = { name: "Lydia" };
const members = [person];
person = null;

console.log(members);
```

- A: `null`
- B: `[null]`
- C: `[{}]`
- D: `[{ name: "Lydia" }]`

<details><summary><b>答案</b></summary>
<p>

#### 答案: D


首先我们声明了一个拥有`name`属性的对象 `person`。

<img src="https://i.imgur.com/TML1MbS.png" width="200">

然后我们又声明了一个变量`members`. 将首个元素赋值为变量`person`。 当设置两个对象彼此相等时，它们会通过 _引用_ 进行交互。但是当你将引用从一个变量分配至另一个变量时，其实只是执行了一个 _复制_ 操作。（注意一点，他们的引用 _并不相同_!）

<img src="https://i.imgur.com/FSG5K3F.png" width="300">

接下来我们让`person`等于`null`。

<img src="https://i.imgur.com/sYjcsMT.png" width="300">

我们没有修改数组第一个元素的值，而只是修改了变量`person`的值,因为元素（复制而来）的引用与`person`不同。`members`的第一个元素仍然保持着对原始对象的引用。当我们输出`members`数组时，第一个元素会将引用的对象打印出来。

</p>
</details>

---


###### 49. `num`的值是什么?

```javascript
const num = parseInt("7*6", 10);
```

- A: `42`
- B: `"42"`
- C: `7`
- D: `NaN`

<details><summary><b>答案</b></summary>
<p>

#### 答案: C

只返回了字符串中第一个字母. 设定了 _进制_ 后 (也就是第二个参数，指定需要解析的数字是什么进制: 十进制、十六机制、八进制、二进制等等……),`parseInt` 检查字符串中的字符是否合法. 一旦遇到一个在指定进制中不合法的字符后，立即停止解析并且忽略后面所有的字符。

`*`就是不合法的数字字符。所以只解析到`"7"`，并将其解析为十进制的`7`. `num`的值即为`7`.

</p>
</details>

---

###### 50. 输出是什么?

```javascript
[1, 2, 3].map(num => {
  if (typeof num === "number") return;
  return num * 2;
});
```

- A: `[]`
- B: `[null, null, null]`
- C: `[undefined, undefined, undefined]`
- D: `[ 3 x empty ]`

<details><summary><b>答案</b></summary>
<p>

#### 答案: C

对数组进行映射的时候,`num`就是当前循环到的元素. 在这个例子中，所有的映射都是number类型，所以if中的判断`typeof num === "number"`结果都是`true`.map函数创建了新数组并且将函数的返回值插入数组。

但是，没有任何值返回。当函数没有返回任何值时，即默认返回`undefined`.对数组中的每一个元素来说，函数块都得到了这个返回值，所以结果中每一个元素都是`undefined`.

</p>
</details>

---

###### 51. 输出的是什么?

```javascript
function getInfo(member, year) {
  member.name = "Lydia";
  year = "1998";
}

const person = { name: "Sarah" };
const birthYear = "1997";

getInfo(person, birthYear);

console.log(person, birthYear);
```

- A: `{ name: "Lydia" }, "1997"`
- B: `{ name: "Sarah" }, "1998"`
- C: `{ name: "Lydia" }, "1998"`
- D: `{ name: "Sarah" }, "1997"`

<details><summary><b>答案</b></summary>
<p>

#### 答案: A

普通参数都是 _值_ 传递的，而对象则不同，是 _引用_ 传递。所以说，`birthYear`是值传递，因为他是个字符串而不是对象。当我们对参数进行值传递时，会创建一份该值的 _复制_ 。（可以参考问题46）

变量`birthYear`有一个对`"1997"`的引用，而传入的参数也有一个对`"1997"`的引用，但二者的引用并不相同。当我们通过给 `year`赋值`"1998"`来更新`year`的值的时候我们只是更新了`year`（的引用）。此时`birthYear`仍然是`"1997"`.

而`person`是个对象。参数`member`引用与之 _相同的_ 对象。当我们修改`member`所引用对象的属性时,`person`的相应属性也被修改了,因为他们引用了相同的对象. `person`的 `name`属性也变成了 `"Lydia"`.

</p>
</details>


---

###### 53. 输出是什么?

```javascript
function Car() {
  this.make = "Lamborghini";
  return { make: "Maserati" };
}

const myCar = new Car();
console.log(myCar.make);
```

- A: `"Lamborghini"`
- B: `"Maserati"`
- C: `ReferenceError`
- D: `TypeError`

<details><summary><b>答案</b></summary>
<p>

#### 答案: B

返回属性的时候，属性的值等于 _返回的_ 值，而不是构造函数中设定的值。我们返回了字符串 `"Maserati"`，所以 `myCar.make`等于`"Maserati"`.

</p>
</details>

---

###### 54. 输出是什么?

```javascript
(() => {
  let x = (y = 10);
})();

console.log(typeof x);
console.log(typeof y);
```

- A: `"undefined", "number"`
- B: `"number", "number"`
- C: `"object", "number"`
- D: `"number", "undefined"`

<details><summary><b>答案</b></summary>
<p>

#### 答案: A

`let x = y = 10;` 是下面这个表达式的缩写:

```javascript
y = 10;
let x = y;
```

我们设定`y`等于`10`时,我们实际上增加了一个属性`y`给全局对象(浏览器里的`window`, Nodejs里的`global`)。在浏览器中， `window.y`等于`10`.

然后我们声明了变量`x`等于`y`,也是`10`.但变量是使用 `let`声明的，它只作用于 _块级作用域_, 仅在声明它的块中有效；就是案例中的立即调用表达式(IIFE)。使用`typeof`操作符时, 操作值 `x`没有被定义：因为我们在`x`声明块的外部，无法调用它。这就意味着`x`未定义。未分配或是未声明的变量类型为`"undefined"`. `console.log(typeof x)`返回`"undefined"`.

而我们创建了全局变量`y`，并且设定`y`等于`10`.这个值在我们的代码各处都访问的到。 `y`已经被定义了，而且有一个`"number"`类型的值。 `console.log(typeof y)`返回`"number"`.

</p>
</details>

---

###### <a name=20190629></a>55. 输出是什么?

```javascript
class Dog {
  constructor(name) {
    this.name = name;
  }
}

Dog.prototype.bark = function() {
  console.log(`Woof I am ${this.name}`);
};

const pet = new Dog("Mara");

pet.bark();

delete Dog.prototype.bark;

pet.bark();
```

- A: `"Woof I am Mara"`, `TypeError`
- B: `"Woof I am Mara"`,`"Woof I am Mara"`
- C: `"Woof I am Mara"`, `undefined`
- D: `TypeError`, `TypeError`

<details><summary><b>答案</b></summary>
<p>

#### 答案: A

我们可以用`delete`关键字删除对象的属性，对原型也是适用的。删除了原型的属性后，该属性在原型链上就不可用了。在本例中，函数`bark`在执行了`delete Dog.prototype.bark`后不可用, 然而后面的代码还在调用它。

当我们尝试调用一个不存在的函数时`TypeError`异常会被抛出。在本例中就是 `TypeError: pet.bark is not a function`，因为`pet.bark`是`undefined`.

</p>
</details>

---

###### 56. 输出是什么?

```javascript
const set = new Set([1, 1, 2, 3, 4]);

console.log(set);
```

- A: `[1, 1, 2, 3, 4]`
- B: `[1, 2, 3, 4]`
- C: `{1, 1, 2, 3, 4}`
- D: `{1, 2, 3, 4}`

<details><summary><b>答案</b></summary>
<p>

#### 答案: D

`Set`对象手机 _独一无二_ 的值：也就是说同一个值在其中仅出现一次。

我们传入了数组`[1, 1, 2, 3, 4]`，他有一个重复值`1`.以为一个集合里不能有两个重复的值，其中一个就被移除了。所以结果是 `{1, 2, 3, 4}`.

</p>
</details>

---

###### 57. 输出是什么?

```javascript
// counter.js
let counter = 10;
export default counter;
```

```javascript
// index.js
import myCounter from "./counter";

myCounter += 1;

console.log(myCounter);
```

- A: `10`
- B: `11`
- C: `Error`
- D: `NaN`

<details><summary><b>答案</b></summary>
<p>

#### 答案: C

引入的模块是 _只读_ 的: 你不能修改引入的模块。只有导出他们的模块才能修改其值。

当我们给`myCounter`增加一个值的时候会抛出一个异常： `myCounter`是只读的，不能被修改。

</p>
</details>

---

###### 58. 输出是什么?

```javascript
const name = "Lydia";
age = 21;

console.log(delete name);
console.log(delete age);
```

- A: `false`, `true`
- B: `"Lydia"`, `21`
- C: `true`, `true`
- D: `undefined`, `undefined`

<details><summary><b>答案</b></summary>
<p>

#### 答案: A

`delete`操作符返回一个布尔值： `true`指删除成功，否则返回`false`. 但是通过 `var`, `const` 或 `let` 关键字声明的变量无法用 `delete` 操作符来删除。

`name`变量由`const`关键字声明，所以删除不成功:返回 `false`. 而我们设定`age`等于`21`时,我们实际上添加了一个名为`age`的属性给全局对象。对象中的属性是可以删除的，全局对象也是如此，所以`delete age`返回`true`.

</p>
</details>

---

###### 59. 输出是什么?

```javascript
const numbers = [1, 2, 3, 4, 5];
const [y] = numbers;

console.log(y);
```

- A: `[[1, 2, 3, 4, 5]]`
- B: `[1, 2, 3, 4, 5]`
- C: `1`
- D: `[1]`

<details><summary><b>答案</b></summary>
<p>

#### 答案: C

我们可以通过解构赋值来解析来自对象的数组或属性的值，比如说：

```javascript
[a, b] = [1, 2];
```

<img src="https://i.imgur.com/ADFpVop.png" width="200">

`a`的值现在是`1`，`b`的值现在是`2`.而在题目中，我们是这么做的:

```javascript
[y] = [1, 2, 3, 4, 5];
```

<img src="https://i.imgur.com/NzGkMNk.png" width="200">

也就是说，`y`等于数组的第一个值就是数字`1`.我们输出`y`， 返回`1`.

</p>
</details>

---

###### 60. 输出是什么?

```javascript
const user = { name: "Lydia", age: 21 };
const admin = { admin: true, ...user };

console.log(admin);
```

- A: `{ admin: true, user: { name: "Lydia", age: 21 } }`
- B: `{ admin: true, name: "Lydia", age: 21 }`
- C: `{ admin: true, user: ["Lydia", 21] }`
- D: `{ admin: true }`

<details><summary><b>答案</b></summary>
<p>

#### 答案: B

扩展运算符`...`为对象的组合提供了可能。你可以复制对象中的键值对，然后把它们加到另一个对象里去。在本例中，我们复制了`user`对象键值对，然后把它们加入到`admin`对象中。`admin`对象就拥有了这些键值对，所以结果为`{ admin: true, name: "Lydia", age: 21 }`.

</p>
</details>

---

###### 61. 输出是什么?

```javascript
const person = { name: "Lydia" };

Object.defineProperty(person, "age", { value: 21 });

console.log(person);
console.log(Object.keys(person));
```

- A: `{ name: "Lydia", age: 21 }`, `["name", "age"]`
- B: `{ name: "Lydia", age: 21 }`, `["name"]`
- C: `{ name: "Lydia"}`, `["name", "age"]`
- D: `{ name: "Lydia"}`, `["age"]`

<details><summary><b>答案</b></summary>
<p>

#### 答案: B

通过`defineProperty`方法，我们可以给对象添加一个新属性，或者修改已经存在的属性。而我们使用`defineProperty`方法给对象添加了一个属性之后，属性默认为 _不可枚举(not enumerable)_. `Object.keys`方法仅返回对象中 _可枚举(enumerable)_ 的属性，因此只剩下了`"name"`.

用`defineProperty`方法添加的属性默认不可变。你可以通过`writable`, `configurable` 和 `enumerable`属性来改变这一行为。这样的话， 相比于自己添加的属性，`defineProperty`方法添加的属性有了更多的控制权。

</p>
</details>

---

###### 62. 输出是什么?

```javascript
const settings = {
  username: "lydiahallie",
  level: 19,
  health: 90
};

const data = JSON.stringify(settings, ["level", "health"]);
console.log(data);
```

- A: `"{"level":19, "health":90}"`
- B: `"{"username": "lydiahallie"}"`
- C: `"["level", "health"]"`
- D: `"{"username": "lydiahallie", "level":19, "health":90}"`

<details><summary><b>答案</b></summary>
<p>

#### 答案: A

`JSON.stringify`的第二个参数是 _替代者(replacer)_. 替代者(replacer)可以是个函数或数组，用以控制哪些值如何被转换为字符串。

如果替代者(replacer)是个 _数组_ ，那么就只有包含在数组中的属性将会被转化为字符串。在本例中，只有名为`"level"` 和 `"health"` 的属性被包括进来， `"username"`则被排除在外。 `data` 就等于 `"{"level":19, "health":90}"`.

而如果替代者(replacer)是个 _函数_，这个函数将被对象的每个属性都调用一遍。
函数返回的值会成为这个属性的值，最终体现在转化后的JSON字符串中（译者注：Chrome下，经过实验，如果所有属性均返回同一个值的时候有异常，会直接将返回值作为结果输出而不会输出JSON字符串），而如果返回值为`undefined`，则该属性会被排除在外。

</p>
</details>

---



###### 64. 输出什么?

```javascript
const value = { number: 10 };

const multiply = (x = { ...value }) => {
  console.log(x.number *= 2);
};

multiply();
multiply();
multiply(value);
multiply(value);
```

- A: `20`, `40`, `80`, `160`
- B: `20`, `40`, `20`, `40`
- C: `20`, `20`, `20`, `40`
- D: `NaN`, `NaN`, `20`, `40`

<details><summary><b>答案</b></summary>
<p>

#### 答案: C

在ES6中，我们可以使用默认值初始化参数。如果没有给函数传参，或者传的参值为 `"undefined"` ，那么参数的值将是默认值。上述例子中，我们将 `value` 对象进行了解构并传到一个新对象中，因此 `x` 的默认值为 `{number：10}` 。

默认参数在调用时才会进行计算，每次调用函数时，都会创建一个新的对象。我们前两次调用 `multiply` 函数且不传递值，那么每一次 `x` 的默认值都为 `{number：10}` ，因此打印出该数字的乘积值为`20`。

第三次调用 `multiply` 时，我们传递了一个参数，即对象`value`。 `*=`运算符实际上是`x.number = x.number * 2`的简写，我们修改了`x.number`的值，并打印出值`20`。

第四次，我们再次传递`value`对象。 `x.number`之前被修改为`20`，所以`x.number * = 2`打印为`40`。

</p>
</details>

---

###### 65. 输出什么?

```javascript
[1, 2, 3, 4].reduce((x, y) => console.log(x, y));
```

- A: `1` `2` and `3` `3` and `6` `4`
- B: `1` `2` and `2` `3` and `3` `4`
- C: `1` `undefined` and `2` `undefined` and `3` `undefined` and `4` `undefined`
- D: `1` `2` and `undefined` `3` and `undefined` `4`

<details><summary><b>答案</b></summary>
<p>

#### 答案: D

`reducer` 函数接收4个参数:

1. Accumulator (acc) (累计器)
2. Current Value (cur) (当前值)
3. Current Index (idx) (当前索引)
4. Source Array (src) (源数组)

`reducer` 函数的返回值将会分配给累计器，该返回值在数组的每个迭代中被记住，并最后成为最终的单个结果值。

`reducer` 函数还有一个可选参数`initialValue`, 该参数将作为第一次调用回调函数时的第一个参数的值。如果没有提供`initialValue`，则将使用数组中的第一个元素。

在上述例子，`reduce`方法接收的第一个参数(Accumulator)是`x`, 第二个参数(Current Value)是`y`。
 
在第一次调用时，累加器`x`为`1`，当前值`“y”`为`2`，打印出累加器和当前值：`1`和`2`。

例子中我们的回调函数没有返回任何值，只是打印累加器的值和当前值。如果函数没有返回值，则默认返回`undefined`。 在下一次调用时，累加器为`undefined`，当前值为“3”, 因此`undefined`和`3`被打印出。

在第四次调用时，回调函数依然没有返回值。 累加器再次为 `undefined` ，当前值为“4”。 `undefined`和`4`被打印出。
</p>
</details>
  
---



###### 67. 输出什么?

```javascript
// index.js
console.log('running index.js');
import { sum } from './sum.js';
console.log(sum(1, 2));

// sum.js
console.log('running sum.js');
export const sum = (a, b) => a + b;
```

- A: `running index.js`, `running sum.js`, `3`
- B: `running sum.js`, `running index.js`, `3`
- C: `running sum.js`, `3`, `running index.js`
- D: `running index.js`, `undefined`, `running sum.js`

<details><summary><b>答案</b></summary>
<p>

#### 答案: B

`import`命令是编译阶段执行的，在代码运行之前。因此这意味着被导入的模块会先运行，而导入模块的文件会后执行。

这是CommonJS中`require（）`和`import`之间的区别。使用`require()`，您可以在运行代码时根据需要加载依赖项。 如果我们使用`require`而不是`import`，`running index.js`，`running sum.js`，`3`会被依次打印。

</p>
</details>

---

###### 68. 输出什么?

```javascript
console.log(Number(2) === Number(2))
console.log(Boolean(false) === Boolean(false))
console.log(Symbol('foo') === Symbol('foo'))
```

- A: `true`, `true`, `false`
- B: `false`, `true`, `false`
- C: `true`, `false`, `true`
- D: `true`, `true`, `true`

<details><summary><b>答案</b></summary>
<p>

#### 答案: A

每个`Symbol`都是完全唯一的。传递给`Symbol`的参数只是给`Symbol`的一个描述。 `Symbol`的值不依赖于传递的参数。 当我们测试相等时，我们创建了两个全新的符号：第一个`Symbol（'foo'）`，第二个`Symbol（'foo'）`, 这两个值是唯一的，彼此不相等，因此返回`false`。

</p>
</details>

---

###### 69. 输出什么?

```javascript
const name = "Lydia Hallie"
console.log(name.padStart(13))
console.log(name.padStart(2))
```

- A: `"Lydia Hallie"`, `"Lydia Hallie"`
- B: `"           Lydia Hallie"`, `"  Lydia Hallie"` (`"[13x whitespace]Lydia Hallie"`, `"[2x whitespace]Lydia Hallie"`)
- C: `" Lydia Hallie"`, `"Lydia Hallie"` (`"[1x whitespace]Lydia Hallie"`, `"Lydia Hallie"`)
- D: `"Lydia Hallie"`, `"Lyd"`

<details><summary><b>答案</b></summary>
<p>

#### 答案: C

使用`padStart`方法，我们可以在字符串的开头添加填充。传递给此方法的参数是字符串的总长度（包含填充）。字符串`Lydia Hallie`的长度为`12`, 因此`name.padStart（13）`在字符串的开头只会插入1（`13 - 12 = 1`）个空格。

如果传递给`padStart`方法的参数小于字符串的长度，则不会添加填充。

</p>
</details>

---

###### 70. 输出什么?

```javascript
console.log("🥑" + "💻");
```

- A: `"🥑💻"`
- B: `257548`
- C: A string containing their code points
- D: Error

<details><summary><b>答案</b></summary>
<p>

#### 答案: A

使用`+`运算符，您可以连接字符串。 上述情况，我们将字符串`“🥑”`与字符串`”💻“`连接起来，产生`”🥑💻“`。

</p>
</details>

---

###### 71. 如何能打印出`console.log`语句后注释掉的值？

```javascript
function* startGame() {
  const answer = yield "Do you love JavaScript?";
  if (answer !== "Yes") {
    return "Oh wow... Guess we're gone here";
  }
  return "JavaScript loves you back ❤️";
}

const game = startGame();
console.log(/* 1 */); // Do you love JavaScript?
console.log(/* 2 */); // JavaScript loves you back ❤️
```

- A: `game.next("Yes").value` and `game.next().value`
- B: `game.next.value("Yes")` and `game.next.value()`
- C: `game.next().value` and `game.next("Yes").value`
- D: `game.next.value()` and `game.next.value("Yes")`

<details><summary><b>答案</b></summary>
<p>

#### 答案: C

`generator`函数在遇到`yield`关键字时会“暂停”其执行。 首先，我们需要让函数产生字符串`Do you love JavaScript?`，这可以通过调用`game.next().value`来完成。上述函数的第一行就有一个`yield`关键字，那么运行立即停止了，`yield`表达式本身没有返回值，或者说总是返回`undefined`, 这意味着此时变量 `answer` 为`undefined`

`next`方法可以带一个参数，该参数会被当作上一个 `yield` 表达式的返回值。当我们调用`game.next("Yes").value`时，先前的 `yield` 的返回值将被替换为传递给`next()`函数的参数`"Yes"`。此时变量 `answer` 被赋值为 `"Yes"`，`if`语句返回`false`，所以`JavaScript loves you back ❤️`被打印。

</p>
</details>

---

###### 72. 输出什么?

```javascript
console.log(String.raw`Hello\nworld`);
```

- A: `Hello world!`
- B: `Hello` <br />&nbsp; &nbsp; &nbsp;`world`
- C: `Hello\nworld`
- D: `Hello\n` <br /> &nbsp; &nbsp; &nbsp;`world`

<details><summary><b>答案</b></summary>
<p>

#### 答案: C

`String.raw`函数是用来获取一个模板字符串的原始字符串的，它返回一个字符串，其中忽略了转义符（`\n`，`\v`，`\t`等）。但反斜杠可能造成问题，因为你可能会遇到下面这种类似情况：

```javascript
const path = `C:\Documents\Projects\table.html`
String.raw`${path}`
```

这将导致：

`"C:DocumentsProjects able.html"`

直接使用`String.raw`
```javascript
String.raw`C:\Documents\Projects\table.html`
```
它会忽略转义字符并打印：`C:\Documents\Projects\table.html`

上述情况，字符串是`Hello\nworld`被打印出。

</p>
</details>

---

###### 73. 输出什么?

```javascript
async function getData() {
  return await Promise.resolve("I made it!");
}

const data = getData();
console.log(data);
```

- A: `"I made it!"`
- B: `Promise {<resolved>: "I made it!"}`
- C: `Promise {<pending>}`
- D: `undefined`

<details><summary><b>答案</b></summary>
<p>

#### 答案: C

异步函数始终返回一个promise。`await`仍然需要等待promise的解决：当我们调用`getData()`并将其赋值给`data`，此时`data`为`getData`方法返回的一个挂起的promise，该promise并没有解决。

如果我们想要访问已解决的值`"I made it!"`，可以在`data`上使用`.then()`方法：

`data.then(res => console.log(res))`

这样将打印 `"I made it!"`

</p>
</details>

---

###### 74. 输出什么?

```javascript
function addToList(item, list) {
  return list.push(item);
}

const result = addToList("apple", ["banana"]);
console.log(result);
```

- A: `['apple', 'banana']`
- B: `2`
- C: `true`
- D: `undefined`

<details><summary><b>答案</b></summary>
<p>

#### 答案: B

`push()`方法返回新数组的长度。一开始，数组包含一个元素（字符串`"banana"`），长度为1。 在数组中添加字符串`"apple"`后，长度变为2，并将从`addToList`函数返回。

`push`方法修改原始数组，如果你想从函数返回数组而不是数组长度，那么应该在push `item`之后返回`list`。

</p>
</details>

---

###### 75. 输出什么?

```javascript
const box = { x: 10, y: 20 };

Object.freeze(box);

const shape = box;
shape.x = 100;
console.log(shape)
```

- A: `{ x: 100, y: 20 }`
- B: `{ x: 10, y: 20 }`
- C: `{ x: 100 }`
- D: `ReferenceError`

<details><summary><b>答案</b></summary>
<p>

#### 答案: B

`Object.freeze`使得无法添加、删除或修改对象的属性（除非属性的值是另一个对象）。

当我们创建变量`shape`并将其设置为等于冻结对象`box`时，`shape`指向的也是冻结对象。你可以使用`Object.isFrozen`检查一个对象是否被冻结，上述情况，`Object.isFrozen（shape）`将返回`true`。

由于`shape`被冻结，并且`x`的值不是对象，所以我们不能修改属性`x`。 `x`仍然等于`10`，`{x：10，y：20}`被打印。

注意，上述例子我们对属性`x`进行修改，可能会导致抛出TypeError异常（最常见但不仅限于严格模式下时）。

</p>
</details>

---

###### 76. 输出什么?

```javascript
const { name: myName } = { name: "Lydia" };

console.log(name);
```

- A: `"Lydia"`
- B: `"myName"`
- C: `undefined`
- D: `ReferenceError`

<details><summary><b>答案</b></summary>
<p>

#### 答案: D

当我们从右侧的对象解构属性`name`时，我们将其值`Lydia`分配给名为`myName`的变量。

使用`{name：myName}`，我们是在告诉JavaScript我们要创建一个名为`myName`的新变量，并且其值是右侧对象的`name`属性的值。

当我们尝试打印`name`，一个未定义的变量时，就会引发`ReferenceError`。

</p>
</details>

---

###### 77. 以下是个纯函数么?

```javascript
function sum(a, b) {
  return a + b;
}
```

- A: Yes
- B: No

<details><summary><b>答案</b></summary>
<p>

#### 答案: A

纯函数一种若输入参数相同，则永远会得到相同输出的函数。

`sum`函数总是返回相同的结果。 如果我们传递`1`和`2`，它将总是返回`3`而没有副作用。 如果我们传递`5`和`10`，它将总是返回`15`，依此类推，这是纯函数的定义。

</p>
</details>

---

###### 78. 输出什么?

```javascript
const add = () => {
  const cache = {};
  return num => {
    if (num in cache) {
      return `From cache! ${cache[num]}`;
    } else {
      const result = num + 10;
      cache[num] = result;
      return `Calculated! ${result}`;
    }
  };
};

const addFunction = add();
console.log(addFunction(10));
console.log(addFunction(10));
console.log(addFunction(5 * 2));
```

- A: `Calculated! 20` `Calculated! 20` `Calculated! 20`
- B: `Calculated! 20` `From cache! 20` `Calculated! 20`
- C: `Calculated! 20` `From cache! 20` `From cache! 20`
- D: `Calculated! 20` `From cache! 20` `Error`

<details><summary><b>答案</b></summary>
<p>

#### 答案: C

`add`函数是一个记忆函数。 通过记忆化，我们可以缓存函数的结果，以加快其执行速度。上述情况，我们创建一个`cache`对象，用于存储先前返回过的值。

如果我们使用相同的参数多次调用`addFunction`函数，它首先检查缓存中是否已有该值，如果有，则返回缓存值，这将节省执行时间。如果没有，那么它将计算该值，并存储在缓存中。

我们用相同的值三次调用了`addFunction`函数：

在第一次调用，`num`等于`10`时函数的值尚未缓存，if语句`num in cache`返回`false`，else块的代码被执行：`Calculated! 20`，并且其结果被添加到缓存对象，`cache`现在看起来像`{10：20}`。

第二次，`cache`对象包含`10`的返回值。 if语句 `num in cache` 返回`true`，`From cache! 20`被打印。

第三次，我们将`5 * 2`(值为10)传递给函数。 `cache`对象包含`10`的返回值。 if语句 `num in cache` 返回`true`，`From cache! 20`被打印。

</p>
</details>
